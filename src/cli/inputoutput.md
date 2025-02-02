# Input and Output

## Input

Bakta accepts bacterial genomes and plasmids (complete / draft assemblies) in (zipped) fasta format. For a full description of how further genome information can be provided and workflow customizations can be set, please have a look at the [Usage](#usage) section or this [manual](https://bakta.readthedocs.io/).

### Replicon meta data table

To fine-tune the very details of each sequence in the input fasta file, Bakta accepts a replicon meta data table provided in `csv` or `tsv` file format: `--replicons <file.tsv>`. Thus, complete replicons within partially completed draft assemblies can be marked & handled as such, *e.g.* detection & annotation of features spanning sequence edges.

Table format:

original sequence id  |  new sequence id  |  type  |  topology  |  name
----|----------------|----------------|----------------|----------------
`old id` | `new id`, `<empty>` | `chromosome`, `plasmid`, `contig`, `<empty>` | `circular`, `linear`, `<empty>` | `name`, `<empty>`

For each input sequence recognized via the `original locus id` a `new locus id`, the replicon `type` and the `topology` as well a `name` can be explicitly set.

Shortcuts:

- `chromosome`: `c`
- `plasmid`: `p`
- `circular`: `c`
- `linear`: `l`

`<empty>` values (`-` / ``) will be replaced by defaults. If **new locus id** is `empty`, a new contig name will be autogenerated.

Defaults:

- type: `contig`
- topology: `linear`

Example:

original locus id  |  new locus id  |  type  |  topology  |  name
----|----------------|----------------|----------------|----------------
NODE_1 | chrom | `chromosome` | `circular` | `-`
NODE_2 | p1 | `plasmid` | `c` | `pXYZ1`
NODE_3 | p2 | `p`  |  `c` | `pXYZ2`
NODE_4 | special-contig-name-xyz |  `-` | `-` | `-`
NODE_5 | `` |  `-` | `-` | `-`

### User-provided regions

Bakta accepts pre-annotated (*a priori*), user-provided feature regions via `--regions` in either GFF3 or GenBank format. These regions supersede all de novo-predicted regions, but are equally subject to the internal functional annotation process. Currently, only `CDS` are supported. A maximum overlap with *de novo*-predicted CDS of 30 bp is allowed. If you would like to provide custom functional annotations, you can provide these via `--proteins` which is described in the following section.

### User-provided protein sequences

Bakta accepts user-provided trusted protein sequences via `--proteins` in either GenBank (CDS features) or Fasta format which are used in the functional annotation process. Using the Fasta format, each reference sequence can be provided in a short or long format:

```bash
# short:
>id gene~~~product~~~dbxrefs
MAQ...

# long:
>id min_identity~~~min_query_cov~~~min_subject_cov~~~gene~~~product~~~dbxrefs
MAQ...
```

Allowed values:

field  |  value(s)  |  example
----|----------------|----------------
min_identity | `int`, `float` | 80, 90.3
min_query_cov | `int`, `float` | 80, 90.3
min_subject_cov | `int`, `float` | 80, 90.3
gene | `<empty>`, `string` | msp
product | `string` | my special protein
dbxrefs | `<empty>`, `db:id`, `,` separated list  | `VFDB:VF0511`

Protein sequences provided in short Fasta or GenBank format are searched with default thresholds of 90%, 80% and 80% for minimal identity, query and subject coverage, respectively.

### User-provided HMMs

Bakta accepts user-provided trusted HMMs via `--hmms` in HMMER's text format. If set, Bakta will adhere to the *trusted cutoff* specified in the HMM header. In addition, a max. evalue threshold of 1e-6 is applied. By default, Bakta uses the HMM description line as a product description. Further information can be provided via the HMM description line using the *short* format as explained above in the [User-provided protein sequences](####user-provided-protein-sequences) section.

```bash
# default
HMMER3/f [3.1b2 | February 2015]
NAME  id
ACC   id
DESC  product
LENG  435
TC    600 600

# short
NAME  id
ACC   id
DESC  gene~~~product~~~dbxrefs
LENG  435
TC    600 600
```

## Output

Annotation results are provided in standard bioinformatics file formats:

- `<prefix>.tsv`: annotations as simple human readble TSV
- `<prefix>.gff3`: annotations & sequences in GFF3 format
- `<prefix>.gbff`: annotations & sequences in (multi) GenBank format
- `<prefix>.embl`: annotations & sequences in (multi) EMBL format
- `<prefix>.fna`: replicon/contig DNA sequences as FASTA
- `<prefix>.ffn`: feature nucleotide sequences as FASTA
- `<prefix>.faa`: CDS/sORF amino acid sequences as FASTA
- `<prefix>.inference.tsv`: inference metrics (score, evalue, coverage, identity) for annotated accessions as TSV
- `<prefix>.hypotheticals.tsv`: further information on hypothetical protein CDS as simple human readble tab separated values
- `<prefix>.hypotheticals.faa`: hypothetical protein CDS amino acid sequences as FASTA
- `<prefix>.txt`: summary as TXT
- `<prefix>.png`: circular genome annotation plot as PNG
- `<prefix>.svg`: circular genome annotation plot as SVG
- `<prefix>.json`: all (internal) annotation & sequence information as JSON

The `<prefix>` can be set via `--prefix <prefix>`. If no prefix is set, Bakta uses the input file prefix.

Of note, Bakta provides all detailed (internal) information on each annotated feature in a standardized machine-readable JSON file `<prefix>.json`:

```json
{
    "genome": {
        "genus": "Escherichia",
        "species": "coli",
        ...
    },
    "stats": {
        "size": 5594605,
        "gc": 0.497,
        ...
    },
    "features": [
        {
            "type": "cds",
            "contig": "contig_1",
            "start": 971,
            "stop": 1351,
            "strand": "-",
            "gene": "lsoB",
            "product": "type II toxin-antitoxin system antitoxin LsoB",
            ...
        },
        ...
    ],
    "sequences": [
        {
            "id": "c1",
            "description": "[organism=Escherichia coli] [completeness=complete] [topology=circular]",
            "nt": "AGCTTT...",
            "length": 5498578,
            "complete": true,
            "type": "chromosome",
            "topology": "circular"
            ...
        },
        ...
    ]
}
```

Bakta provides a helper function to create above mentioned output files from the (GNU-zipped) *JSON* result file, thus helping potential long-term or large-scale annotation projects to reduce overall storage requirements.

```bash
bakta_io --output <output-path> --prefix <prefix> result.json.gz

bakta_io --help
```

Exemplary annotation result files for several genomes (mostly ESKAPE species) are hosted at Zenodo: [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.4770026.svg)](https://doi.org/10.5281/zenodo.4770026)