---
title: What's new in ASP.NET Core 2.2
author: tdykstra
description: Learn about the new features in ASP.NET Core 2.2.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/03/2018
uid: aspnetcore-2.2
---
# What's new in ASP.NET Core 2.2

This article highlights the most significant changes in ASP.NET Core 2.2, with links to relevant documentation.

## Open API Analyzers & Conventions

Open API (also known as Swagger) is a language-agnostic specification for describing REST APIs. The Open API ecosystem has tools that allow for discovering, testing, and producing client code using the specification. Support for generating and visualizing Open API documents in ASP.NET Core MVC is provided via community driven projects such as [NSwag](https://github.com/RSuter/NSwag), and [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore). ASP.NET Core 2.2 provides improved tooling and runtime experiences for creating Open API documents.

For more information, see the following resources:

* <xref:tutorials/web-api-help-pages-using-swagger>
* [ASP.NET Core 2.2.0-preview1: Open API Analyzers & Conventions](https://blogs.msdn.microsoft.com/webdev/2018/08/23/asp-net-core-2-20-preview1-open-api-analyzers-conventions/).

## Endpoint Routing

ASP.NET Core 2.2 uses a new *endpoint routing* system for improved dispatching of requests. The changes include new link generation API members.

For more information, see [Differences between IRouter- and endpoint-based routing](xref:fundamentals/routing?view=aspnetcore-2.2#differences-from-earlier-versions-of-routing).

## Health checks

A new health checks service makes it easier to use ASP.NET Core in environments that require health checks, such as Kubernetes. Health checks includes middleware and a set of libraries that define an `IHealthCheck` abstraction and service.

Health checks are used by a container orchestrator or load balancer to quickly determine if a system is responding to requests normally. A container orchestrator might respond to a failing health check by halting a rolling deployment or restarting a container. A load balancer might respond to a health check by routing traffic away from the failing instance of the service.

Health checks are exposed by an application as an HTTP endpoint used by monitoring systems. Health checks can be configured for a variety of real-time monitoring scenarios and monitoring systems.

For more information, see [Health checks in ASP.NET Core](xref:host-and-deploy/health-checks).

## HTTP/2 in Kestrel

ASP.NET Core 2.2 adds support for HTTP/2. 

HTTP/2 is a major revision of the HTTP protocol. Some of the notable features of HTTP/2 are support for header compression and fully multiplexed streams over a single connection. While HTTP/2 preserves HTTP’s semantics (HTTP headers, methods, etc) it's a breaking change from HTTP/1.x on how this data is framed and sent over the wire.

As a consequence of this change in framing, servers and clients need to negotiate the protocol version used. Application-Layer Protocol Negotiation (ALPN) is a TLS extension that allows the server and client negotiate the protocol version used as part of their TLS handshake. While it is possible to have prior knowledge between the server and the client on the protocol, all major browsers support ALPN as the only way to establish an HTTP/2 connection.

For more information, see [HTTP/2 support](xref:fundamentals/servers/index?view=aspnetcore-2.2#http2-support).

## Kestrel configuration

In earlier versions of ASP.NET Core, Kestrel options are configured by calling `UseKestrel`. In 2.2, Kestrel options are configured by calling `ConfigureKestrel` on the host builder. This change resolves an issue with the order of `IServer` registrations for in-process hosting. For more information, see the following resources:

* [Mitigate UseIIS conflict](https://github.com/aspnet/KestrelHttpServer/issues/2760)
* [Configure Kestrel server options with ConfigureKestrel](xref:fundamentals/servers/kestrel?view=aspnetcore-2.2#how-to-use-kestrel-in-aspnet-core-apps)

## IIS in-process hosting

In earlier versions of ASP.NET Core, IIS serves as a reverse proxy. In 2.2, the ASP.NET Core Module can boot the CoreCLR and host an app inside the IIS worker process (*w3wp.exe*). In-process hosting provides significant performance and diagnostic gains when running with IIS.

For more information, see [IIS in-process hosting](xref:fundamentals/servers/aspnet-core-module?view=aspnetcore-2.2#in-process-hosting-model).

## SignalR Java client

ASP.NET Core 2.2 introduces a Java Client for SignalR. This client supports connecting to an ASP.NET Core SignalR Server from Java code, including Android apps.

For more information, see [ASP.NET Core SignalR Java client](https://docs.microsoft.com/aspnet/core/signalr/java-client?view=aspnetcore-2.2).

## CORS improvements

In earlier versions of ASP.NET Core, CORS Middleware allows `Accept`, `Accept-Language`, `Content-Language`, and `Origin` headers to be sent regardless of the values configured in `CorsPolicy.Headers`. In 2.2, a CORS Middleware policy match is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.

For more information, see [CORS Middleware](xref:security/cors?view=aspnetcore-2.2#set-the-allowed-request-headers).

## Response compression

ASP.NET Core 2.2 can compress responses with the [Brotli compression format](https://tools.ietf.org/html/rfc7932).

For more information, see [Response Compression Middleware supports Brotli compression](xref:performance/response-compression?view=aspnetcore-2.2#brotli-compression-provider).

## Project templates

ASP.NET Core web project templates were updated to [Bootstrap 4](https://getbootstrap.com/docs/4.1/migration/) and [Angular 6](https://blog.angular.io/version-6-of-angular-now-available-cc56b0efa7a4). The new look is visually simpler and makes it easier to see the important structures of the app.

![Home or Index page](~/tutorials/razor-pages/razor-pages-start/_static/home2.2.png)

## Validation performance

MVC’s validation system is designed to be extensible and flexible, allowing you to determine on a per request basis which validators apply to a given model. This is great for authoring complex validation providers. However, in the most common case an application only uses the built-in validators and don’t require this extra flexibility. Built-in validators include DataAnnotations such as [Required] and [StringLength], and `IValidatableObject`.

In ASP.NET Core 2.2, MVC can short-circuit validation if it determines that a given model graph doesn't require validation. Skipping validation results in significant improvements when validating models that can't or don't have any validators. This includes objects such as collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`), or complex object graphs without many validators.

## HTTP Client performance

In ASP.NET Core 2.2, the performance of `SocketsHttpHandler` was improved by reducing connection pool locking contention. For apps that make many outgoing HTTP requests, such as some microservices architectures, throughput is improved. Under load, `HttpClient` throughput can be improved by up to 60% on Linux and 20% on Windows.

For more information, see [the pull request that made this improvement](https://github.com/dotnet/corefx/pull/32568).

## Additional information

For the complete list of changes, see the [ASP.NET Core 2.2 Release Notes](https://github.com/aspnet/Home/releases/tag/2.2.0).
