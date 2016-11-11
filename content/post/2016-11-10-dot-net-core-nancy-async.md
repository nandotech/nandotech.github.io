---
title: Exploring Asynchronous Modules with NancyFX 2.0 & .NET Core
subtitle: Plus, using the Before & After hooks built into Nancy Request Pipeline.
date: 2016-11-10
bigimg: /img/nancyhome.png
---

## This post will serve as a follow-up to our earlier NancyFX exploration.  A few weeks ago, I posted [Building an (awesome) API with NancyFX 2.0 + Dapper](/post/2016-10-25-nancyfx-webapi-dapper/) where we covered all the basics of [Nancy](https://www.nancyfx.org) and running on [.NET Core](https://dot.net).

If you have not [read our previous post](/post/2016-10-25-nancyfx-webapi-dapper/), not to worry, there will be very little to no overlap between the 2 posts.  Here, we will focus in on and deep dive into the use of `Async` Modules in _NancyFX_ as well as how the `Before` and `After` action method hooks work.  Since there is not much additional code, we will use the same repository to demonstrate usage.

### Github Repo: [https://github.com/nandotech/VSNancyDemo](https://github.com/nandotech/VSNancyDemo)

If you are unfamiliar with [.NET Core](http://dot.net) or [Nancy](http://www.nancyfx.org), then I do reccomend checking out some of the other posts available on this blog related to .NET Core and specifically our post on [Building an API w/ NancyFX 2.0 & Dapper](/post/2016-10-25-nancyfx-webapi-dapper/).

Unfortunately, there are not a ton of resources covering NancyFX 2.0 just yet, so here I am doing my part. [Scott Hanselman](https://twitter.com/shanselman) recently also wrote a similar post (but using  [Entity Framework Core 1.0](https://docs.efproject.org)) available [here](http://www.hanselman.com/blog/ExploringAMinimalWebAPIWithNETCoreAndNancyFX.aspx). 

The Nancy [repository](https://github.com/nancyfx/nancy) has a ton of [samples](https://github.com/NancyFx/Nancy/tree/master/samples) including one showing off `Async`: [Nancy.Demo.Async](https://github.com/NancyFx/Nancy/tree/master/samples/Nancy.Demo.Async).  Once again, very unfortunately, the grand majority of what you see is syntax for version `1.x` and therefore does not help us.

If you've read this far along, I'm sure you're ready to get down and dirty.

[NancyFX makes it incredibly easy to declare async modules](https://github.com/NancyFx/Nancy/tree/master/samples/Nancy.Demo.Async), making the `Modules` appear nearly indistinguishable from any other Nancy Module. That previous link covers all the basics, however using version `1.x` style route declarations. The announcement and wiki detailing the [Nancy v2 upgrade notes](https://github.com/NancyFx/Nancy/wiki/Nancy-v2-Upgrade-Notes) explain some of the differences between `1.x` and `2.x` as of now.

_AsyncModule.cs_

```csharp
public class AsyncModule : NancyModule
    {
        public AsyncModule()
        {
            Get("/async", async (args, ct) =>
            {
                await Task.Delay(1000);

                await Task.Delay(1000);

                var client = new HttpClient();
                var res = await client.GetAsync("http://nancyfx.org");
                var content = await res.Content.ReadAsStringAsync();

                return (Response)content;
            });
            
        }
    }
```

While extremely contrived, the above example shows how to declare `Async Modules`, `await` several responses and then return our `content` in the from of a string. That is, if you are following along and have entered the above as a `Module` in your Nancy project, you should see this:

![NancyFX Async Module](http://i.imgur.com/A9TJowS.png)

Upon closer inspection, that is clearly the plain `HTML` from [http://www.nancyfx.org](http://www.nancyfx.org).  Nonetheless, the point here was to highlight the proper syntax for an asynchronous `Nancy Module` and demonstrate using `await` to simulate some possibly long-running process.

Speaking of asynchronicity, _NancyFX_ also introduces an incredibly useful tool in the `Before` and `After` Request Hooks that allow you to define functions and/or code that you would like to run either before or after any request in the entire `Module`.  These are also known as the `BeforePipeline` and `AfterPipeline`--the final one, which we will not cover here, is the `OnError` pipeline.

These pipelines for requests provide us with a lot of lattitude and the possibility of causing all types of different behaviors. As an example, if you have authentication/authorization on your application and there are only specific spots with certain priviledges: it is perfectly feasible and would make sense to add some type of authenticator that could run in the `Before` method of any `Module`.  By the same token, you are capable of completely intercepting a request from either the `Before` or `After` module (or allowing `OnError` to execute, if there were errors, let's say).

To illustrate, below is our _DispoModule.cs_

```csharp
public class DispoModule : NancyModule
    {
        public DispoModule(IDispoRepository _repo)
            : base("/dispo")
        {
            Get("/", args =>
            {
                return _repo.GetAll();
            });

            Get("Id={id}", args =>
            {
                return _repo.Get(args.id);
            });

            Post("/Name={name}&Desc={description}", args =>
            {
                var posted = new Disposition();
                posted.Name = args.Name;
                posted.Description = args.Description;
                posted.Timestamp = DateTime.Now;
                _repo.Add(posted);

                return posted;
            });

            Delete("Id={id}", args =>
            {
                _repo.Remove(args.id);
                return $"{args.id} Removed";
            });
        }

    }
```

If you read our [other post](/post/2016-10-25-nancyfx-webapi-dapper/) this probably looks very familiar--it is the module that allows `GET` and `POST` requests to accomplish different things.  For further clarification, feel free to [check out the Github repo here](https://github.com/nandotech/VSNancyDemo).  So what if we wanted to run a `Before` hook that intercepted the request under certain circumstances?

_DispoModule.cs_

```csharp
 public class DispoModule : NancyModule
    {
        public DispoModule(IDispoRepository _repo)
            : base("/dispo")
        {
            Before += (ctx) => {
                return "Intercepted";
            };

            //Remainder of code stays same
        }
    }
```

We navigate to `/dispo/` and exactly as expected, the browser simply returns the `string` "Intercepted", indicating that our `Before` hook did hijack the request. So, in other words, using `Before` executes that specified action before executing the requested Route, right?

This is not _entirely_ true.  Because of the manner which Nancy operates, when you first hit a module (ANY Module) with a request, Nancy steps through the `Module` to ensure the current route is valid/matches. I imagine this has in large part to do with the fact the routes are dynamically constructed and also for Nancy to determine the Route with the highest **weight** to receive the request.

Once Nancy finds a matching route, then it jumps over to the `Before` hook, if there is one available. See the execution of our `DispoModule.cs` currently, first breaking at the matching Route ("/") and upon pressing `F5` it jumps right into the before hook.

![Nancy 2.0 Sync Before Gif](http://i.imgur.com/wdc06Yu.gif)

There is one minor difference in syntax between `async` and non-`async` Modules when implementing `Before/After`.  As you just saw, in a synchronous controller, your `Before` & `After` lifecycle hooks should look like these:

_Synchronous_

```csharp
Before += (ctx) => 
            {
                return "Interception";
            };

After += (ctx) => 
            {
                ctx.Resposne = "Post Interception";
            };
```

Now having added this to our DispoModule.cs, let's see how Nancy treats this.  Not surprisingly, very similar to what we just saw a minute ago.  _Nancy_ steps through every route (including `Before` and `After`) to figure out which is the correctly matching route (or pattern). Once the framework decides the request is properly formed, it enters the `Before` method and sets our `Response`, however our request is not done yet.

From here, (since we performed a `GET` request on `/dispo/`), we enter `Get("/", args)` from the _DispoModule.cs_. Now here's the interesting part, or at the very least something to be conscious of: inside of our `GET` we do `return _repo.GetAll();` which is supposed to return a JSON object of all the `Dispo`'s currently saved in the database. Our lovely `NancyFX` framework then moves on to the `After` action (`ctx.Response = "Post-Interception"`) which re-writes the current `Response` and leaves us with the new simple string.

![NancyFX After Hook](http://i.imgur.com/BEtuXdO.png)

Like I mentioned, using `Before`/`After` in `async` modules has a slightly different syntax. In these, not only are you forced to declare them as explicity `async`, but you also have the option of using a "Cancellation Token" (`ct`) in order to cause and/or track failures.

_Asynchronous_

```csharp
Before += async (ctx, ct) =>
            {
                this.AddToLog("Before Hook Delay\n");
                await Task.Delay(5000);

                return null;
            };

After += async (ctx, ct) =>
            {
                this.AddToLog("After Hook Delay\n");
                await Task.Delay(5000);
                this.AddToLog("After Hook Complete\n");

                ctx.Response = this.GetLog();
            };
```

In both the synchronous and asynchronous examples, if your eye is better than mine you also noticed one other glaring difference.  Personally, I had to figure it out the hard way when error after error kept popping up.

In your `Before` pipeline you are capable of `return`ing anything and short-circuiting a request in that way, so long as you do not also have an `After` method (or so long as your `After` pipeline does not edit the `ctx.Response` as your response would then be overwritten). 

### Please note: **You _may not_ `return` values from the `After` pipeline.  In order to achieve this you may use `ctx.Response =`  and set it as you please.**

Nonetheless, these are fantastic abilities offered up for free by the framework and are extremely easy to take advantage of. Moreover, even though I didn't really go over it, these code examples all also show the `ctx` or `NancyContext` that comes baked into the framework, accessible from nearly any request.

So with that said, here is our `AsyncModule` that just shows off some of the features available to us as well as a decent example of using built-in `NancyContext` to make a simple logger.

_AsyncModule.cs_

```csharp
    public class AsyncModule : NancyModule
    {
        public AsyncModule()
        {
            Before += async (ctx, ct) =>
            {
                this.AddToLog("Before Hook Delay\n");
                await Task.Delay(5000);

                return null;
            };

            After += async (ctx, ct) =>
            {
                this.AddToLog("After Hook Delay\n");
                await Task.Delay(5000);
                this.AddToLog("After Hook Complete\n");

                ctx.Response = this.GetLog();
            };

            Get("/async", async (args, ct) =>
            {

                this.AddToLog("Delay 1\n");
                await Task.Delay(1000);

                this.AddToLog("Delay 2\n");
                await Task.Delay(1000);

                this.AddToLog("Executing async http client\n");
                var client = new HttpClient();
                var res = await client.GetAsync("http://nancyfx.org");
                var content = await res.Content.ReadAsStringAsync();

                this.AddToLog("Response: " + content.Split('\n')[0] + "\n");

                return (Response)this.GetLog();
            });
            
        }

        private void AddToLog(string logLine)
        {
            if (!this.Context.Items.ContainsKey("Log"))
            {
                this.Context.Items["Log"] = string.Empty;
            }

            this.Context.Items["Log"] = (string)this.Context.Items["Log"] + DateTime.Now + " : " + logLine;
        }

        private string GetLog()
        {
            if (!this.Context.Items.ContainsKey("Log"))
            {
                this.Context.Items["Log"] = string.Empty;
            }

            return (string)this.Context.Items["Log"];
        }
    }
```

As you can see, this is designed to really show off some of the `Async` features as well as using `Before` and `After` hooks in tandem with an `Async` Module.

Since our `GET` request is marked `async`, so are our `Before` & `After` pipelines. By that same token, they both now operate just like any other asynchronous method.  We can `await` any long-running action which we simulate here using `Task.Delay()`.

If you run the above code, here is what you will see in your browser:

```
11/10/2016 6:47:15 PM : Before Hook Delay
11/10/2016 6:47:20 PM : Delay 1
11/10/2016 6:47:21 PM : Delay 2
11/10/2016 6:47:22 PM : Executing async http client
11/10/2016 6:47:23 PM : Response: <!DOCTYPE html>
11/10/2016 6:47:23 PM : After Hook Delay
11/10/2016 6:47:28 PM : After Hook Complete
```

We see "Before Hook Delay" is triggered in the `Before` method exactly 5 seconds prior to the next statement.  Then there are "Delay 1" and "Delay 2", followed by "Executing async http client" where we are simply using `HttpClient` to run a `GET` request to [http://nancyfx.org](http://nancyfx.org).  This is also output to the "Log" (using `AddToLog`) and we see on Line 5, `11/10/2016 6:47:23 PM : Response: <!DOCTYPE html>`.  The final 2 messages that display are both clearly in the `After` hook of the pipeline.

In any case, I hope this helps someone and further helps to clarify any questions you may have regarding the framework. Some up-to-date documentation may be hard to find, but **please** do not allow that to discourage you from trying Nancy out and finding out if it's for you.

If you like what you see and would like to either build applications using Nancy or contribute to the project ([Nancy is open source](https://github.com/NancyFx/Nancy)), please do not hesitate.

There is an awesome community of [`NancyFX` developer's Slack](http://nancyfx.slack.com) that you may join to interact with and ask questions, 100% free to join.  The actual Nancy developers also frequent the chat very regularly. 

Thank you for reading and please do come visit us @ [http://nancyfx.slack.com](http://nancyfx.slack.com) and let us know what you think!  Questions? Concerns? Need help implementing a feature?  Come on and stop by.

