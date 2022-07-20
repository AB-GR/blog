---
author: "Abhilash"
title: "Rate Limiting in .NET 7"
date: "2022-07-19"
description: "Sumarizing Rate Limiting in .NET 7"
tags: ["asp.net core", "rate-limiting"]
ShowToc: false
ShowBreadCrumbs: false
draft: false
---

### Source Article(s)
https://devblogs.microsoft.com/dotnet/announcing-rate-limiting-for-dotnet/

### Concept of rate limiting
Rate limiting is all about controlling access to a resource i.e how much can it be accessed.

### Multiple algorithms
Concurrent Rate Limiting : Put a limit on concurrent access to a resource, say no more than 10 requests be allowed concurrently then the 11th concurrent request will be denied or put in a queue.

Token Bucket : Consider a fixed no of tokens being given out per unit of time, like we have 5 tokens per minute, so at the beginning if we have 3 requests within a minute then only 2 more requests are to be allowed within that minute but after a minute 5 more requests can be allowed as the no of tokens got replenished.

Fixed Window : Only x no of requests allowed in a time interval of T, say only 5 requests every minute, very similar to token bucket except for the terminology and both suffer from [request burst at the edges problem](https://stackoverflow.com/q/65868274)

Sliding Window : Instead of keeping the window fixed like 1 minute, how about dividing it further into segments, so 1 minute window can be further divided into 3 20 sec segments. This is not the only difference with the above but the way the window moves
So initial window will be from 0 sec to 60 sec with segements (0-20), (21-40), (41,60)
when this window slides, the segements move to (21-40), (41,60), (61-80) sec intervals.
This avoids the above burst problem where you are further constraining and distributing the requests over finer time intervals.

More on this [topic](https://dev.to/satrobit/rate-limiting-using-the-token-bucket-algorithm-3cjh)

### In .NET 7
API's reside in the [RateLimiting](https://www.nuget.org/packages/System.Threading.RateLimiting) package.
The main type is the abstract base class RateLimiter.
```
public abstract class RateLimiter : IAsyncDisposable, IDisposable
{
    public abstract int GetAvailablePermits();
    public abstract TimeSpan? IdleDuration { get; }

    public RateLimitLease Acquire(int permitCount = 1);
    public ValueTask<RateLimitLease> WaitAsync(int permitCount = 1, CancellationToken cancellationToken = default);

    public void Dispose();
    public ValueTask DisposeAsync();
}
```
`Acquire` is a synchronous method that will check if enough permits are available or not and return a `RateLimitLease` which contains information about whether you successfully acquired the permits or not.

`WaitAsync` is similar to Acquire except that it can support queuing permit requests which can be de-queued at some point in the future when the permits become available, which is why it’s asynchronous and accepts an optional `CancellationToken` to allow canceling the queued request.

There is the `ConcurrencyLimiter`, there are 3 other limiters provided in-box. `TokenBucketRateLimiter`, `FixedWindowRateLimiter`, and `SlidingWindowRateLimiter` all of which implement the abstract class `ReplenishingRateLimiter` which itself implements `RateLimiter`.

`PartitionedRateLimiter<TResource>` is an abstraction that is very similar to the `RateLimiter` class except that it accepts a `TResource` instance as an argument to methods on it. For example `Acquire` is now: `Acquire(TResource resourceID, int permitCount = 1)`. This is useful for scenarios where you might want to change rate limiting behavior depending on the `TResource` that is passed in.

`RateLimiting middleware` is provided via the `Microsoft.AspNetCore.RateLimiting` NuGet package. The main usage pattern is to configure some rate limiting policies and then attach those policies to your endpoints.