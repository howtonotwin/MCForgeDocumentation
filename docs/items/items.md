Items
=====

Along with blocks, items are a key component of most mods. While blocks make up the world around you, items are what let you change it.

Creating an Item
----------------

### Basic Items

Basic items that need no special functionality (think sticks or sugar) don't need custom classes. You can simply instantiate `Item` and call its various setters to set some simple properties.

|         Method         |                  Description                  |
|:----------------------:|:----------------------------------------------|
|    `setCreativeTab`    | Sets which creative tab this item is under. Must be called if this item is meant to be shown on the creative menu. Vanilla tabs can be found in the class `CreativeTabs`. |
|     `setMaxDamage`     | Sets the maximum damage value for this item. If it's over `0`, 2 [item properties][override] "damaged" and "damage" are added. |
|    `setMaxStackSize`   | Sets the maximum stack size.                  |
|      `setNoRepair`     | Makes this item impossible to repair, even if it is damageable. |
|  `setUnlocalizedName`  | Sets this item's unlocalized name, with "item." prepended. |
|    `setHarvestLevel`   | Adds or removes a pair of harvest class (`"shovel"`, `"axe"`) and harvest level. This method is not chainable. |

The above methods are chainable, unless otherwise stated, meaning they `return this` to facilitate calling them in series.

### Advanced Items

Setting the properties of an item as above only works for very simple items. If you want more complicated items, you should subclass Item and override its methods.

Registering an Item
-------------------

### The Item Itself

Now that you have created an item, it is time to register it. As with most things you register an Item by passing it to `GameRegistry.register`. Before you register an item, you must also set its registry name. To set the registry name, simply call `setRegistryName` on the item. You may also pass a `ResourceLocation` directly to `GameRegistry.register`, but this is just shorthand for calling `setRegistryName` beforehand.

!!! note

    When you set the registry name with a simple string like `"myItem"`, the active modid is prefixed onto it. So if examplemod was registering this item, the actual name will be `"examplemod:myItem"`.

### The Item's model

Unlike blocks, which are automatically bound to their blockstate files, items must have their models explicitly registered. This holds true even for `ItemBlock`s. This registering can only happen on the clientside, so make sure you only call this code through your client proxy.

#### Simple Mappings from Meta to Model

The simplest item model registering happens through `ModelLoader.setCustomModelResourceLocation`. This method registers a mapping from an item and a metadata to a `ModelResourceLocation`. A `ModelResourceLocation` is the combination of a `ResourceLocation` (like `examplemod:foo`) with a variant string (like `variant=iron`). For a blockstate JSON, we can construct an RL for it, and then an MRL refers to a certain variant within that JSON. As well as registering the mapping, the method also calls `ModelBakery.registerItemVariants`

When you register an MRL for an item, first the game will try to load the variant from the blockstate JSON that corresponds to the `ResourceLocation`. The blockstate JSON need not actually be used by a block, blockstate JSONs are equally valid for *both* items and blocks. If it fails, the variant string `inventory` is specialcased by vanilla to use the item model in `models/item/`. 
#### `ItemMeshDefinition`

It is also possible to use arbitrary code to map `ItemStack`s to `ModelResourceLocations`. An `ItemMeshDefinition` is a function that maps `ItemStack`s to `ModelResourceLocation`s. You can register an IMD for a certain item through `ModelLoader.setCustomMeshDefinition`. However, when you do so, `ModelBaker.registerItemVariants` is not called. Therefore you must manually register the MRLs that your IMD can return as well as registering the IMD itself.

[override]: overrides
