---
title: Visual Studio 2017 Final Released
subtitle: With lots of super cool awesome features
date: 2017-03-09
description: "Visual Studio 2017 Final Released with an Impressive Feature Set"
tags: ["visual studio",".net core"]
categories:
    - ".NET Core"
    - "visual studio"
---

# [Microsoft](http://www.microsoft.com) [has finally dropped Visual Studio 2017](https://www.visualstudio.com/vs/whatsnew/) with a bevy of new features and also relocated all our [.NET Core](https://dot.net) docs over to [https://docs.microsoft.com/](https://docs.microsoft.com/).

I, for one, had downloaded Visual Studio 2017 RC quite a while back and due to some horrible judgement and lack of forethought I actually had been using the IDE almost full-time for my .NET projects.  I basically used it for everything I could and if I couldn't make the transition, I simply used [Visual Studio Code](https://code.visualstudio.com).  Which, by the way, if you aren't using it, you probably really should be.  VS Code has become my absolute go-to for pretty much everything that I don't use Visual Studio for.

If you haven't already, you can find link & instructions to [download VS 2017 Final](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-mvc-app/start-mvc) and get started on your first app.

So we have Visual Studio 2017 Final now and I honestly must say that I love the IDE thus far.  Some features are kinda small and stupid (but surprisingly useful and freaking awesome) like the new way the IDE can show you _"show structure guide lines"_.  Some are much more practical and extremely useful, like "click of a button" [Docker](https://docker.com) support making it feel very much like a first class citizen, including the auto generation of [docker-compose](https://docs.docker.com/compose/) files.

![Visual Studio 2017 Docker File Structure](http://i.imgur.com/axsA6Xl.png)

As you can see, if when creating your [ASP.NET Core Project](http://docs.microsfot.com) you click the "Enable Docker  Support" box, you get all these files right out of the box, which is pretty freaking awesome.  Shame on us lazy developers for not remembering the syntax, but I for the life of me could not ever recall the _Dockerfile_ layout and rules.  Just didn't stick well in my brain.

Check out the samples of the files provided below.  Once again, this is a completely fresh project with nothing but a Home, About & Contact pages with the standard bootstrap/etc. boilerplate.  One thing I do find interesting: debugging locally this site seems to be running even faster than I feel like my kestrel web servers typically would.

_Dockerfile_

```dockerfile
FROM microsoft/aspnetcore:1.1
ARG source
WORKDIR /app
EXPOSE 80
COPY ${source:-obj/Docker/publish} .
ENTRYPOINT ["dotnet", "VsFinalDemo.dll"]

```

_docker-compose.yml_

```yml
version: '2'

services:
  vsfinaldemo:
    image: vsfinaldemo
    build:
      context: ./VsFinalDemo
      dockerfile: Dockerfile

```
_docker-compose.ci.build.yml_

```yml
version: '2'

services:
  ci-build:
    image: microsoft/aspnetcore-build:1.0-1.1
    volumes:
      - .:/src
    working_dir: /src
    command: /bin/bash -c "dotnet restore ./VsFinalDemo.sln && dotnet publish ./VsFinalDemo.sln -c Release -o ./obj/Docker/publish"

```

Now that Microsoft has this in final release, I may have to start using some `.csproj` and running some `ASP.NET Core` built with Visual Studio 2017.

On another note, I'm also beginning a rather large undertaking of building a microservices architecture.  This is our wrinkle, because I am extremely inclined towards and interested in using [NancyFX](https://nancyfx.org) for all the points of our microservice in general.  That said, I'm not sure `.csproj` and anything that comes along with it will apply once I really get to cranking out code.

Nonetheless, I may totally still use VS2017 to help cheat creating `Dockerfiles` and `docker-compose` files, that's for sure! :)