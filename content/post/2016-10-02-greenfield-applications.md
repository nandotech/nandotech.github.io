---
title: Greenfield Applications (Part 1, Project Armadillo)
subtitle: Architecture Decisions and where we're headed
date: 2016-10-02
---

#### There's something whimsical about Greenfield applications.

The feeling of complete freedom over architecture decisions. The drawn out, sometimes heated debates concerning the most minute of details. Javascript frameworks.  [React](reactjs.org), no--let's use [Angular](http://angular.io)!  Better yet, let's use Anguar 2 because that's got to be better than 1, right?

All kidding aside, the feeling of freedom of laying out and choosing technologies and how they will be distributed, hosted & supported is pretty awesome.

To be _perfectly honest_, we've probably put off this exact project for many months.  Between semi-pending projects hanging over our heads and some existential crises, we spent some time not making much progress.  This is something we've known our clients could use for a long time, but we didn't pull the trigger.

Being .NET developers, we have been closely following the .NET core and it's developments.  We have also very much enjoyed and embraced [TypeScript](http://typescriptlang.org) and dabbled in [NativeScript](http://nativescript.org), which further interested us in Angular 2.

It feels like the perfect storm.  Now that .NET Core was released as 1.0 and then even patched to 1.0.1, I was ready. To my utter joy and surprise, the Angular team then announces Angular 2 Final.

### All that said, this post was about Greenfield apps, right?

So without further ado, here is our tentative model & deployment plan for what we've codenamed **Project Armadillo _1.0 alpha_**

#### Development/Server Technologies:
- [SQL Server 2016](https://www.microsoft.com/en-us/cloud-platform/sql-server)
  * Love the new [Json data type](https://blogs.msdn.microsoft.com/jocapc/2015/05/16/json-support-in-sql-server-2016/) which we will leverage heavily
- [ASP.NET Core 1.0](http://dot.net) Web API
  * Although [MVC](https://docs.asp.net/en/latest/mvc/controllers/index.html) made both MVC & WebAPI controllers derive from the same class, ASP.NET is planned to be used explicitly as a back-end web service
- [Entity Framework Core 1.0](https://docs.efproject.net/en/latest/intro.html)
    * for majority of data access/CRUD)
- [Dapper.NET](https://github.com/StackExchange/dapper-dot-net)
    * for SQL-intensive returns & complex queries
    * I imagine there may be several composing and/or decomposing JSON data
- [JSON.NET](http://www.newtonsoft.com/json)
    * If necessary for further serialization capabilities
- [Redis](http://redis.io)
    * For any caching mechanisms & needs (haven't planed this out)
    * [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) to manage connections

#### Deployment Decisions

- MOSTLY TBD...
- The only thing we are certain of is we will be running kestrel, but it will be either behind IIS or NGINX, something we have not decided on.
- There is the tertiary option of running any/all of these things on Docker now that they are fully supported (other than SQL Server, which we have servers to run on)
- We have plenty of in-house infrastructure on which the bulk of this software will run.
    * We may utilize [Azure](http://www.azure.com) for some fail-safe or high-availability functionality

#### Front End Framework/Design Decisions

- Still finalizing on this front and we will see a part 2 of this blog post.




