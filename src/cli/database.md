# Database

The Bakta database comprises a set of AA & DNA sequence databases as well as HMM & covariance models.
At its core Bakta utilizes a compact read-only SQLite DB storing protein sequence digests, lengths, pre-assigned annotations and dbxrefs of UPS, IPS and PSC from:

- **UPS**: UniParc / UniProtKB (289,894,428)
- **IPS**: UniProt UniRef100 (270,638,882)
- **PSC**: UniProt UniRef90 (119,631,901)
- **PSCC**: UniProt UniRef50 (3,134,924)

This allows the exact protein sequences identification via MD5 digests & sequence lengths as well as the rapid subsequent lookup of related information. Protein sequence digests are checked for hash collisions while the DB creation process. IPS & PSC have been comprehensively pre-annotated integrating annotations & database *dbxrefs* from:

- NCBI nonredundant proteins (IPS: 192,288,757)
- NCBI COG DB (PSC: 3,513,643)
- KEGG Kofams (PSC: 19,818,290)
- SwissProt EC/GO terms (PSC: 336,656)
- NCBI NCBIfams (PSC: 17,308,678)
- PHROG (PSC: 11,243)
- NCBI AMRFinderPlus (IPS: 7,611)
- ISFinder DB (IPS: 137,670, PSC: 12,380)
- Pfam families (PSC: 687,250)

To provide high quality annotations for distinct protein sequences of high importance (AMR, VF, *etc*) which cannot sufficiently be covered by the IPS/PSC approach, Bakta provides additional expert systems. For instance, AMR genes, are annotated via NCBI's AMRFinderPlus.
An expandable alignment-based expert system supports the incorporation of high quality annotations from multiple sources. This currenlty comprises NCBI's BlastRules as well as VFDB and will be complemented with more expert annotation sources over time. Internally, this expert system is based on a Diamond DB comprising the following information in a standardized format:

- source: *e.g.* BlastRules
- rank: a precedence rank
- min identity
- min query coverage
- min model coverage
- gene lable
- product description
- dbxrefs

Rfam covariance models:

- ncRNA: 802
- ncRNA cis-regulatory regions: 270

ori sequences:

- oriC/V: 6,690
- oriT: 502

To provide FAIR annotations, the database releases are SemVer versioned (w/o patch level), *i.e.* `<major>.<minor>`. For each version we provide a comprehensive log file tracking all imported sequences as well as annotations thereof. The DB schema is represented by the `<major>` digit and automatically checked at runtime by Bakta in order to ensure compatibility. Content updates are tracked by the `<minor>` digit.

As this taxonomic-untargeted database is fairly demanding in terms of storage consumption, we also provide a lightweight DB type providing all non-coding feature information but only PSCC information from UniRef50 clusters for CDS. If download bandwiths or storage requirements become an issue or if shorter runtimes are favored over more-specific annotation, the `light` DB will do the job.

Latest database version: 5.1
DB types:

- `light`: 1.4 Gb zipped, 3.4 Gb unzipped, MD5: 31b3fbdceace50930f8607f8d664d3f4
- `full`: 37 Gb zipped, 71 Gb unzipped, MD5: f8823533b789dd315025fdcc46f1a8c1

All database releases are hosted at Zenodo: [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.4247252.svg)](https://doi.org/10.5281/zenodo.4247252)