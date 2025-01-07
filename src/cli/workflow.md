# Annotation Workflow

## RNAs

1. tRNA genes: tRNAscan-SE 2.0
2. tmRNA genes: Aragorn
3. rRNA genes: Infernal vs. Rfam rRNA covariance models
4. ncRNA genes: Infernal vs. Rfam ncRNA covariance models
5. ncRNA cis-regulatory regions: Infernal vs. Rfam ncRNA covariance models
6. CRISPR arrays: PILER-CR

Bakta distinguishes ncRNA genes and (cis-regulatory) regions in order to enable the distinct handling thereof during the annotation process, *i.e.* feature overlap detection.

ncRNA gene types:

- sRNA
- antisense
- ribozyme
- antitoxin

ncRNA (cis-regulatory) region types:

- riboswitch
- thermoregulator
- leader
- frameshift element

## Coding sequences

The structural prediction is conducted via Pyrodigal and complemented by a custom detection of sORF < 30 aa. In addition, superseding regions of pre-predicted CDS can be provided via `--regions`.

To rapidly identify known protein sequences with exact sequence matches and to conduct a comprehensive annotations, Bakta utilizes a compact read-only SQLite database comprising protein sequence digests and pre-assigned annotations for millions of known protein sequences and clusters.

Conceptual terms:

- **UPS**: unique protein sequences identified via length and MD5 hash digests (100% coverage & 100% sequence identity)
- **IPS**: identical protein sequences comprising seeds of UniProt's UniRef100 protein sequence clusters
- **PSC**: protein sequences clusters comprising seeds of UniProt's UniRef90 protein sequence clusters
- **PSCC**: protein sequences clusters of clusters comprising annotations of UniProt's UniRef50 protein sequence clusters

**CDS**:

1. De novo-prediction via Pyrodigal respecting sequences' completeness (distinct prediction for complete replicons and uncompleted contigs)
2. Discard spurious CDS via AntiFam
3. Detect translational exceptions (selenocysteines)
4. Import of superseding user-provided CDS regions (optional)
5. Detection of UPSs via MD5 digests and lookup of related IPS and PCS
6. Sequence alignments of remainder via Diamond vs. PSC (query/subject coverage=0.8, identity=0.5)
7. Assignment to UniRef90 or UniRef50 clusters if alignment hits achieve identities larger than 0.9 or 0.5, respectively
8. Execution of expert systems:
   - AMR: AMRFinderPlus
   - Expert proteins: NCBI BlastRules, VFDB
   - User proteins (optionally via `--proteins <Fasta/GenBank>`)
9. Prediction of signal peptides (optionally via `--gram <+/->`)
10. Detection of pseudogenes:
   1. Search for reference PCSs using `hypothetical` CDS as seed sequences
   2. Translated alignment (blastx) of reference PCSs against up-/downstream-elongated CDS regions
   3. Analysis of translated alignments and detection of pseudogenization causes & effects
11. Combination of IPS, PSC, PSCC and expert system information favouring more specific annotations and avoiding redundancy

CDS without IPS or PSC hits as well as those without gene symbols or product descriptions different from `hypothetical` will be marked as `hypothetical`.

Such hypothetical CDS are further analyzed:

1. Detection of Pfam domains, repeats & motifs
2. Calculation of protein sequence statistics, *i.e.* molecular weight, isoelectric point

**sORFs**:

1. Custom sORF detection & extraction with amino acid lengths < 30 aa
2. Apply strict feature type-dependent overlap filters
3. discard spurious sORF via AntiFam
4. Detection of UPS via MD5 hashes and lookup of related IPS
5. Sequence alignments of remainder via Diamond vs. an sORF subset of PSCs (coverage=0.9, identity=0.9)
6. Exclude sORF without sufficient annotation information
7. Prediction of signal peptides (optionally via `--gram <+/->`)

sORF not identified via IPS or PSC will be discarded. Additionally, all sORF without gene symbols or product descriptions different from `hypothetical` will be discarded.
Due due to uncertain nature of sORF prediction, only those identified via IPS / PSC hits exhibiting proper gene symbols or product descriptions different from `hypothetical` will be included in the final annotation.

## Miscellaneous

1. Gaps: in-mem detection & annotation of sequence gaps
2. oriC/oriV/oriT: Blast+ (cov=0.8, id=0.8) vs. [MOB-suite](https://github.com/phac-nml/mob-suite) oriT & [DoriC](http://tubic.org/doric/public/index.php) oriC/oriV sequences. Annotations of ori regions take into account overlapping Blast+ hits and are conducted based on a majority vote heuristic. Region edges are fuzzy - use with caution!