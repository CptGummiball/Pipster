# Building and testing Pipster

## Requirements

* JDK 25 (Gradle daemon JVM; required by Fabric Loom 1.18) and JDK 21 (compile target of the
  1.21.x families). Gradle toolchains detect both; `gradle/gradle-daemon-jvm.properties` selects 25.
* Python 3 with Pillow, only to regenerate assets/data (`tools/assetgen`).
* Network access to Maven Central, maven.fabricmc.net and maven.neoforged.net.

## Layout

| Path | Content |
|---|---|
| `api/` | public, Minecraft-free integration API (Java 21) |
| `core/` | Minecraft-free transport core (graph, budgets, transfers, filters, tesseract ACL), unit + property tests |
| `platform/mc1211/{common,fabric,neoforge}` | family A (Minecraft 1.21.1), single version |
| `platform/mc1212/{common,fabric,neoforge}` | band B–C (Minecraft 1.21.2–1.21.4), Java 21, one version per build |
| `platform/mc1215/…`, `mc1216/…`, `mc1219/…`, `mc12111/…` | families D (1.21.5), E (1.21.6–1.21.8), F (1.21.9–1.21.10), G (1.21.11), Java 21, one version per build |
| `platform/mc26/{common,fabric,neoforge}` | band H–J (Minecraft 26.1–26.3), Java 25, one version per build |
| `platform/<band>/versions/<mc>.properties` | pins and version-local source/resource variants of one game version |
| `tools/assetgen/` | deterministic texture/model/data/lang generator |
| `docs/evidence/` | boot logs, screenshots, benchmark reports produced by the tools below |

Every profile is its own Gradle build (`./gradlew -p platform/<profile> ...`). Band profiles build
exactly one game version per invocation, selected with `-Ppipster.mc=<version>` (default:
`default_minecraft` in the profile's `gradle.properties`). Outputs go to `build/<mc>/` and run
directories to `run/<mc>/`, so artifacts of different versions never mix. Each version is built,
booted and tested separately. A jar lists exactly the one version it was tested on.

## Commands

```bash
./gradlew build                                   # api + core, incl. 10,000-seed property test
./gradlew build -Ppipster.seeds=100000            # more seeds
./gradlew -p platform/mc1211 build checkNoClientRefs
./gradlew -p platform/mc1211 :fabric:runGametest          # headless GameTests (Fabric)
./gradlew -p platform/mc1211 :neoforge:runGameTestServer  # headless GameTests (NeoForge)
tools/bootcheck.sh mc1211 fabric server           # dedicated-server boot evidence
tools/bootcheck.sh mc1211 neoforge client         # client boot evidence (opens a window)
./gradlew -p platform/mc1211 :fabric:runClient -Ppipster.showcase=true   # 64-mask screenshots
tools/bench.sh mc1211 fabric idle10k active10k active50k rebuild        # benchmarks (long)
./gradlew -p platform/mc26 -Ppipster.mc=26.2 build checkNoClientRefs     # one band version
./gradlew -p platform/mc26 -Ppipster.mc=26.2 :fabric:runGametest
tools/bootcheck.sh mc26 neoforge server 26.2       # band profiles take the version as 4th argument
MC=26.2 tools/bench.sh mc26 fabric idle10k         # band benchmark
python tools/assetgen/generate.py --family mc1211 # regenerate committed assets/data (also: mc1212, mc1215, mc1216, mc1219, mc12111, mc26)
python tools/apicheck.py <classes> <target-jar[@mappings]> --base=<source-jar>  # port planning, see docs/PORTING.md
python tools/check_artifacts.py                   # author/id/version metadata of built jars
```

Dedicated-server runs need `eula=true` in the run directory.

## Artifacts

`platform/mc1211/<loader>/build/libs/pipster-<version>-mc1.21.1-<loader>.jar` and
`platform/<band>/<loader>/build/<mc>/libs/pipster-<version>+mc<mc>-<loader>.jar`. Fabric and
NeoForge always get separate jars. A jar lists exactly the Minecraft version it was tested on.

## CI order

format/static checks → unit/property tests → datagen diff → loader builds → dedicated boot and
GameTests → artifact metadata check (`.github/workflows/ci.yml`). Checks that did not actually
run are reported as *not run*, never as passed.
