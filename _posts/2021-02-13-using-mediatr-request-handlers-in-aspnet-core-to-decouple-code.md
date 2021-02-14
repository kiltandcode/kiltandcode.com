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

Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.

[MediatR](https://github.com/jbogard/MediatR)
[Jimmy Bogard](https://github.com/jbogard)

This article isn't meant to be an in-depth explanation of the Mediator pattern, for that 
I'd recommend the series of articles over at [.NET Core Tutorials](https://dotnetcoretutorials.com/) 
beginning with 
[The Mediator Pattern In .NET Core – Part 1 – What’s A Mediator?](https://dotnetcoretutorials.com/2019/04/30/the-mediator-pattern-in-net-core-part-1-whats-a-mediator/). Instead, I thought I'd demonstrate 
the advantages of using the MediatR package within an API built using ASP.NET Core and C#, more specifically 
using it's request handlers to decouple your code and create a cleaner codebase.

https://github.com/kiltandcode/mediatr-demo

## ASP.NET Core Project Without using MediatR

{%
    include image.html
    year='2021'
    month='02'
    file='solution-explorer-without-mediatr.png'
    alt='ASP.NET Core Project without using MediatR'
    caption='ASP.NET Core Project without using MediatR'
%}

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

## ASP.NET Core Project using MediatR

{%
    include image.html
    year='2021'
    month='02'
    file='solution-explorer-with-mediatr.png'
    alt='ASP.NET Core Project using MediatR'
    caption='ASP.NET Core Project using MediatR'
%}

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

```csharp
public class GetArtistRequest : IRequest<Artist>
{
    public Guid ArtistId { get; set; }
}
```

```csharp
public class CreateAlbumRequest : IRequest<Guid>
{
    public Album Album { get; set; }
}
```

```csharp
public class SetRatingRequest : IRequest<Unit>
{
    public Guid SongId { get; set; }

    public int Rating { get; set; }
}
```

### Request Handlers

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

https://github.com/kiltandcode/mediatr-demo