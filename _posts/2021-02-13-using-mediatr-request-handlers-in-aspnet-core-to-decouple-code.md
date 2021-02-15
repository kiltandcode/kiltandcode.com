---
title: "Using MediatR Request Handlers in ASP.NET Core to Decouple Code"
date: 2021-02-13 21:45:00 +00:00
author: "Steven McLintock"
layout: post
image: /assets/img/2021/02/mediatr_gradient_128x128.png
category: csharp
---

{%
    include image-lead.html
    year='2021'
    month='02'
    file='mediatr_gradient_128x128.png'
    alt='MediatR'
%}

MediatR, by it's definition, is a **simple, unambitious mediator implementation 
in .NET**. It was released in [2014](https://www.nuget.org/packages/MediatR/0.1.0) 
by [Jimmy Bogard](https://github.com/jbogard) and is a useful package that we can 
add to our .NET projects to implement the popular mediator pattern. It's available 
on [NuGet](https://www.nuget.org/packages/MediatR/) and is also open-source 
on [GitHub](https://github.com/jbogard/MediatR).

This article isn't meant to be an in-depth explanation of the Mediator pattern, 
for that I'd recommend the series of articles over at 
[.NET Core Tutorials](https://dotnetcoretutorials.com/) beginning with 
[The Mediator Pattern In .NET Core – Part 1 – What’s A Mediator?](https://dotnetcoretutorials.com/2019/04/30/the-mediator-pattern-in-net-core-part-1-whats-a-mediator/). Instead, I thought 
I'd demonstrate the advantages of using the MediatR package within an API built using 
ASP.NET Core and C#, more specifically using it's **request handlers to decouple your 
code** and create a cleaner codebase.

## ASP.NET Core Web Application Without MediatR

In this guide I'll be using a demo ASP.NET Core web application 
*(available on [GitHub](https://github.com/kiltandcode/mediatr-demo))* that 
**was originally built without using MediatR**. This is to demonstrate 
the benefits of implementing the mediator pattern and how introducing MediatR request 
handlers can help decouple our code.

The demo ASP.NET Core web application contains the following projects:

* **MediatRApi** *(the API used for accepting GET, POST and PUT requests)*
* **MediatRApi.Repositories** *(the repositories used for interacting with a database)*
* **MediatRApi.Services** *(the services used throughout the application, such as the validation service)*

{%
    include image.html
    year='2021'
    month='02'
    file='solution-explorer-without-mediatr.png'
    alt='ASP.NET Core web application without MediatR'
    caption='ASP.NET Core web application without MediatR'
%}

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

#### Startup.cs *(extract)*

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Register the services and repositories
    services.AddSingleton<IValidationService, ValidationService>();
    services.AddSingleton<IArtistRepository, ArtistRepository>();
    services.AddSingleton<IAlbumRepository, AlbumRepository>();
    services.AddSingleton<ISongRepository, SongRepository>();

    services.AddControllers();
    services.AddSwaggerGen(c =>
    {
        c.SwaggerDoc("v1", new OpenApiInfo { Title = "MediatRApi", Version = "v1" });
    });
}
```

## ASP.NET Core Web Application With MediatR

{%
    include image.html
    year='2021'
    month='02'
    file='solution-explorer-with-mediatr.png'
    alt='ASP.NET Core Project using MediatR'
    caption='ASP.NET Core Project using MediatR'
%}

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

{%
    include image.html
    year='2021'
    month='02'
    file='mediatr-nuget.png'
    alt='MediatR on Nuget'
    caption='MediatR on Nuget'
%}

### Requests

#### GetArtistRequest.cs

```csharp
public class GetArtistRequest : IRequest<Artist>
{
    public Guid ArtistId { get; set; }
}
```

#### CreateAlbumRequest.cs

```csharp
public class CreateAlbumRequest : IRequest<Guid>
{
    public Album Album { get; set; }
}
```

#### SetRatingRequest.cs

```csharp
public class SetRatingRequest : IRequest<Unit>
{
    public Guid SongId { get; set; }

    public int Rating { get; set; }
}
```

### Request Handlers

#### GetArtistHandler.cs

```csharp
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

#### CreateAlbumHandler.cs

```csharp
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

#### SetRatingHandler.cs

```csharp
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

### Dependency Injection

{%
    include image.html
    year='2021'
    month='02'
    file='mediatr-nuget-dependency-injection.png'
    alt='MediatR.Extensions.Microsoft.DependencyInjection on Nuget'
    caption='MediatR.Extensions.Microsoft.DependencyInjection on Nuget'
%}

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

#### Startup.cs *(extract)*

```csharp
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

## GitHub Repository

The code for this article is available at 
[github.com/kiltandcode/mediatr-demo](https://github.com/kiltandcode/mediatr-demo)