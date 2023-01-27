# Applying Asynchronous Programming in C# 8 / C# 10

This repository contains examples showing use of asynchronous programming in C#.

## Getting Started with Asynchronous Programming in C# using Async and Await

To get started with asynchronous programming in C# you need to understand the following concepts:

> Asynchronous programming is a way of writing code that doesn't block the current thread while waiting for a result.

To use asynchronous programming in C# you need to use the `async` and `await` keywords.

The `async` keyword is used to mark a method as asynchronous.

The `await` keyword is used to wait for an asynchronous operation to complete.

The `async` and `await` keywords are used together to create asynchronous methods.

You must know that `Task` would be returned from an asynchronous method. You can use the `Task` class to represent a single operation that does not return a value. You can use the `Task<T>` class to represent a single operation that returns a value of type `T`. Also in case of an exception inside the asynchronous method, the `Task` or `Task<T>` will rethrow the original exception.

An example of an asynchronous method is shown below:

```csharp
public async Task<string> GetHtml(string url)
{
    using (var client = new HttpClient())
    {
        var html = await client.GetStringAsync(url);
        return html;
    }
}
```

Continuation is a way of chaining asynchronous operations together. It means that you can start an asynchronous operation and then continue with another asynchronous operation when the first one completes.

The `await` keyword is used to wait for an asynchronous operation to complete. It also allows you to continue with another asynchronous operation.

Handling exceptions in asynchronous code is a bit different than handling exceptions in synchronous code. You need to use the `try` and `catch` keywords to handle exceptions in asynchronous code.

Summary:

- always use `async` and `await` together,
- always return a `Task` or `Task<T>` from an asynchronous method,
- always await an asynchronous method to validate the operation,
- use `async` and `await` all the way up the chain,
- never use `async void` unless it's an event handler (and even then, it's not recommended) - if you do exception handling is a nightmare,
- never block an asynchronous operation with `.Result` or `.Wait()` - this will cause a deadlock, you might on the other hand call `.Result` after an `await` statement.

## Using the Task Parallel Library for Asynchronous Programming

> The Task Parallel Library (TPL) is a library that provides support for parallel programming in .NET. It provides a high-level abstraction for parallel programming. It also provides a low-level abstraction for parallel programming.

The Task Parallel Library provides the following features:

- parallel programming model,
- task-based asynchronous programming model,
- dataflow programming model,
- Parallel LINQ (PLINQ) (parallel version of LINQ, created by using the `AsParallel` method),
- parallel collections (such as `ConcurrentDictionary` and `ConcurrentQueue`),
- parallel algorithms (such as `Parallel.For` and `Parallel.ForEach`).

Sometimes you've got a great method (not necessarily asynchronous) that you want to use in an asynchronous way.

You can use the `Task.Run` method to run a method asynchronously.

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
var html = await task;
```

To retrieve the result of a task you can use the `Result` property. This property is a blocking call and will wait for the task to complete.

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
var html = task.Result;
```

We can use `Dispatcher` to run a method on the UI thread.

Example:

```csharp
Dispatcher.Invoke(() => MessageBox.Show("Hello World!"));
```

To wait until a task is completed you can use the `Wait` method.

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
task.Wait();
```

Alternatively, you can use `async` and `await` to wait for a task to complete.

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
await task;
```

To chain asynchronous operations together you can use the `ContinueWith` method (we can also nest asynchronous operations).

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
var continuation = task.ContinueWith(task => Dispatcher.Invoke(() => MessageBox.Show(task.Result)););
```

To check whether a task is completed you can use the `IsCompleted` property.

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
if (task.IsCompleted)
{
    // do something
}
```

To verify whether a task has completed successfully you can use the `IsFaulted` property.

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
if (task.IsFaulted)
{
    // do something
}
```

To continue only if a task has completed successfully you can use the `ContinueWith` method.

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
var continuation = task.ContinueWith(task => Dispatcher.Invoke(() => MessageBox.Show(task.Result)), TaskContinuationOptions.OnlyOnRanToCompletion);
```

To log exceptions in a task you can use the `ContinueWith` method.

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
var continuation = task.ContinueWith(task => Log(task.Exception), TaskContinuationOptions.OnlyOnFaulted);
```

If we're running a long-running task we might want to cancel it. We can use the `CancellationToken` class to cancel a task.

Example:

```csharp
var cancellationTokenSource = new CancellationTokenSource();
var cancellationToken = cancellationTokenSource.Token;
var task = Task.Run(() => GetHtml("https://www.google.com", cancellationToken));
cancellationTokenSource.Cancel();
```

We can do something similar with `HttpClient`.

Example:

```csharp
var cancellationTokenSource = new CancellationTokenSource();
var cancellationToken = cancellationTokenSource.Token;
var client = new HttpClient();
var task = client.GetStringAsync("https://www.google.com", cancellationToken);
cancellationTokenSource.CancelAfter(1000);
```

To verify that a task was cancelled we can use the `IsCancellationRequested` property.

Example:

```csharp
var cancellationTokenSource = new CancellationTokenSource();
var cancellationToken = cancellationTokenSource.Token;
var client = new HttpClient();
var task = client.GetStringAsync("https://www.google.com", cancellationToken);
cancellationTokenSource.CancelAfter(1000);
if (cancellationToken.IsCancellationRequested)
{
    // do something
}
```

We can check if cancellation was requested in a task by using the `ThrowIfCancellationRequested` method, we can do that inside `Task.Run`.

Example:

```csharp
var cancellationTokenSource = new CancellationTokenSource();
var cancellationToken = cancellationTokenSource.Token;
var task = Task.Run(async () =>
{
    var result = await GetHtml("https://www.google.com");

    if (cancellationToken.IsCancellationRequested)
    {
        cancellationToken.ThrowIfCancellationRequested();
    }

    // do something with result

    return result;
}, cancellationToken);
cancellationTokenSource.CancelAfter(1000);

if (cancellationToken.IsCancellationRequested)
{
    // do something
}
```

We can use task cancellation token and task continuation options to cancel a task. But if you do that, you must also specify the `TaskScheduler` to use.

Example:

```csharp
var cancellationTokenSource = new CancellationTokenSource();
var cancellationToken = cancellationTokenSource.Token;
var task = Task.Run(() => GetHtml("https://www.google.com"), cancellationToken);
var continuation = task.ContinueWith(task =>
    Dispatcher.Invoke(() => MessageBox.Show(task.Result)),
    cancellationToken,
    TaskContinuationOptions.OnlyOnRanToCompletion,
    TaskScheduler.Current);
cancellationTokenSource.Cancel();
```

It is possible to register a callback that will be called when a task is cancelled.

Example:

```csharp
var cancellationTokenSource = new CancellationTokenSource();
var cancellationToken = cancellationTokenSource.Token;
var task = Task.Run(() => GetHtml("https://www.google.com"), cancellationToken);
cancellationToken.Register(() => MessageBox.Show("Task was cancelled"));
cancellationTokenSource.Cancel();
```

Summary:

- a `Task` represents a single asynchronous operation,
- implement two versions of a method if you need both synchronous and asynchronous versions,
- do not wrap the synchronous method in a `Task.Run` just to make the method asynchronous, copy the code instead and implement the asynchronous version,
- be aware that `await` would continue on the same thread if the task is already completed, but `ContinueWith` would continue on a different thread.

## Exploring Useful Methods in the Task Parallel Library

If we need to know whether all tasks or any task has completed we can use the `Task.WhenAll` and `Task.WhenAny` methods.

Example:

```csharp
var tasks = new List<Task<string>>();
for (var i = 0; i < 10; i++)
{
    tasks.Add(Task.Run(() => GetHtml("https://www.google.com")));
}

var completedTask = await Task.WhenAny(tasks);
var completedTaskResult = completedTask.Result;
```

If we need to wait for all tasks to complete we can use the `Task.WhenAll` method.

Example:

```csharp
var tasks = new List<Task<string>>();
for (var i = 0; i < 10; i++)
{
    tasks.Add(Task.Run(() => GetHtml("https://www.google.com")));
}

await Task.WhenAll(tasks);
```

If we need to be sure that all tasks are completed within the given timeout we can use the `Task.WhenAll` and `Task.Delay` methods.

Example:

```csharp
var cancellationTokenSource = new CancellationTokenSource();
var cancellationToken = cancellationTokenSource.Token;

var tasks = new List<Task<string>>();
for (var i = 0; i < 10; i++)
{
    tasks.Add(Task.Run(() => GetHtml("https://www.google.com")), cancellationToken);
}

var timeoutTask = Task.Delay(1000);
var completedTask = await Task.WhenAny(tasks.Concat(new[] { timeoutTask }));
if (completedTask == timeoutTask)
{
    cancellationTokenSource.Cancel();
    throw new OperationCanceledException("Timeout!");
}
```

Precomputed results of a task can be used to start another task. Sometimes we don't want to call an API, but
we want to use a mock result. We can use the `Task.FromResult` method to create a task that is already completed.

Example:

```csharp
var task = Task.FromResult("Hello World!");
```

There is also `Task.CompletedTask` that is a task that is already completed, also there is `Task.FromCanceled` and `Task.FromException`.

To process tasks results as they complete we can use the `Task.WhenAny` method.

Example:

```csharp
var tasks = new List<Task<string>>();
for (var i = 0; i < 10; i++)
{
    tasks.Add(Task.Run(() => GetHtml("https://www.google.com")));
}

while (tasks.Count > 0)
{
    var completedTask = await Task.WhenAny(tasks);
    tasks.Remove(completedTask);
    var result = completedTask.Result;
    // do something with result
}
```

To work with TPL we should use concurrent collections. There are two types of concurrent collections: thread-safe and lock-free. The thread-safe collections are `ConcurrentQueue`, `ConcurrentStack`, `ConcurrentBag`, `ConcurrentDictionary`, `BlockingCollection`. The lock-free collections are `ConcurrentQueue`, `ConcurrentStack`, `ConcurrentBag`, `ConcurrentDictionary`.

Example:

```csharp
var concurrentQueue = new ConcurrentQueue<string>();
concurrentQueue.Enqueue("Hello");
concurrentQueue.Enqueue("World");
concurrentQueue.TryDequeue(out var result);
```

To create a producer-consumer pattern we can use the `BlockingCollection` class.

Example:

````csharp
var blockingCollection = new BlockingCollection<string>();
var producer = Task.Run(() =>
{
    while (true)
    {
        var result = GetHtml("https://www.google.com");
        blockingCollection.Add(result);
    }
});

Execution context and continuation options can be used to control the execution of a continuation.

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
var continuation = task.ContinueWith(task =>
    Dispatcher.Invoke(() => MessageBox.Show(task.Result)),
    TaskContinuationOptions.OnlyOnRanToCompletion);
````

But what is an execution context?

> It is a set of information that is associated with a task. It includes the task scheduler, the synchronization context, and the security context. The `TaskScheduler` is used to schedule the task, the `SynchronizationContext` is used to marshal the continuation to the correct thread, and the `SecurityContext` is used to set the security context for the continuation.

The `ConfigureAwait` is a method that can be used to configure the `await` operator. It can be used to specify whether the continuation should be scheduled on the current synchronization context or not.

Example:

```csharp
var task = Task.Run(() => GetHtml("https://www.google.com"));
var result = await task.ConfigureAwait(false);
```

Summary:

## Async and Await Advanced Topics and Best Practices

Summary:

## Asynchronous Programming Deep Dive

Summary:

Now you know how to use asynchronous programming in C# :-) (at least the basics).
