---

## ✨ Looking For Sponsors ✨

FastEndpoints needs sponsorship to [sustain the project](https://github.com/FastEndpoints/FastEndpoints/issues/449). Please help out if you can.

---

[//]: # (<details><summary>title text</summary></details>)

## New 🎉

<details><summary>Jwt token revocation middleware</summary>

Jwt token revocation can be easily implemented with the newly provided abstract class like so:

```csharp
public class JwtBlacklistChecker(RequestDelegate next) : JwtRevocationMiddleware(next)
{
    protected override Task<bool> JwtTokenIsValidAsync(string jwtToken, CancellationToken ct)
    { 
        //return true if the supplied token is still valid
    }
}
```

Simply register it before any auth related middleware like so:

```csharp
app.UseJwtRevocation<JwtBlacklistChecker>()
   .UseAuthentication()
   .UseAuthorization()
```

</details>

<details><summary>Ability to override JWT Token creation options per request for Refresh Tokens</summary>

todo: write docs + description here

ref: https://discord.com/channels/933662816458645504/1258013749948977273/1260889170876956682

</details>

<details><summary>Ability to subscribe to gRPC Events from Blazor Wasm projects</summary>

Until now, only gRPC Command initiations were possible from within Blazor Wasm projects. Support has been added to the `FastEndpoints.Messaging.Remote.Core` project which is capable of running in the browser to be able to act as a subscriber for Event broadcasts from a gRPC server. [See here](https://github.com/FastEndpoints/Blazor-Wasm-Remote-Messaging-Demo) for a sample project showcasing both.

</details>

## Improvements 🚀

<details><summary>Get rid of Swagger middleware ordering requirements</summary>

Swagger middleware ordering is no longer important. You can now place the `.SwaggerDocument()` and `.UseSwaggerGen()` calls wherever you prefer.

</details>

## Fixes 🪲

<details><summary>Swagger generator issue with [FromBody] properties</summary>

The referenced schema was generated as a `OneOf` instead of a direct `$ref` when a request DTO property was being annotated with the `[FromBody]` attribute.

</details>

<details><summary>Swagger route parameter detection issue</summary>

The Nswag operation processor did not correctly recognize route parameters in the following form:

```csharp
api/a:{id1}:{id2}
```

Which has now been corrected thanks to PR #735

</details>

<details><summary>Kiota client generation issue with 'clean output' setting</summary>

If the setting for cleaning the output folder was enabled, Kiota client generation was throwing an error that it can't find the input swagger json file, because Kiota deletes everything in the output folder when that setting is enabled. From now on, if the setting is enabled, the swagger json file will be created in the system temp folder instead of the output folder.

</details>

<details><summary>Graceful shutdown issue of gRPC streaming</summary>

Server & Client Streaming was not listening for application shutdown which made app shutdown to wait until the streaming was finished. It has been fixed to be able to gracefully terminate the streams if the application is shutting down.

</details>

## Minor Breaking Changes ⚠️