# Genome plots

Bakta allows the creation of circular genome plots via [pyCirclize](https://github.com/moshi4/pyCirclize). Plots are generated as part of the default workflow and saved as `PNG` and `SVG` files. In addition to the default workflow, Bakta provides a dedicated CLI entry point `bakta_plot`:

Examples:

```bash
bakta_plot input.json

bakta_plot --output test --prefix test --config config.yaml --sequences 1,2 input.json
```

It accepts the results of a former annotation process in JSON format and allows the selection of distinct sequences, either denoted by their `FASTA` identifiers or sequential number starting by 1. Colors for each feature type can be adopted via a simple configuration file in `YAML` format, *e.g.* [config.yaml](config.yaml). Currently, two default plot types are supported, *i.e.* `features` and `cog`. Examples for chromosomes and plasmids are provided in [here](https://github.com/oschwengers/bakta/tree/main/examples/)

## Usage

```bash
usage: bakta_plot [--config CONFIG] [--output OUTPUT] [--prefix PREFIX]
                  [--sequences SEQUENCES] [--type {features,cog}] [--label LABEL] [--size {4,8,16}] [--dpi {150,300,600}]
                  [--help] [--verbose] [--debug] [--tmp-dir TMP_DIR] [--version]
                  <input>

Rapid & standardized annotation of bacterial genomes, MAGs & plasmids

positional arguments:
  <input>               Bakta annotations in (zipped) JSON format

Input / Output:
  --config CONFIG, -c CONFIG
                        Plotting configuration in YAML format
  --output OUTPUT, -o OUTPUT
                        Output directory (default = current working directory)
  --prefix PREFIX, -p PREFIX
                        Prefix for output files

Plotting:
  --sequences SEQUENCES
                        Sequences to plot: comma separated number or name (default = all, numbers one-based)
  --type {features,cog}
                        Plot type: feature/cog (default = features)
  --label LABEL         Plot center label (for line breaks use '|')
  --size {4,8,16}       Plot size in inches: 4/8/16 (default = 8)
  --dpi {150,300,600}   Plot resolution as dots per inch: 150/300/600 (default = 300)

General:
  --help, -h            Show this help message and exit
  --verbose, -v         Print verbose information
  --debug               Run Bakta in debug mode. Temp data will not be removed.
  --tmp-dir TMP_DIR     Location for temporary files (default = system dependent auto detection)
  --version             show program's version number and exit
```

## Description

Currently, there are two types of plots: `features` (the default) and `cog`. In default mode (`features`), all features are plotted on two rings representing the forward and reverse strand from outer to inner, respectively using the following feature colors:

- CDS: `#cccccc`
- tRNA/tmRNA: `#b2df8a`
- rRNA: `#fb8072`
- ncRNA: `#fdb462`
- ncRNA-region: `#80b1d3`
- CRISPR: `#bebada`
- Gap: `#000000`
- Misc: `#666666`

In the `cog` mode, all protein-coding genes (CDS) are colored due to assigned COG functional categories. To better distinguish non-coding genes, these are plotted on an additional 3rd ring.

In addition, both plot types share two innermost GC content and GC skew rings. The first ring represents the GC content per sliding window over the entire sequence(s) in green (`#33a02c`) and red `#e31a1c` representing GC above and below average, respectively. The 2nd ring represents the GC skew in orange (`#fdbf6f`) and blue (`#1f78b4`). The GC skew gives hints on a replicon's replication bubble and hence, on the completeness of the assembly. On a complete & circular bacterial chromosome, you normally see two inflection points at the origin of replication and at its opposite region -> [Wikipedia](https://en.wikipedia.org/wiki/GC_skew)

Custom plot labels (text in the center) can be provided via `--label`:

```bash
bakta_plot --sequences 2 --dpi 300 --size 8 --prefix plot-cog-p2 --type cog --label="pO157|plasmid, 92.7 kbp"
```

![Plot example of Bakta test genome.](https://github.com/oschwengers/bakta/tree/main/examples/plot-cog-p2.png)
