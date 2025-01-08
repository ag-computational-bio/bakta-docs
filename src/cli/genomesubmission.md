# Genome Submission

Most genomes annotated with Bakta should be ready-to-submid to INSDC member databases GenBank and ENA. As a first step, please register your BioProject (e.g. PRJNA123456) and your locus_tag prefix (*e.g.* ESAKAI).

```bash
# annotate your genome in `--compliant` mode:
$ bakta --db <db-path> -v --genus Escherichia --species "coli O157:H7" --strain Sakai --complete --compliant --locus-tag ESAKAI test/data/GCF_000008865.2.fna.gz
```

## GenBank

Genomes are submitted to GenBank via Fasta (`.fna`) and SQN files. Therefore, `.sqn` files can be created with NCBI's new [table2asn](https://ftp.ncbi.nlm.nih.gov/asn1-converters/by_program/table2asn/) tool via Bakta's `.gff3` files.
Please, have a look at the [documentation]((https://www.ncbi.nlm.nih.gov/genbank/genomes_gff)) and have all additional files (template.txt) prepared:

```bash
# download table2asn for Linux
$ wget https://ftp.ncbi.nlm.nih.gov/asn1-converters/by_program/table2asn/linux64.table2asn.gz
$ gunzip linux64.table2asn.gz

# or MacOS
$ wget https://ftp.ncbi.nlm.nih.gov/asn1-converters/by_program/table2asn/mac.table2asn.gz
$ gunzip mac.table2asn.gz

$ chmod 755 linux64.table2asn.gz mac.table2asn.gz

# create the SQN file:
$ linux64.table2asn -Z -W -M n -J -c w -t template.txt -V vbt -l paired-ends -i GCF_000008865.2.fna -f GCF_000008865.2.gff3 -o GCF_000008865.2.sqn
```

## ENA

Genomes are submitted to ENA as EMBL (`.embl`) files via EBI's [Webin-CLI](https://ena-docs.readthedocs.io/en/latest/submit/general-guide/webin-cli.html) tool.
Please have all additional files (manifest.tsv, chrom-list.tsv) prepared as described [here](https://ena-docs.readthedocs.io/en/latest/submit/fileprep/assembly.html#flat-file).

```bash
# download ENA Webin-CLI
$ wget https://github.com/enasequence/webin-cli/releases/download/8.1.0/webin-cli-8.1.0.jar

$ gzip -k GCF_000008865.2.embl
$ gzip -k chrom-list.tsv
$ java -jar webin-cli-8.1.0.jar -submit -userName=<LOGIN> -password <PWD> -context genome -manifest manifest.tsv
```

Exemplarey manifest.tsv and chrom-list.tsv files might look like:

```bash
$ cat manifest.tsv
STUDY    PRJEB44484
SAMPLE    ERS6291240
ASSEMBLYNAME    GCF
ASSEMBLY_TYPE    isolate
COVERAGE    100
PROGRAM    SPAdes
PLATFORM    Illumina
MOLECULETYPE    genomic DNA
FLATFILE    GCF_000008865.2.embl.gz
CHROMOSOME_LIST    chrom-list.tsv.gz

$ cat chrom-list.tsv
contig_1    contig_1    circular-chromosome
contig_2    contig_2    circular-plasmid
contig_3    contig_3    circular-plasmid
```