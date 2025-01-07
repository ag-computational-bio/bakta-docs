# Init Job

Initialize a new bakta annotation job.

**Method**: POST

**Request Body**:

```json
{
  "name": "string",
  "repliconTableType": "CSV"
}
```

The body contains a user-defined name and the type of the replicon table (either `TSV` or `CSV`).

**Response Body**:

```json
{
  "job": {
    "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "secret": "string"
  },
  "uploadLinkFasta": "string",
  "uploadLinkProdigal": "string",
  "uploadLinkReplicons": "string"
}
```
The `job` section of the response contains a `UUIDv4` (jobID) and a corresponding `secret`, this must be stored and provided by all subsequent requests that are associated to this job. 

The response also contains three pre-authenticated S3 Urls:

``uploadLinkFasta`` should be used to upload the (fasta) sequence data for annotation.
``uploadLinkProdigal`` (optional) can be used to upload an additional prodigal training file
``uploadLinkReplicons`` (optional) should be used to upload a replicon table in ``tsv`` format that describes the provided replicons in the fasta input file

By issuing a **PUT** request with the associated data as body you can upload the necessary data needed for the initialization.

**NOTE:** Previous API versions required the upload of all three files (FASTA, prodigal.tf and replicons.tsv) even if they were not used, this is no longer necessary as long these are not used in the subsequent start request.

## Full Example (cURL)

Init request:

```bash
curl -X 'POST' \
  'https://api.bakta.computational.bio/api/v1/job/init' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "name": "test",
  "repliconTableType": "TSV"
}'
``` 

Put sequence data (required):

```bash
curl -X 'PUT' \
  '<uploadLinkFasta>' \
  --data-binary "@path/to/file.fasta"
```

Put prodigal training file (optional):

```bash
curl -X 'PUT' \
  '<uploadLinkProdigal>' \
  --data-binary "@path/to/file.tf"
```

Put replicon table (optional):

```bash
curl -X 'PUT' \
  '<uploadLinkReplicons>' \
  --data-binary "@path/to/file.tsv"
```