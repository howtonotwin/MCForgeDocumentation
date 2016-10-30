Creating Events
===============

Anyone can create a new event, which will act just like every other event and will work with matching event handlers out of the box. There is no "registry"-like object that controls events. Instead, any instance of `Event` can be passed into `EventBus::post`, and any event handler that accepts that event will receive it.

Therefore, to create a new event, simply create a new class that extends `Event` or one of its subclasses. For example, the following class is a `PlayerEvent` that is fired every time the player "foos the bar," with an extra field `fooage` that represents how much foo should be applied:

```java
public class PlayerFooBarEvent extends PlayerEvent {
    private float fooage;

    public PlayerFooBarEvent(EntityPlayer player, float fooage) {
        super(player);
        this.fooage = fooage;
    }

    public float getFooage() { return fooage; }
    public void setFooage(float newFooage) { fooage = newFooage; }
}
```

The event handler for this event is just like any other event handler, where the declaration looks something like this: `@SubscribeEvent public void onPlayerFooBar(PlayerFooBarEvent pfbe)`.

In order for any of this to be actually useful, something has to actually fire the `PlayerFooBarEvent`. This is done by passing it to `EventBus::post`. Note that an event isn't explicitly tied down to a single event bus; it applies to any event busses where it is fired. Most events will be on `MinecraftForge.EVENT_BUS`.

Cancelable Events
-----------------

An event may be marked `@Cancelable`. This means that the `setCanceled` method can be used to (un)cancel the event. The return value from `EventBus::post` represents whether the event has been allowed to continue or not, and `true` is returned for events that aren't cancelable.

Events with Results
-------------------

An event may be marked `@HasResult`. This means that the event can have one of three results, `DENY`, `DEFAULT`, and `ALLOW`. The result can be set with `setResult` and gotten with `getResult`.

Generic Events
--------------

Events may be filtered by a generic parameter if they implement `IGenericEvent`. Implementors of this interface support getting the `Type` associated with each instance of the event, and the event handlers are filtered by getting the type of the parameter through reflection. This matching is invariant, even in the presence of wildcards (e.g. `RegistryEvent.Register<?>` does not match the `RegisterEvent.Register<Block>` event when it is fired).

Context Setting Events
----------------------

All of an event's handlers run in the same mod context as the code that fired it. This means that, for example, every time Forge fires the registry events they would normally all execute with the active mod container set to that of Forge. This has the unpleasant consequence of making anything that depends on the current mod container (e.g. `setRegistryName(String)` takes the mod container and uses its modid as the domain of the registry name) malfunction. `IContextSetter` is marker interface that enables a way to fix this issue at the cost of performance. If an event implements this interface, whenever an event handler is called to handle the event, the mod owning the handler is set as the active mod container before the actual handler runs.
