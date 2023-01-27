# Applying Asynchronous Programming in C# 8 / C# 10

This repository contains examples showing use of asynchronous programming in C#.

## Getting Started with Asynchronous Programming in C# using Async and Await

Summary:

- always use `async` and `await` together,
- always return a `Task` or `Task<T>` from an asynchronous method,
- always await an asynchronous method to validate the operation,
- use `async` and `await` all the way up the chain,
- never use `async void` unless it's an event handler (and even then, it's not recommended) - if you do exception handling is a nightmare,
- never block an asynchronous operation with `.Result` or `.Wait()` - this will cause a deadlock, you might on the other hand call `.Result` after an `await` statement.

## Using the Task Parallel Library for Asynchronous Programming

Summary:

## Exploring Useful Methods in the Task Parallel Library

Summary:

## Async and Await Advanced Topics and Best Practices

Summary:

## Asynchronous Programming Deep Dive

Summary:

Now you know how to use asynchronous programming in C# :-) (at least the basics).
