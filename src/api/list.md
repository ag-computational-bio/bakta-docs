# List

List the status of all jobs provided.

**Method**: POST

**Request Body**:

```json
{
  "jobs": [
    {
      "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "secret": "secret-job-1"
    },
    {
      "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "secret": "secret-job-2"
    }
  ]
}
```

The request contains a list for all jobs you want to have a status update (potentially all), including their secrets.

**Response Body**:

```json

{
  "jobs": [
    {
      "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "jobStatus": "SUCCESSFUL",
      "started": "2025-01-07T17:01:14Z",
      "updated": "2025-01-07T17:09:22Z",
      "name": "result_1_.fna"
    }
  ],
  "failedJobs": []
}
```

The response contains two sections: `jobs` a list with all jobs including the following fields:

- **jobID**: Job UUID
- **jobStatus**: Status of the job
    - **INIT**: Job has not started yet, either not started or queued due to high demand
    - **RUNNING**: Job is currently running
    - **SUCCESSFUL**: Job has successfully annotated the sequence
    - **ERROR**: Either malformed inputs/sequences or an internal server error, query [logs](./logs.md) for deeper information
- **started**: Started timestamp
- **updated**: Updated timestamp
- **name**: Provided name of the job

The `failedJobs` section contains jobs that could not be returned. This can have two reasons: A wrong secret `UNAUTHORIZED` or a wrong id / deleted job `NOT_FOUND`

## Full Example (cURL)

```bash
curl -X 'POST' \
  'https://api.bakta.computational.bio/api/v1/job/list' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "jobs": [
    {
      "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "secret": "string"
    }
  ]
}'
``` 