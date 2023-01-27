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
public async Task<string> GetHtmlAsync(string url)
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

The Task Parallel Library (TPL) is a library that provides support for parallel programming in .NET. It provides a high-level abstraction for parallel programming. It also provides a low-level abstraction for parallel programming.

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

Summary:

## Exploring Useful Methods in the Task Parallel Library

Summary:

## Async and Await Advanced Topics and Best Practices

Summary:

## Asynchronous Programming Deep Dive

Summary:

Now you know how to use asynchronous programming in C# :-) (at least the basics).
