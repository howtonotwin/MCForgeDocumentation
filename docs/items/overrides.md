Item Property Overrides
=======================

Items do not have something analogous to blockstate JSONs, where you can define models in `models/block/` and then stitch them into one big model in a blockstate JSON under `blockstates/` depending on variants. Instead, you must assign an item a single model defined under `models/item/`. Then, the item will expose certain properties from the code. These properties are defined as a name string and a function that maps `ItemStack`s to arbitrary float values. The model JSON defines the default model for the item, and has an extra `"overrides"` tag, which is a list of predicates and a model to use if the predicate is true. The format can be seen on the [wiki][].

Adding Properties to Items
--------------------------

To add a property to an `Item`, simply call `addPropertyOverride` on it. The `ResourceLocation` parameter is simply the name given to the property (e.g. `new ResourceLocation("pull")`). The `IItemPropertyGetter` is a function that takes the `ItemStack` for which we're looking for a model, the `World` it's in, and the `EntityLivingBase` that holds it, returning the `float` value for the property. Some examples are the `"pulling"` and "`pull`" properties in `ItemBow`, and the several `static final` ones in `Item`.

Using Overrides
---------------

The format of an override can be seen on the [wiki][], and a good example can be found in `model/item/bow.json`. For reference, here is a hypothetical example of an item with a `power` property and a `usage` property.

    {
      "parent": "item/generated",
      "texture": {
        "layer0": "examplemod:items/exampleUsedEmpty" // Default
      },
      "overrides": [
        {
          "predicate": { // power >= .25 && usage >= .9
            "power": .25,
            "usage": .9
          },
          "model": "examplemod:item/exampleUsedPowered"
        }
        {
          "predicate": { // power >= 0 && usage >= .9
            "power": 0,
            "usage": .9
          },
          "model": "examplemod:item/exampleUsedEmpty",
        },
        {
          "predicate": { // power >= 0 && usage >= 0
            "power": .25,
            "usage": 0
          },
          "model": "examplemod:item/exampleUnusedPowered",
        }
      ]
    }

[wiki]: http://minecraft.gamepedia.com/Model#Item_models
