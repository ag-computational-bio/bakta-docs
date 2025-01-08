# Installation

Bakta can be installed via BioConda, Docker, Singularity and Pip. However, we encourage to use [Conda](https://conda.io/projects/conda/en/latest/user-guide/install/index.html) or [Docker](https://www.docker.com/get-started)/[Singularity](https://sylabs.io/singularity) to automatically install all required 3rd party dependencies.

In all cases a mandatory [database](#database-download) must be downloaded.

## BioConda

```bash
conda install -c conda-forge -c bioconda bakta
```

## Podman (Docker)

We maintain a Docker image `oschwengers/bakta` providing an entrypoint, so that containers can be used like an executable:

```bash
podman pull oschwengers/bakta
podman run oschwengers/bakta --help
```

Installation instructions and get-started guides: Podman [docs](https://podman.io/docs). For further convenience, we provide a shell script (`bakta-podman.sh`) handling Podman related parameters (volume mounting, user IDs, etc):

```bash
bakta-podman.sh --db <db-path> --output <output-path> <input>
```

For experienced users and full functionality (`bakta_db` & `bakta_proteins`), an image without entrypoint might be a better option. For these cases, please use one of the [Biocontainer](https://quay.io/repository/biocontainers/bakta?tab=tags) images:

```bash
export CONTAINER="quay.io/biocontainers/bakta:1.8.2--pyhdfd78af_0"
podman run -it --rm $CONTAINER bakta --help
podman run -it --rm $CONTAINER bakta_db --help
```

## Pip

```bash
python3 -m pip install --user bakta
```

Bakta requires the following 3rd party software tools which must be installed and executable to use the full set of features:

- tRNAscan-SE (2.0.11) <https://doi.org/10.1101/614032> <http://lowelab.ucsc.edu/tRNAscan-SE>
- Aragorn (1.2.41) <http://dx.doi.org/10.1093/nar/gkh152> <http://130.235.244.92/ARAGORN>
- INFERNAL (1.1.4) <https://dx.doi.org/10.1093%2Fbioinformatics%2Fbtt509> <http://eddylab.org/infernal>
- PILER-CR (1.06) <https://doi.org/10.1186/1471-2105-8-18> <http://www.drive5.com/pilercr>
- Pyrodigal (3.5.0) <https://doi.org/10.21105/joss.04296> <https://github.com/althonos/pyrodigal>
- PyHMMER (0.10.15) <https://doi.org/10.21105/joss.04296> <https://github.com/althonos/pyhmmer>
- Diamond (2.1.10) <https://doi.org/10.1038/nmeth.3176> <https://github.com/bbuchfink/diamond>
- Blast+ (2.14.0) <https://www.ncbi.nlm.nih.gov/pubmed/2231712> <https://blast.ncbi.nlm.nih.gov>
- AMRFinderPlus (4.0.3) <https://github.com/ncbi/amr>
- pyCirclize (1.7.0) https://github.com/moshi4/pyCirclize

## Database download

Bakta requires a mandatory database which is publicly hosted at Zenodo: [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.4247252.svg)](https://doi.org/10.5281/zenodo.4247252)
We provide 2 types: `full` and `light`. To get best annotation results and to use all features, we recommend using the `full` (default). If you seek for maximum runtime performance or if download time/storage requirements are an issue, please try the `light` version. Further information is provided in the [database](#database) section below.

List available DB versions (available as either `full` or `light`):

```shell
bakta_db list
...
```

To download the most recent compatible database version we recommend to use the internal database download & setup tool:

```bash
bakta_db download --output <output-path> --type [light|full]
```

Of course, the database can also be downloaded manually:

```bash
wget https://zenodo.org/record/10522951/files/db-light.tar.gz
tar -xzf db-light.tar.gz
rm db-light.tar.gz
```

If required, or desired, the AMRFinderPlus DB can also be updated manually:

```bash
amrfinder_update --force_update --database db-light/amrfinderplus-db/
```

If you're using bakta on Docker:

```bash
docker run -v /path/to/desired-db-path:/db --entrypoint /bin/bash oschwengers/bakta:latest -c "bakta_db download --output /db --type [light|full]"
```

As an additional data repository backup, we provide the most recent database version via our institute servers: [full](https://jlubox.uni-giessen.de/getlink/fiKeyT1huWv9vW5cXKYkZXYB/db.tar.gz), [light](https://jlubox.uni-giessen.de/getlink/fiG6AHmHA94t4v2r2vwW91WB/db-light.tar.gz). However, the bandwith is limited. Hence, please use it with caution and only if Zenodo might be temporarily uncreachable or slow. In these cases, please also download the AMRFinderPlus database as indicated above.

Update an existing database:

```bash
bakta_db update --db <existing-db-path> [--tmp-dir <tmp-directory>]
```

Update using Docker:

```bash
docker run -v /path/to/desired-db-path:/db --entrypoint /bin/bash oschwengers/bakta:latest -c "bakta_db update --db /db/db-[light|full]"
```

The database path can be provided either via parameter (`--db`) or environment variable (`BAKTA_DB`):

```bash
bakta --db <db-path> genome.fasta

export BAKTA_DB=<db-path>
bakta genome.fasta
```

For system-wide setups, the database can also be copied to the Bakta base directory:

```bash
cp -r db/ <bakta-installation-dir>
```

As Bakta takes advantage of AMRFinderPlus for the annotation of AMR genes, AMRFinder is required to setup its own internal databases in a `<amrfinderplus-db>` subfolder within the Bakta database `<db-path>`, once via `amrfinder_update --force_update --database <db-path>/amrfinderplus-db/`. To ease this process we recommend to use Bakta's internal download procedure.