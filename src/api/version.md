# Version

Retrieve the versions of all backend components, including the Bakta CLI, Database and backend.

**Method**: GET

**Request**:

This Request has no body.

**Response Body**:

```json
{
  "toolVersion": "1.10.3",
  "dbVersion": "5.1.0",
  "backendVersion": "0.6.4"
}
```

Versions of the bakta tool `toolVersion`, the bakta database `dbVersion` and the backend `backendVersion`.

## Full Example (cURL)

```bash

curl -X 'GET' \
  'https://api.staging.bakta.computational.bio/api/v1/version' \
  -H 'accept: application/json'
``` 