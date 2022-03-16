# Assignment

Your task is to create a login form (e-mail, password) which will authenticate the user over the Hakka API <https://apidev.hakka.eu/api/v1/docs> using the <https://angular.io> framework. Validate the e-mail. Notify the user about wrong credentials.

Users on hakka can have access to multiple companies (Tenants), so after the user is authenticated, you should present him with a tenant picker. Once a tenant is picked, visualize it's remaining wallet balance.

Cover the functionality with a Cypress test suite <https://www.cypress.io>.

Publish the code in a public Git repository on <https://github.com>.

Hakka uses OAuth2 for authentication, you can find your credentials together with a few curl examples that should get you going below (note the `x-tenant-id` header on the wallet balance call):

```bash
curl -X POST 'https://apidev.hakka.eu/oauth/v2/token' \
  -H 'Accept: application/ld+json'\
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data 'username=job.candidate@hakka.eu&password=gjsM4g6qcZqanzzU&grant_type=password&client_id=38158571-1da9-11ea-b12a-525400eeb47f_3ch0ns9nb2wwsgg8gskw88sos080k04ckko4sg4sssk4g08o0w&client_secret=8d4hqi49kzk04c400oc04s0kk8owo0wgws8o40wwcsgw4cssw'
```

```json
{
  "access_token": "MzljNTE5ZWZhYThjZDUxMjQyZmNiZDRjMjgyNDgwYTkxZDE1YmYxNDRmYzYwNjA4NzA3MmEzMWI2YzY0ODlmZA",
  "expires_in": 3600,
  "token_type": "bearer",
  "scope": null,
  "refresh_token": "M2UzODgyN2EwMmU4NzVhNzU4MDMwOTI3M2EyMDRjMGY4ODBkZmYzN2E0ZGM2YjM4NjQ1ZjBiMmJkYTcxMjgwYQ"
}
```

```bash
curl -X GET "https://apidev.hakka.eu/api/v1/me/tenants" \
  -H "accept: application/ld+json" \
  -H "authorization: Bearer MzljNTE5ZWZhYThjZDUxMjQyZmNiZDRjMjgyNDgwYTkxZDE1YmYxNDRmYzYwNjA4NzA3MmEzMWI2YzY0ODlmZA"
```

```json
{
  "@context": "/api/v1/contexts/Tenant",
  "@id": "/api/v1/tenants",
  "@type": "hydra:Collection",
  "hydra:member": [
    {
      "@id": "/api/v1/tenants/3a746ac3-1d9d-11ea-b12a-525400eeb47f",
      "@type": "Tenant",
      "id": "3a746ac3-1d9d-11ea-b12a-525400eeb47f",
      "name": "Trucking Company Netherlands",
      ...
    },
    {
      "@id": "/api/v1/tenants/a3b970d9-1d9c-11ea-b12a-525400eeb47f",
      "@type": "Tenant",
      "id": "a3b970d9-1d9c-11ea-b12a-525400eeb47f",
      "name": "Trucking Company Belgium",
      ...
    }
  ],
  "hydra:totalItems": 2,
  ...
}
```

```bash
curl -X GET "https://apidev.hakka.eu/api/v1/me/wallet" \
  -H "accept: application/ld+json" \
  -H 'X-tenant-id: a3b970d9-1d9c-11ea-b12a-525400eeb47f' \
  -H "authorization: Bearer MzljNTE5ZWZhYThjZDUxMjQyZmNiZDRjMjgyNDgwYTkxZDE1YmYxNDRmYzYwNjA4NzA3MmEzMWI2YzY0ODlmZA"
```

```json
{
  "@context": "/api/v1/contexts/Wallet",
  "@id": "/api/v1/wallets/a42728d2-1d9c-11ea-b12a-525400eeb47f",
  "@type": "Wallet",
  "credits": 250,
  "reservedCredits": 0,
  "warningThreshold": 125,
  "id": "a42728d2-1d9c-11ea-b12a-525400eeb47f"
}
```

Good luck!
