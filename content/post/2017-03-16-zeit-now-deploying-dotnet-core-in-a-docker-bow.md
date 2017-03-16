---
title: ZEIT Now Deploying ASP.NET (and .NET) Core Apps in seconds with your Dockerfile
subtitle: Add the Dockerfile, watch magic happen.        
date: 2017-03-16
description: "Using ZEIT cloud service & now deployment tool to take your .NET Core web app from staging to production.  Dockerizing your apps made simple!"
tags: ["DevOps","cloud",".net core","docker","zeit","now"]
categories:
    - ".NET Core"
    - "Docker"
    - "Deployment"
    - "Automation"
    - "DevOps"
---

# If you're anything like me, you constantly are finding more and more tasks that require your attention.

To make matters worse, the industry definition of [_"DevOps"_](https://en.wikipedia.org/wiki/DevOps) is still evolving itself, quickly becoming ubiquitous with "a litle bit of everything" in the process. 

Like a breath of fresh air, [ZEIT](https://zeit.co) is a cloud service that allows near-mindless deployment of `javascript` & `node.js` apps.  Much thanks probably also belongs to [Scott Hanselman](https://twitter.com/shanselman) and his ever-tinkering soul who published ["ZEIT now deployments of open source ASP.NET Core web apps with Docker"](https://www.hanselman.com/blog/ZEITNowDeploymentsOfOpenSourceASPNETCoreWebAppsWithDocker.aspx) yesterday and I was immediately intrigued.  Since Zeit Now supports [Docker](https://docker.com), then why not [.NET Core](https://dot.net)?

In the blog post, Scott (with help from friend [Glenn Condron](https://twitter.com/condrong?lang=en)) walks you through from creating a fresh ASP.NET Core Web App using the `dotnet cli`.  Next, it is almost embarassingly simple to see him 

* run a `dotnet restore`
* create a valid `Dockerfile`
* `dotnet publish`
* ensure your project outputs are in the right directory
* type `now` command down in your publish folder

And that's it.  You now have a live URL on your clipboard--this is the DNS assigned to the [`Docker`](https://docker.com) container that is running in the cloud for you. As I poked around the [Zeit Now](https://zeit.co/now) app & documentation I also found some really cool features like single click "file sharing" in the form of deploying a container with your file.

The number of technically savvy people available in my vicinity has been reduced, yet in the mid-to-long term here we do expect to have a significant software [microservices architecture](http://amzn.to/2mydV0m) deployment. That said, it is in my best interest to begin finding and familiarizing myself with the _absolute simplest_ tools and procedures in order to put them into place and avoid some of the DevOps minefield that a team can find itself on when it is unfamiliar with the options available.

[Zeit Now](https://zeit.co/now) seems like a fantastic service to fit many of our needs. My test was then to see if this was as simple & easy for me as it seemed to be for Mr. Hanselman.  One main difference between my project and Scott's here as well: I am not using the barebones `ASP.NET MVC` template.  Instead, I took a functioning [NancyFX](https://www.nancyfx.org) example API/website, still using `project.json` structure instead of `MSBuild` as well, generated the `Dockerfile` and after adding a few items under our `include` in `publishOptions`...

I wanted to know if this was going to work with my existing [NancyFX](https://nancyfx.org) services and/or if there were any extra gotcha's or things to know. As a jumpstart, the easiest thing to do was simply clone my [`VSNancyDemo` repository](https://github.com/nandotech/VSNancyDemo/) from [GitHub](https://github.com).

The [repository is public](https://github.com/nandotech/VSNancyDemo/) so feel free to clone the project as well if you'd like to follow along or try out your ZEIT Now with something a little different or pull in any project of your own!

Lines added to _project.json_ (to ensure required files copy to `publish` dir)

```json
  "publishOptions": {
    "include": [
      "wwwroot",
      "web.config",
      "appsettings.json",
      "Content",
      "Dockerfile",
      "views"
    ]
  }
```

I noticed if some of the above were missing, there would be some issues. For instance, before I added the `"views"` folder, I was getting a `500 Error` from `Nancy` as if the `index.html` did not exist.  Rightfully so, as in the container environment that ZEIT deployed the folder with views was not copied.

_Dockerfile_

```dockerfile
FROM microsoft/aspnetcore
LABEL name="DispoStatsService"
ENTRYPOINT ["dotnet", "DispoStatsService.dll"]
ARG source=.
WORKDIR /app
EXPOSE 80
COPY $source .

```

Easy as pie.

So as to not be repetetive, I'll pick up here _after_ the `dotnet publish`

![Publish Output Directory](http://i.imgur.com/C6tiFbc.png)

You can see at the very end here I ran the `now` command inside the folder `dotnet cli` published to.  When it is done there, your app is deployed and running in a container.  The URL (since it's a little long and auto-generated) gets automatically copied to your clipboard.

I wanted to go ahead and also get custom domain names working as [Zeit.co](https://zeit.co) seemed to make doing so extremely easy. See the `cli` output, deployment and alias assignment below:

![ZEIT Deployment and Alias](http://i.imgur.com/zAV9pgo.png)

Lo' and behold, I have my `NancyFX 2.0-clinteastwood` sample app running on `Docker` in the cloud using [https://zeit.co](https://zeit.co).  The container may be reached using either the `alias` I assigned, [https://umgdss.nt-x.com](https://umgdss.nt-x.com), or the original address, [https://dispostatsservice-wjpwcfkfcq.now.sh/](https://dispostatsservice-wjpwcfkfcq.now.sh/).  While we are admiring the shiny tool: see how it also auto-issued SSL certificate for the `alias` domain.

Another cool thing about this sample application is some of the very basic routes and functions incorporate some framework bits, like the [dependency injection](https://en.wikipedia.org/wiki/Dependency_injection) container, so we can be confident parts of the app were tested/working early on.

For example, as shown below, my _Home_ `/` Route returns an `index.html` with whatever `Greeting` is in `appsettings.json` and the `/test/` route right next to it does the same, but returns plain text with no web page. This is helpful, for instance, in how it helped me realize the `/views/` folder was not  in the publish path.  The final 2 browsers simply show a sample `AsyncModule` with some of the possibilities using the built-in request pipelines and last but not least: the `/os/` route!

Don't let the code for this overwhelm you, ok?

```csharp
   Get("/os", x =>
    {
       return System.Runtime.InteropServices.RuntimeInformation.OSDescription;
    });

```

![Route Screenshot](http://i.imgur.com/fXmVmvo.png)

I find this one particularly cool because it works no matter where or how you are hosting your [NancyFX](https://nancyfx.org) app, the route will return what operating system the host is running. Here on this cloud provider the app is aware of itself being hosted on `Linux 4.7.0-coreos` which sounds more like a kernel version (container) to me.

Here is the exact same app again, but debugging locally (ignoring our `Dockerfile` etc).

![/os/ route debugging](http://i.imgur.com/kkkZejX.png)

### Now go on, hack and be Merry.

I hope some part of this was helpful, I personally want to thank [Scott Hanselman](https://twitter.com/shanselman) and [Glenn Condron](https://twitter.com/condrong) for their tinkering and getting this to work! This has not only helped me overall understand the concept and inner workings with regard to [Docker](https://docker.com), but it also exposed me to an awesome service & host in [ZEIT](https://zeit.co) that I immediately fell in love with!

Now that I've completed that diversion--back to writing the **core functionality** for this API & app I'm supposed to be working on.