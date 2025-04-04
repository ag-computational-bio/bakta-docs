# Full roundtrip example

This is a full roundtrip example it expects the file available at:
https://github.com/oschwengers/bakta/raw/refs/heads/main/test/data/GCF_000008865.2.fna.gz

to be downloaded to the local path `/tmp/GCF_000008865.2.fna.gz`.


## Step 1: Initialize a new job

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

This will initialize a new job and return a response that includes a job description and various authenticated S3 upload urls.

Example:
```json
{
  "job": {
    "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "secret": "exampleSecret"
  },
  "uploadLinkFasta": "https://s3.computational.bio.uni-giessen.de/some-link/inputdata.fasta",
  "uploadLinkProdigal": "https://s3.computational.bio.uni-giessen.de/some-link",
  "uploadLinkReplicons": "https://s3.computational.bio.uni-giessen.de/some-link"
}
```

All future API requests need to specify the jobID and the secret for authentication.

## Step 2: Upload Input Data

Use cURL to upload the fasta file at `/tmp/GCF_000008865.2.fna.gz`.

```bash
curl -X 'PUT' \
  'https://s3.computational.bio.uni-giessen.de/some-link/inputdata.fasta' \
  --data-binary "@/tmp/GCF_000008865.2.fna.gz"
```

Note: the `@` symbol is important here, it signalizes to cURL to use the content of a file and not the raw string `/tmp/GCF_000008865.2.fna.gz`. 

## Step 3: Start your job

Use the `/api/start` endpoint to start your job.

```bash
curl -X 'POST' \
  'https://api.bakta.computational.bio/api/v1/job/start' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "config": {
    "completeGenome": true
  },
  "job": {
    "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "secret": "exampleSecret"
  }
}'
```


## Step 4: Monitor your job and wait for completion


Use the `/api/list` endpoint to query the job status:


```bash
curl -X 'POST' \
  'https://api.bakta.computational.bio/api/v1/job/list' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "jobs": [
    {
      "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "secret": "exampleSecret"
    }
  ]
}'
```


The response will show a `Successful` state if your job completed successfully.


```json

{
  "jobs": [
    {
      "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "jobStatus": "SUCCESSFUL",
      "started": "2025-01-07T17:01:14Z",
      "updated": "2025-01-07T17:09:22Z",
      "name": "GCF_000008865.2.fna.gz"
    }
  ],
  "failedJobs": []
}
```

## Step 5: Retrieve your results

When your job was successful you can use the `/api/result` endpoint to retrieve results.


```bash

curl -X 'POST' \
  'https://api.bakta.computational.bio/api/v1/job/result' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "secret": "exampleSecret"
}'
```

This will return a list of json files:

```json
{
  "ResultFiles": {
    "EMBL": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "FAA": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "FAAHypothetical": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "FFN": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "FNA": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "GBFF": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "GFF3": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "JSON": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "PNGCircularPlot": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "SVGCircularPlot": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "TSV": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "TSVHypothetical": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "TSVInference": "https://s3.computational.bio.uni-giessen.de/resultfile",
    "TXTLogs": "https://s3.computational.bio.uni-giessen.de/resultfile"
  },
  "jobID": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "name": "GCF_000008865.2.fna.gz",
  "started": "2025-01-07T20:12:26.387Z",
  "updated": "2025-01-07T20:12:26.387Z"
}
```

Retrieve a result from **ResultFiles**:

```bash
curl 'https://s3.computational.bio.uni-giessen.de/resultfile'
```
