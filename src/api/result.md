# Result

The request to retrieve the results of an annotation workflow.

**Method**: POST

**Request Body**:

```json
{
  "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "secret": "string"
}
```

**Response Body**:

```json
{
  "ResultFiles": {
    "EMBL": "string",
    "FAA": "string",
    "FAAHypothetical": "string",
    "FFN": "string",
    "FNA": "string",
    "GBFF": "string",
    "GFF3": "string",
    "JSON": "string",
    "PNGCircularPlot": "string",
    "SVGCircularPlot": "string",
    "TSV": "string",
    "TSVHypothetical": "string",
    "TSVInference": "string",
    "TXTLogs": "string"
  },
  "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "name": "string",
  "started": "2025-01-07T20:12:26.387Z",
  "updated": "2025-01-07T20:12:26.387Z"
}
```

The results response contains the usual job metadata (`jobID`, `name`, `started` and `updated`), as well as a `ResultsFiles` section, this section contains pre-authenticated URLs that can be used to retrieve the results of the job with a simple **GET** request. 

## Full Example (cURL)

```bash

curl -X 'POST' \
  'https://api.bakta.computational.bio/api/v1/job/result' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "secret": "string"
}'
``` 

Retrieve a result from **ResultFiles**:

```bash
curl '<ResultFilesUrl>'
```
