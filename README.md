# MultiAuthority.AccessTokenValidation
This project was copied from the IdentityServer4.AccessTokenValidation implementation.  
It removed the introspection scheme.  It lets you add many JWTBearer schemes where the client has to add a custom header to indicate which scheme to use.

```
Body:

Authorization:Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJodHRwOi8vc2NoZW1hcy54bWxzb2FwLm9yZy93cy8yMDA1LzA1L2lkZW50aXR5L2NsYWltcy9uYW1lIjoiYm9iIiwic3ViIjoiYm9iIiwiYXVkIjpbInlvdXJkb21haW4uY29tIiwibml0cm8iXSwiZXhwIjoxNTI1MzU4ODcxLCJpc3MiOiJ5b3VyZG9tYWluLmNvbSJ9.5DJ2Nw7TLo2XpnORXPtGnhtAcBaSj5AJApbd8e-nVt8
x-authScheme:Self

```

## Usage 
```
List<SchemeRecord> schemeRecords = new List<SchemeRecord>()
{
    new SchemeRecord()
    {
        Name = "One",
        JwtBearerOptions = options =>
        {
            options.Authority = "https://p7identityserver4.azurewebsites.net";
            options.RequireHttpsMetadata = false;
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuer = true,
                ValidateAudience = true,
                ValidateLifetime = true,
                ValidateIssuerSigningKey = true,
                ValidAudiences = new List<string>()
                {
                    "nitro"
                }
            };
        }
    },
    new SchemeRecord()
    {
        Name = "Two",
        JwtBearerOptions = options =>
        {
            options.Authority = "https://p7identityserver4two.azurewebsites.net";
            options.RequireHttpsMetadata = false;
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuer = true,
                ValidateAudience = true,
                ValidateLifetime = true,
                ValidateIssuerSigningKey = true,
                ValidAudiences = new List<string>()
                {
                    "metal"
                }
            };
        }
    },
    new SchemeRecord()
    {
        Name = "Self",
        JwtBearerOptions = options =>
        {
                       
            options.TokenValidationParameters = new TokenValidationParameters
            {
                ValidateIssuer = true,
                ValidIssuer = "yourdomain.com",
                ValidateAudience = true,
                ValidateLifetime = true,
                ValidateIssuerSigningKey = true,
                ValidAudiences = new List<string>()
                {
                    "nitro"
                },
                IssuerSigningKey = issuerSigningKey
            };
        }
    }
};

services.AddAuthentication("Bearer")
    .AddMultiAuthorityAuthentication(schemeRecords, options =>
    {
 
    });
```


# Credit

[IdentityServer4.AccessTokenValidation](https://github.com/IdentityServer/IdentityServer4.AccessTokenValidation)
