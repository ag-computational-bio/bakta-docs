# Logs

This API endpoint can be used to retrieve STDOUT / STDERR logs from both running and already finished jobs. This is intended to be used to get a better understanding about the annotation process and to make it easier to debug errors that might come from invalid user input.

**Method**: GET

**Request**:

This Request has no body, but requires the user to provide a specific job via query parameters `jobId` and `secret`.

**Response Body**:

The response contains the stdout/stderr output of the specified job.

**Example responses**:

If an internal server occurs that is not associated with wrong or malformed user-input the following message will be returned:
```bash
Internal server error, please contact the administrator or try again later.
```

A regular Bakta stdout output, it will always start with a print of the issued command.

```bash
Bakta Command: bakta --tmp-dir /cache --threads 8 --prefix result -o /results --db /db/db --replicons /data/replicons.tsv --gram ? /data/fastadata.fasta --force
Parse genome sequences...
	imported: 86
	filtered & revised: 86
	contigs: 86

Start annotation...
predict tRNAs...
	found: 78
predict tmRNAs...
	found: 1
predict rRNAs...
	found: 3
predict ncRNAs...
	found: 221
predict ncRNA regions...
	found: 54
predict CRISPR arrays...
	found: 2
predict & annotate CDSs...
	predicted: 4586 
	discarded spurious: 6
	revised translational exceptions: 3
	detected IPSs: 4412
	found PSCs: 137
	found PSCCs: 11
	lookup annotations...
	conduct expert systems...
		amrfinder: 16
		protein sequences: 690
	combine annotations and mark hypotheticals...
	detect pseudogenes...
		candidates: 26
		verified: 15
	analyze hypothetical proteins: 72
		detected Pfam hits: 1 
		calculated proteins statistics
	revise special cases...
detect & annotate sORF...
	detected: 59639
	discarded due to overlaps: 47954
	discarded spurious: 9
	detected IPSs: 92
	found PSCs: 11
	lookup annotations...
	filter and combine annotations...
	filtered sORFs: 88
detect gaps...
	found: 0
detect oriCs/oriVs...
	found: 5
detect oriTs...
	found: 0
apply feature overlap filters...
select features and create locus tags...
	selected: 5014
improve annotations...
	revised gene symbols: 43

Genome statistics:
	Genome size: 4,879,557 bp
	Contigs/replicons: 86
	GC: 50.6 %
	N50: 146,704
	N90: 32,820
	N ratio: 0.0 %
	coding density: 88.6 %

annotation summary:
	tRNAs: 77
	tmRNAs: 1
	rRNAs: 3
	ncRNAs: 221
	ncRNA regions: 54
	CRISPR arrays: 2
	CDSs: 4574
		hypotheticals: 70
		pseudogenes: 15
	sORFs: 77
	gaps: 0
	oriCs/oriVs: 5
	oriTs: 0

Export annotation results to: /results
	human readable TSV...
	GFF3...
	INSDC GenBank & EMBL...
	genome sequences...
	feature nucleotide sequences...
	translated CDS sequences...
	feature inferences...
	circular genome plot...
	hypothetical TSV...
	translated hypothetical CDS sequences...
	machine readable JSON...
	Genome and annotation summary...

If you use these results please cite Bakta: https://doi.org/10.1099/mgen.0.000685
Annotation successfully finished in 7:26 [mm:ss].
```

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
