# Delete Job

Delete a job including all of its data. 

**NOTE**: This is a destructive action that cannot be undone.

**Method**: DELETE

**Request**:

This Request has no body, but requires the user to provide a specific job via query parameters `jobId` and `secret`.

**Response Body**:

This request has no response body, a successfull request will be indicated by a `200` statuscode.

## Full Example (cURL)

```bash

curl -X 'DELETE' \
  'https://api.staging.bakta.computational.bio/api/v1/job/delete?secret=test&jobID=957f4923-0b18-413d-b705-51b54015864d'
``` 