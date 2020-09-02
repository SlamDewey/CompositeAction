# CompositeAction
A Composite Action is a set of void delegates that you can invoke in a single call, and can manage easily.
Internally, the `CompositeAction` is just a `HashSet<Action>` with a few helper functions, which makes it very
lightweight, and has no external dependencies.  Inspiration for the CompositeAction came from development in Unity (specifically, the UnityEvent system).
___
### Creating A Composite Action
To create a `CompositeAction`, you simply need to declare it.  The constructor is `private`, so you won't be able
to initialize it.
```c#
public CompositeAction MyCompositeAction;
```
___
### Managing Delegates in the Composite Action
Managing delegates comes in two easily accessible forms.  The `CompositeAction` class has overloaded operators, and so
you can simply add and remove delegates with both `.Add()` and `.Remove()` as well as using `+=` and `-=` operators.

For Example, if we had a random void function like `PrintHello()`:

```c#
private void PrintHello() 
{
    System.Console.WriteLine("Hello");
}
```
We could add this to our Composite Action like so:

```c#
    MyCompositeAction += PrintHello;
    // or
    MyCompositeAction.Add(PrintHello);
```

Similarly, to remove the delegates:
```c#
    MyCompositeAction -= PrintHello;
    // or
    MyCompositeAction.Remove(PrintHello);
```
___
### Invoking the Delegate Set
Invokation of the `CompositeAction` is just as simple, although there is one special recommendation I will make.

To actually invoke the action, you simply call the `Invoke()` method.  **However, the action may be set to `null` if no delegates have been added yet**

Therefore the proper way to invoke any action is by using the `?` operator:
```c#
MyCompositeAction?.Invoke();
```
This way, we don't get any errors when the `CompositeAction` hasn't yet been initialized, but we don't need to waste any memory.