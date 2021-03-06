# Command Values

> **Warning** This chapter describes ReactiveCommand implementation for future version of ReactiveUI (7.0). For documentation of the current version (part of ReactiveUI 6.5), consult the main repository docs: [ReactiveCommand](https://github.com/reactiveui/ReactiveUI/blob/master/docs/basics/reactive-command.md) and [Asynchronous operations with ReactiveCommand](https://github.com/reactiveui/ReactiveUI/blob/master/docs/basics/reactive-command-async.md). 

The execution of a command produces a value, which is captured as `TResult` in `ReactiveCommand<TParam, TResult>`. If you don't need to return anything special, you can just use `Unit` for `TResult`. Indeed, this is what will happen automatically if you use `Create*` overloads that don't return values:

```cs
// a synchronous command that does not return an interesting result
var command1 = ReactiveCommand.Create(() => { });

// an observable-based asynchronous command that does not return an interesting result
var command2 = ReactiveCommand.CreateFromObservable(() => Observable.Return(Unit.Default));

// a Task-based asynchronous command that does not return an interesting result
var command3 = ReactiveCommand.CreateFromTask(async () => await Task.Delay(TimeSpan.FromSeconds(2)));
```

All the above commands are of type `ReactiveCommand<Unit, Unit>`. Note that `CreateFromObservable` is required to eventually return an `IObservable<T>`, so `T` must be known. Therefore, to get a `ReactiveCommand<Unit, Unit>` from `CreateFromObservable`, you must ensure your observable is of type `IObservable<Unit>`.

If you _do_ want to return something interesting each time your command executes, you need only use the appropriate `Create*` method:

```cs
// a synchronous command that always returns 42 upon execution
var command1 = ReactiveCommand.Create(() => 42);

// an observable-based asynchronous command that always returns 42 upon execution
var command2 = ReactiveCommand.CreateFromObservable(() => Observable.Return(42));

// a Task-based asynchronous command that always returns 42 upon execution
var command3 = ReactiveCommand.CreateFromTask(() => Task.FromResult(42));
```

Here, all commands are of type `ReactiveCommand<Unit, int>`. Subscribing to the observable return by `Execute` will tick through the value `42`.
