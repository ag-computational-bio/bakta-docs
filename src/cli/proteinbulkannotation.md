# Protein bulk annotation

For the direct bulk annotation of protein sequences aside from the genome, Bakta provides a dedicated CLI entry point `bakta_proteins`:

Examples:

```bash
bakta_proteins --db <db-path> input.fasta

bakta_proteins --db <db-path> --prefix test --output test --proteins special.faa --threads 8 input.fasta
```

## Output

Annotation results are provided in standard bioinformatics file formats:

- `<prefix>.tsv`: annotations as simple human readble TSV
- `<prefix>.faa`: protein sequences as FASTA
- `<prefix>.hypotheticals.tsv`: further information on hypothetical proteins as simple human readble tab separated values
- `<prefix>.json`: all (internal) annotation & sequence information as JSON

The `<prefix>` can be set via `--prefix <prefix>`. If no prefix is set, Bakta uses the input file prefix.

## Usage

```bash
usage: bakta_proteins [--db DB] [--output OUTPUT] [--prefix PREFIX] [--force]
                      [--proteins PROTEINS]
                      [--help] [--verbose] [--debug] [--threads THREADS] [--tmp-dir TMP_DIR] [--version]
                      <input>

Rapid & standardized annotation of bacterial genomes, MAGs & plasmids

positional arguments:
  <input>               Protein sequences in (zipped) fasta format

Input / Output:
  --db DB, -d DB        Database path (default = <bakta_path>/db). Can also be provided as BAKTA_DB environment variable.
  --output OUTPUT, -o OUTPUT
                        Output directory (default = current working directory)
  --prefix PREFIX, -p PREFIX
                        Prefix for output files
  --force, -f           Force overwriting existing output folder

Annotation:
  --proteins PROTEINS   Fasta file of trusted protein sequences for annotation

General:
  --help, -h            Show this help message and exit
  --verbose, -v         Print verbose information
  --debug               Run Bakta in debug mode. Temp data will not be removed.
  --threads THREADS, -t THREADS
                        Number of threads to use (default = number of available CPUs)
  --tmp-dir TMP_DIR     Location for temporary files (default = system dependent auto detection)
  --version, -V         show program's version number and exit
```
