# For Mod Developers

You do **not** need to depend on Pipster for items, fluids or energy. If your blocks offer the
standard interfaces of the loader (see [Resources and Compatibility](Resources-and-Compatibility)),
Pipster pipes connect to them.

The Pipster API is only needed for:

* **gas** (`pipster:gas`), because neither loader has a gas interface, and
* **endpoint providers**, to offer gas handlers for blocks you cannot change.

The complete reference, with code examples, contracts and Maven coordinates, is in the
repository: **[API for mod developers](https://github.com/CptGummiball/Pipster/blob/main/api/README.md)**.

## In short

* Get `pipster-api-<version>.jar` from the
  [releases](https://github.com/CptGummiball/Pipster/releases) and add it as a **compile-only**
  dependency. Never ship Pipster files in your own mod.
* Register a gas: `GasRegistry.register(new GasType("examplemod:steam", "mB"));`
* Offer a `GasHandler` on your block entity through the lookup/capability `pipster:gas_handler`.
* `extract` must remove only exactly the requested gas. With `simulate == true` nothing may change.
* If Pipster is optional for your mod, check that the mod `pipster` is loaded before touching its
  classes.

The [license](https://github.com/CptGummiball/Pipster/blob/main/LICENSE) (section 5) allows you
to compile against the API and publish mods that use it.
