# Start Job

Start a job that has been initialized before. Please make sure that all files have been successfully uploaded before issuing this request.

**Method**: POST

**Request Body**:

```json
{
  "config": {
    "completeGenome": true, // Complete genome
    "compliant": true, // INDSC compliant
    "dermType": null, // (optional) Either empty or one of "UNKNOWN", "MONODERM", "DIDERM"
    "genus": "string", // Genus name
    "hasReplicons": true, // If true a PUT to uploadLinkReplicons must have been issued beforehand
    "keepContigHeaders": true, // Keep the contig header names
    "locus": "string", // Add locus name
    "locusTag": "string", // Add locus tag
    "minContigLength": 9007199254740991, // Minimal contig length
    "plasmid": "string", // --plasmid option
    "prodigalTrainingFile": "string", // If any string is provided a PUT to uploadLinkProdigal must have been issued before
    "species": "string", // Species name
    "strain": "string", // Strain
    "translationTable": 4 // Either 4 or 11
  },
  "job": {
    "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "secret": "string"
  }
}
```
For a more detailed description of all the config options please visit the corresponding [CLI docs](../cli/usage.md).

**Response Body**:

This request has no response body, a successfull request will be indicated by a `200` statuscode.

## Full Example (cURL)

```bash

curl -X 'POST' \
  'https://api.bakta.computational.bio/api/v1/job/start' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "config": {
    "completeGenome": true,
    "compliant": true,
    "dermType": null,
    "genus": "string",
    "hasReplicons": true,
    "keepContigHeaders": true,
    "locus": "string",
    "locusTag": "string",
    "minContigLength": 9007199254740991,
    "plasmid": "string",
    "prodigalTrainingFile": "string",
    "species": "string",
    "strain": "string",
    "translationTable": 1073741824
  },
  "job": {
    "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "secret": "string"
  }
}'
``` 