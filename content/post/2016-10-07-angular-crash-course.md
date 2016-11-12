---
title: Angular 2 Crash Course
subtitle: A collection of free & paid Angular 2 resources
date: 2016-10-07
tags: ["angular2","learning","resources","javascript"]
categories:
    - "Angular 2"
    - "JS Frameworks"
---

_PSA: Skip to the **TL;DR** for list of resources_

## So, due to forces of nature beyond our control (specifically Hurricane Matthew),
the [Angular 2 Workshop](http://ftlauderdale.ng-learn.com) we were registered for this Thursday & Friday, October 6th & 7th with both [John Papa](https://johnpapa.net/) and [Dan Wahlin](https://www.codewithdan.com) was cancelled. Thankfully for us South Floridians, [Hurricane Matthew](http://www.nytimes.com/2016/10/07/us/hurricane-matthew.html?_r=0) narrowly avoided a direct hit with us southernmost residents and the tri-county area of Dade, Broward & Palm Beach was mostly unaffected.

#### That said, we were incredibly lucky, and many residents in the central and northern regions of our state are not nearly as lucky.  Areas near and around Daytona Beach, FL I believe suffered the brunt of it and our best wishes are with the most affected communities.

That's not even saying anything in regards to the rest of the Carribean like Cuba, Haiti & Jamaica who suffered through the grand [majority of Matthew's wrath](https://weather.com/news/news/hurricane-matthew-haiti-latest-news) and will be in recovery mode for quite a while.  I wish I knew a good charity, because this is where I would link and recommend some.  If someone cares to enlighten me, I will update this post.

## **Workshop Or Not:**

Our _biggest issue_ and concern with missing out on this conference is really that we were 100% **planning on using [Angular 2](https://angular.io) as the front end framework** for our _Project Armadillo_.  I also even have a couple of small WebAPI apps fleshed out with no front end in _anticipation_, awaiting our 2nd such workshop with John & Dan (we saw them during Fall 2015 [DevIntersection](http://www.DevIntersection.com)) before getting started and really digging into the framework.

The aforementioned cancellation due to _Hurricane Matthew_ has now put us at quite a crossroads with a bit of a decision to make.  To a certain degree, absolutely nothing has changed and we could still use Angular 2. An alternative (which appeals less to me, but more to my coding partner) is to simply use the [ASP.NET Core](https://dot.net) Razor View Engine and avoid messing with more new frameworks, technologies as well as moving parts.  

We were extremely excited (especially with Angular 2 Final's recent release) to get Dan & John's crash course, and more importantly, have access to these two of the community's foremost experts on our entire technology stack. This made us confident that even with my partner's minimal Angular exposure we would be able to quickly get it together.

Now this all isn't going to happen, and it does no one any good to cry over spilled milk...So the crash course is up to us. What follows is my **"Getting Started w/ Angular 2 Crash Course"**, or my compilation of resources, walkthroughs, blog posts & some paid content like [Pluralsight Videos](https://www.pluralsight.com), [Code School](http://www.codeschool.com) and anything else I could find to help get started with Angular 2's Final release.

# **TL;DR**

## __**Getting Started w/ Angular 2 Crash Course**__
### **Ultimate compilation**

**_The Basics/Getting Started_**

* As web developers, we will assume some familiarity with JavaScript
* Angular 2 has gone the way of many frameworks and chosen to leverage [TypeScript](http://typescriptlang.org), encouraging you to use it to build your ng2 apps.
* First order of business is familiarizing yourself with TypeScript, which I will not detail too much here, but the main documentation @ [http://www.typescriptlang.org/docs/tutorial.html](http://www.typescriptlang.org/docs/tutorial.html) is very good and thorough
* Get your feet wet and use the [TypeScript Playground](http://www.typescriptlang.org/play/index.html) to see how some TypeScript will transpile down into ES5 JavaScript.
* If you know some TypeScript and are familiar with web constructs, start below

**_Introducing Angular 2_**

* Again here we'll start at the [Official Angular 2 Site](https://angular.io/docs/ts/latest/quickstart.html#!#prereq)
* The link above is to the Angular 2 quickstart
    - This guide takes you through Creating your project, your first component, bootstrapping your application, installing all your dependencies and getting up and running
* This is an awesome start, but for a deeper understanding of the concepts and the framework as a whole, check these out:

_Pluralsight_

- [Angular 2: Getting Started](https://app.pluralsight.com/library/courses/angular-2-getting-started/table-of-contents) by [Deborah Kurata](https://www.twitter.com/DeborahKurata)
- [Angular 2 First Look](https://app.pluralsight.com/library/courses/angular-2-first-look/table-of-contents) by [John Papa](https://twitter.com/JohnPapa)
- [Play By Play: Angular2/RxJS/HTTP](https://app.pluralsight.com/library/courses/angular-2-first-look/table-of-contents) by John Papa & [Dan Wahlin](https://twitter.com/DanWahlin)
    
_Code School_

- [Accelerating Through Angular 2](https://www.codeschool.com/courses/accelerating-through-angular-2) by [Gregg Pollack](https://www.twitter.com/greggpollack)

_Egghead.io_

- [Angular 2](https://egghead.io/technologies/angular2)

_Blogs/Resources_

* [Why TypeScript?](https://vsavkin.com/writing-angular-2-in-typescript-1fa77c78d8e8#.szbkd4gw4) by [Victor Savkin](https://twitter.com/victorsavkin)
* [Core Concepts of Angular 2](https://vsavkin.com/the-core-concepts-of-angular-2-c3d6cbe04d04#.mcjjar9sc) by Victor Savkin
* [Angular 2 Final Resource List](http://blog.thoughtram.io/angular/2016/09/15/angular-2-final-is-out.html) by [Pascal Precht](https://twitter.com/PascalPrecht)
* [Angular 2 Education Curated List](https://github.com/timjacobi/angular2-education) by [Tim Jacobi](https://github.com/timjacobi/)
* [Angular 2 Style Guide](https://angular.io/docs/ts/latest/guide/style-guide.html) by John Papa
* [Angular 2 Snippets by John Papa](https://marketplace.visualstudio.com/items?itemName=johnpapa.Angular2) or [by Dan Wahlin](https://marketplace.visualstudio.com/items?itemName=danwahlin.angular2-snippets)
* Blogs with consistently awesome ng2 content:
     - [Victor Savkin's Blog](https://www.vsavkin.com)
     - [John Papa's Blog](https://johnpapa.net)
     - [Pascal Precht's Blog](http://blog.thoughtram.io/)
     - [Dan Wahlin's Blog](http://www.codewithdan.com)
     - [AngularJS Blog](https://angularjs.blogspot.com/)
       

**_Putting it all together_**

* [Tutorial: Angular 2 Docs Tour of Heroes](https://angular.io/docs/ts/latest/tutorial/)
    - Directly from the ng2 team, this tutorial moves pretty quick but covers all the basics you need to get up and running up through some Routing and calling HTTP services.
    - This does expect you have some prior knowledge of [TypeScript](https://typescriptlang.org) and at have gotten through the [Angular 2 Quick Start](https://angular.io/docs/ts/latest/quickstart.html) picking up where it left off

* Slides: [Angular 2 and ASP.NET Core](http://codewithdan.me/angular2-aspnet-core
)
   - Many thanks and praises to the awesome [Dan Wahlin](https://twitter.com/DanWahlin) for these Slides
   - Helps to understand some of the "guts" or innards of Angular 2 in a basic visual way as well as providing real-world example and use creating an app that calls a real web service 
   - Along with his quickstart projects, I found most useful his repo (fully updated to latest versions) of an [ASP.NET Core](https://dot.net) WebAPI and fully functional Angular 2 front end that includes examples of all types of requests
   - Full repo: [https://github.com/DanWahlin/CoreWebAPIAndAngular](https://github.com/DanWahlin/CoreWebAPIAndAngular) and just follow the instructions he's provided.
* [Angular2-Go](https://github.com/johnpapa/angular2-go), a super-simple barebones Angular2 App by John Papa featuring 1 module with 2 routes
* John Papa's [Tour of Heroes Repo](https://github.com/johnpapa/angular2-tour-of-heroes) which has his implementation of the Official Docs Tutorial
* For those migrating existing 1.x apps, [Joseph Eames](https://twitter.com/josepheames) Pluralsight course on [Migrating to Angular 2](https://app.pluralsight.com/courses/migrating-applications-angular-2) is probably the best resource I have come across.
    - Also, [Adventures in Angular Podcast](https://dev.to/adventuresinangular/112-aia-upgrading-from-angular-1x-to-angular-2)
* [Creating a CRUD App with Angular CLI](https://www.sitepoint.com/angular-2-tutorial/) by [Todd Motto](https://twitter.com/toddmotto) and [Jurgen Van der Moere](https://twitter.com/jvandemo)


# Phew, That was longer than expected.

Nonetheless, I feel like you can never have too many **good examples** or posts showing different styles of implementations of Angular 2. For our part, we will likely read Victor Savkin's listed post above and then cherry-pick some of the resources in the "resource list"-style blog posts to round out our knowledge.

From there, we move into the **"Putting it All Together"** section and go through some of the code samples and best practices. If you've taken the time to understand all the Angular 2 concepts thus far you will be well on your way. Part of _putting it all together_ also means starting to put together your own modules/components and just jumping in to get your feet wet.

I am a strong proponent of clean coding and best practices, but I have also found that the greatest value and the most deeply ingrained learning takes place when you are implementing your own solutions. 

Immediately after getting what I think I need from the **Putting it All Together** section--next step is we plan to use Angular 2 to build a simple front end dashboard for an [ASP.NET Core](https://dot.net) API project running with the [NancyFx](http://www.nancyfx.org) framework.  Using RxJS & Observables + Http in Angular 2 (as per some of the resources above) I'm hoping to provide a rich SPA showing some key performance indicators in real time, polling data from 2 disparate web-based systems.


