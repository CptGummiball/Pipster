# FAQ

### My pipe is connected, but nothing moves.

New ports start in **Insert** mode (network → block). At least one port must be set to
**Extract** or **Both** so resources can enter the network. Open the port with the Conduit
Wrench; the status line tells you what is missing ("Source empty", "No targets", "Filter
blocks", ...).

### My generator on Fabric does not fill the cable.

Fabric energy is pushed by the generator. Set the cable's port at the generator to **Extract**
or **Both**, then the push is accepted.

### Why is my Quantum line so slow?

Throughput is limited per pipe segment. One lower-tier segment somewhere in the path limits the
whole path. Upgrade it with a bolt.

### Can I put different pipe kinds in one block?

No. One block holds one pipe kind. Place pipes of different kinds next to each other; they do
not connect to each other.

### I broke a pipe and my items are gone!

They are inside the dropped pipe item. The tooltip says "Contains buffered resources". Place
the pipe again to release them.

### Why can't I craft my pipes into the next tier?

Pipes that still hold buffered content are rejected by the upgrade recipes, so crafting never
deletes resources. Place them to empty them first, or upgrade placed pipes with a bolt.

### My tesseract says "offline".

Either the chunk of another member is not loaded, you lost access to the channel, or the
channel was deleted. Tesseracts never load chunks on their own.

### Can I send items to another dimension?

Yes, with Quantum Tesseracts on both ends. Resonant Tesseracts only work within one dimension.

### Do Mekanism gases work in gas pipes?

Not yet. See [Resources and Compatibility](Resources-and-Compatibility).

### Does Pipster work with mod X?

If mod X offers the standard item, fluid or energy interfaces of your loader, very likely yes.
Pipster's automated tests do not include other mods, so please report problems.

### Is there a single jar for all versions?

No, on purpose. Each jar is built and tested for exactly one Minecraft version and one loader.
See [Supported Versions](Supported-Versions).

### Can I use Pipster in my modpack?

Yes. Anyone may use Pipster in a modpack, public or private, free or paid, and ship the Pipster
jar with it, as long as the jar files stay exactly as published. Pipster is not open source:
modifying, decompiling or re-uploading it outside a modpack is not allowed. See the
[license](https://github.com/CptGummiball/Pipster/blob/main/LICENSE).
