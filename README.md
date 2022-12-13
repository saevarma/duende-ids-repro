# Duende IDS repro

Repo to repro `iss` authorization response param issue in Duende IDS.

## Re-produce

Set `EmitIssuerIdentificationResponseParameter` option on `AddIdentityServer` in `HostingExtensions.cs`

```
builder.Services.AddIdentityServer(options =>
    {
        // https://docs.duendesoftware.com/identityserver/v6/fundamentals/resources/api_scopes#authorization-based-on-scopes
        options.EmitStaticAudienceClaim = true;

        // Disable `iss` parameter in authorization repsonses.
        options.EmitIssuerIdentificationResponseParameter = false; // 👈 Add this
    })
    .AddInMemoryIdentityResources(Config.IdentityResources)
    .AddInMemoryApiScopes(Config.ApiScopes)
    .AddInMemoryClients(Config.Clients)
    .AddTestUsers(TestUsers.Users);
```

Start the server:

```
# In src/Identityserver run
dotnet run
```

Navigate to https://localhost:5001/.well-known/openid-configuration in browser and verify that `authorization_response_iss_parameter_supported` is set to `true`

### Optionally
Test with postman or similiar tool to test Athorization Code flow and verify that `iss` parameter is removed from authorization response.
