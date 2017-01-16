Resources
=========

A resource is extra data used by the game, and is stored in a data file, instead of being in the code. Resources are contained in the `assets` directory on the classpath. In the default MDK, this directory is under the `src/main/resources` directory of the project. They include things such as models, textures, and localization files.

When multiple resource packs are enabled, they are merged. Generally, files from resource packs at the top of the stack override those below; however, for certain files, such as localization files, they are actually merged contentwise. Mods actually define resource packs too, and their resources are simply merged with the default resource pack inside the core Minecraft JAR in the same way as any other.

All resources should have snake_case paths; this is enforced. This casing convention is used even outside the resource system.

`ResourceLocation`
------------------

Minecraft identifies resources using `ResourceLocation`s (RLs). An RL contains two parts: a domain and a path. It generally points to the resource at `assets/<domain>/<ctx>/<path>`, where `ctx` is a context-specific path fragment that depends on how the RL is being used. When an RL is written/read as/from a string, it is seen as `<domain>:<path>`. If the domain and the colon are left out, then when the string is read into an RL the domain will normally default to `"minecraft"`. A mod should put its resources into a domain with the same name as its modid (E.g. a mod with id `examplemod` should place its resources in `assets/examplemod`, and RLs pointing to those files would look like `examplemod:<path>`.). RLs are used outside the resource system, too, as they happen to be a great way to uniquely identify objects.

Wherever a `ResourceLocation` is used, it should almost always be in snake_case.
