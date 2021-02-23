---
title: "Using MediatR Request Handlers in ASP.NET Core to Decouple Code"
date: 2021-02-15 17:28:00 +00:00
author: "Steven McLintock"
layout: post
image: /assets/img/2021/02/mediatr_gradient_128x128.png
icon: mediatr.png
tags: csharp mediatr
---

MediatR, by it's definition, is a **simple, unambitious mediator implementation 
in .NET**. It was released in [2014](https://www.nuget.org/packages/MediatR/0.1.0) 
by [Jimmy Bogard](https://github.com/jbogard) and is a useful package that can be 
used to implement the popular mediator pattern in .NET projects. It's available on 
[NuGet](https://www.nuget.org/packages/MediatR/) and is also open-source 
on [GitHub](https://github.com/jbogard/MediatR).

This article isn't meant to be an in-depth explanation of the mediator pattern, 
for that I'd recommend the series of articles over at 
[.NET Core Tutorials](https://dotnetcoretutorials.com/) beginning with 
[The Mediator Pattern In .NET Core – Part 1 – What’s A Mediator?](https://dotnetcoretutorials.com/2019/04/30/the-mediator-pattern-in-net-core-part-1-whats-a-mediator/). Instead, I thought 
I'd demonstrate the advantages of using the MediatR package within a web application 
built using ASP.NET Core and C#, using request handlers to decouple code.

## ASP.NET Core Web Application Without MediatR

In this article I'll be using a demo ASP.NET Core web application 
*(available on [GitHub](https://github.com/kiltandcode/mediatr-demo))* that 
**was originally built without using MediatR**. This is to demonstrate 
the benefits of adding the mediator pattern into an existing project and how 
implementing MediatR request handlers can help decouple the code.

The demo ASP.NET Core web application contains the following projects:

* **MediatRApi** *(the API used for accepting GET, POST and PUT requests)*
* **MediatRApi.Repositories** *(the repositories used for interacting with the database)*
* **MediatRApi.Services** *(the services used throughout the application, such as the validation service)*

{%
    include image.html
    year='2021'
    month='02'
    file='solution-explorer-without-mediatr.png'
    alt='ASP.NET Core web application without MediatR'
    caption='ASP.NET Core web application without MediatR'
%}

In the controller that *doesn't* use MediatR, you'll notice **the constructor is being injected 
with 4 dependencies**; 1 validation service as well as 3 repositories. However, if you 
notice in each of the action methods, only one repository is being used by each of them. **Without 
using MediatR, this constructor is already a code smell:**

#### WithoutMediatRController.cs

```csharp
[ApiController]
[Route("[controller]")]
public class WithoutMediatRController : ControllerBase
{
    private readonly IValidationService _validationService;
    private readonly IArtistRepository _artistRepository;
    private readonly IAlbumRepository _albumRepository;
    private readonly ISongRepository _songRepository;

    public WithoutMediatRController(
        IValidationService validationService,
        IArtistRepository artistRepository,
        IAlbumRepository albumRepository,
        ISongRepository songRepository)
    {
        _validationService = validationService;
        _artistRepository = artistRepository;
        _albumRepository = albumRepository;
        _songRepository = songRepository;
    }

    [HttpGet("get-artist/{artistId:guid}")]
    public async Task<Artist> GetArtist(Guid artistId)
    {
        _validationService.Validate<Guid>(artistId);

        return await _artistRepository.GetArtist(artistId);
    }

    [HttpPost("create-album")]
    public async Task<Guid> CreateAlbum(Album album)
    {
        _validationService.Validate<Album>(album);

        return await _albumRepository.CreateAlbum(album);
    }

    [HttpPut("set-rating")]
    public async Task SetRating(SetRating setRating)
    {
        _validationService.Validate<SetRating>(setRating);

        await _songRepository.SetRating(setRating);
    }
}
```

## ASP.NET Core Web Application With MediatR

Ideally, **we want one dependency in the constructor of the controller: MediatR**.

We'll use the mediator pattern to decouple the code, creating separate 
**"requests"** that will store instructions for executing code in 
associated **"request handlers"**, with each request handler having it's 
own set of dependencies.

I've added the new project **MediatRApi.Handlers** into the solution to store all our 
MediatR requests and request handlers that will be used to implement the mediator pattern 
in our ASP.NET Core web application:

{%
    include image.html
    year='2021'
    month='02'
    file='solution-explorer-with-mediatr.png'
    alt='ASP.NET Core Project using MediatR'
    caption='ASP.NET Core Project using MediatR'
%}

You'll need to add the MediatR [NuGet](https://www.nuget.org/packages/MediatR/) 
package into your project to use the MediatR namespace:

{%
    include image.html
    year='2021'
    month='02'
    file='mediatr-nuget.png'
    alt='MediatR package on Nuget'
    caption='MediatR package on Nuget'
%}

### Requests and Request Handlers

Let's explore each request and it's associated request handler in this new project.

#### GetArtistRequest and GetArtistHandler

GetArtistRequest inherits from **MediatR.IRequest** and it's response will be an Artist. The 
GetArtistHandler class inherits from **MediatR.IRequestHandler** and the dependencies being injected 
into it's constructor are *only* used for retrieving an artist. The Handle method is used to 
execute the code and return the artist:

```csharp
public class GetArtistRequest : IRequest<Artist>
{
    public Guid ArtistId { get; set; }
}

public class GetArtistHandler : IRequestHandler<GetArtistRequest, Artist>
{
    private readonly IValidationService _validationService;
    private readonly IArtistRepository _artistRepository;

    public GetArtistHandler(
        IValidationService validationService,
        IArtistRepository artistRepository)
    {
        _validationService = validationService;
        _artistRepository = artistRepository;
    }

    public async Task<Artist> Handle(
        GetArtistRequest request,
        CancellationToken cancellationToken = default)
    {
        _validationService.Validate<Guid>(request.ArtistId);

        return await _artistRepository.GetArtist(request.ArtistId);
    }
}
```

#### CreateAlbumRequest and CreateAlbumHandler

Similar to GetArtistRequest, the CreateAlbumRequest and CreateAlbumHandler inherit from 
MediatR.IRequest and MediatR.IRequestHandler and are responsible for 
creating an album and returning a guid:

```csharp
public class CreateAlbumRequest : IRequest<Guid>
{
    public Album Album { get; set; }
}

public class CreateAlbumHandler : IRequestHandler<CreateAlbumRequest, Guid>
{
    private readonly IValidationService _validationService;
    private readonly IAlbumRepository _albumRepository;

    public CreateAlbumHandler(
        IValidationService validationService,
        IAlbumRepository albumRepository)
    {
        _validationService = validationService;
        _albumRepository = albumRepository;
    }

    public async Task<Guid> Handle(CreateAlbumRequest request,
        CancellationToken cancellationToken = default)
    {
        _validationService.Validate<Album>(request.Album);

        return await _albumRepository.CreateAlbum(request.Album);
    }
}
```

#### SetRatingRequest and SetRatingHandler

Last but not least, SetRatingRequest and SetRatingHandler will set a rating for a 
song and return **MediatR.Unit**. MediatR.Unit represents a void type, 
since void isn't a valid return type in C#:

```csharp
public class SetRatingRequest : IRequest<Unit>
{
    public Guid SongId { get; set; }

    public int Rating { get; set; }
}

public class SetRatingHandler : IRequestHandler<SetRatingRequest, Unit>
{
    private readonly IValidationService _validationService;
    private readonly ISongRepository _songRepository;

    public SetRatingHandler(
        IValidationService validationService,
        ISongRepository songRepository)
    {
        _validationService = validationService;
        _songRepository = songRepository;
    }

    public async Task<Unit> Handle(
        SetRatingRequest request,
        CancellationToken cancellationToken = default)
    {
        SetRating setRating = new SetRating
        {
            SongId = request.SongId,
            Rating = request.Rating
        };

        _validationService.Validate<SetRating>(setRating);

        await _songRepository.SetRating(setRating);

        return Unit.Value;
    }
}
```

### Add MediatR to Startup.cs

To add MediatR as a dependency throughout our web application *(i.e in the 
constructor of the controller)*, we'll need to register MediatR in the 
**'ConfigureServices'** method within **Startup.cs**. To accomplish this we first need to 
add the [Nuget](https://www.nuget.org/packages/MediatR.Extensions.Microsoft.DependencyInjection/) 
package **MediatR.Extensions.Microsoft.DependencyInjection** into the project:

{%
    include image.html
    year='2021'
    month='02'
    file='mediatr-nuget-dependency-injection.png'
    alt='MediatR.Extensions.Microsoft.DependencyInjection package on Nuget'
    caption='MediatR.Extensions.Microsoft.DependencyInjection package on Nuget'
%}

I've created a **'Dependencies'** class with the one method **'RegisterRequestHandlers'** 
that will be called from the 'ConfigureServices' method within the Startup.cs 
file of the web application:

#### Dependencies.cs

```csharp
public static class Dependencies
{
    public static IServiceCollection RegisterRequestHandlers(
        this IServiceCollection services)
    {
        return services
            .AddMediatR(typeof(Dependencies).Assembly);
    }
}
```

Within the **'ConfigureServices'** method of **Startup.cs**, **'RegisterRequestHandlers'** is 
being called to inject MediatR into the web application and register the request handlers:

#### ConfigureServices (in Startup.cs)

```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    // Register the services and repositories
    services.AddSingleton<IValidationService, ValidationService>();
    services.AddSingleton<IArtistRepository, ArtistRepository>();
    services.AddSingleton<IAlbumRepository, AlbumRepository>();
    services.AddSingleton<ISongRepository, SongRepository>();
    
    // Register the MediatR request handlers
    services.RegisterRequestHandlers();

    services.AddControllers();
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo { Title = "MediatRApi", Version = "v1" });
    });
}
```

Let's now see how this has helped to decouple our code in the web application!

**In the new controller, there is now only one dependency: MediatR.** Each of the action methods 
will initialize a new request and send it off to MediatR to be handled by it's associated 
request handler:

#### WithMediatRController.cs

```csharp
[ApiController]
[Route("[controller]")]
public class WithMediatRController : Controller
{
    private readonly IMediator _mediator;

    public WithMediatRController(
        IMediator mediator) =>
        _mediator = mediator;

    [HttpGet("get-artist/{artistId:guid}")]
    public Task<Artist> GetArtist(Guid artistId) =>
        _mediator.Send(new GetArtistRequest { ArtistId = artistId });

    [HttpPost("create-album")]
    public Task<Guid> CreateAlbum(Album album) =>
        _mediator.Send(new CreateAlbumRequest { Album = album });

    [HttpPut("set-rating")]
    public void SetRating(SetRating setRating) =>
        _mediator.Send(new SetRatingRequest
        {
            SongId = setRating.SongId, Rating = setRating.Rating
        });
}
```

## GitHub Repository

Thank you for reading this article! The full solution used in this guide is available at 
[github.com/kiltandcode/mediatr-demo](https://github.com/kiltandcode/mediatr-demo)