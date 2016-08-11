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
|     `setMaxDamage`     | Sets the maximum damage value for this item. If it's over `0`, 2 item properties "damaged" and "damage" are added. |
|    `setMaxStackSize`   | Sets the maximum stack size.                  |
|      `setNoRepair`     | Makes this item impossible to repair, even if it is damageable. |
|  `setUnlocalizedName`  | Sets this item's unlocalized name, with "item." prepended. |
|    `setHarvestLevel`   | Adds or removes a pair of harvest class (`"shovel"`, `"axe"`) and harvest level. This method is not chainable. |

The above methods are chainable, unless otherwise stated, meaning they `return this` to facilitate calling them in series.

### Advanced Items

Setting the properties of an item as above only works for very simple items. If you want more complicated items, you should subclass Item and override its methods.

Registering an Item
-------------------

Now that you have created an item, it is time to register it. As with most things you register an Item by passing it to `GameRegistry.register`. Before you register an item, you must also set its registry name. To set the registry name, simply call `setRegistryName` on the item. You may also pass a `ResourceLocation` directly to `GameRegistry.register`, but this is just shorthand for calling `setRegistryName` beforehand.

!!! note
    When you set the registry name with a simple string like `"myItem"`, the active modid is prefixed onto it. So if examplemod was registering this item, the actual name will be `"examplemod:myItem"`.
