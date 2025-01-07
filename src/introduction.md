[![DOI:10.1099/mgen.0.000685](https://zenodo.org/badge/DOI/10.1099/mgen.0.000685.svg)](https://doi.org/10.1099/mgen.0.000685)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.4247252.svg)](https://doi.org/10.5281/zenodo.4247252)
[![License: GPL v3](https://img.shields.io/badge/License-GPL%20v3-brightgreen.svg)](https://github.com/oschwengers/bakta/blob/master/LICENSE)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/bakta.svg)
![PyPI - Status](https://img.shields.io/pypi/status/bakta.svg)
![GitHub release](https://img.shields.io/github/release/oschwengers/bakta.svg)

[![PyPI](https://img.shields.io/pypi/v/bakta.svg)](https://pypi.org/project/bakta)
[![Conda](https://img.shields.io/conda/vn/bioconda/bakta.svg)](https://bioconda.github.io/recipes/bakta/README.html)
[![Docker Image Version](https://img.shields.io/docker/v/oschwengers/bakta?sort=semver&label=docker)](https://hub.docker.com/r/oschwengers/bakta)
[![Spack](https://img.shields.io/spack/v/py-bakta)](https://packages.spack.io/package.html?name=py-bakta)
[![Galaxy Toolshed - Tool Version](https://img.shields.io/galaxytoolshed/v/bakta/iuc/bakta?label=usegalaxy.eu)](https://usegalaxy.eu/root?tool_id=bakta)
[![Static Badge](https://img.shields.io/badge/bio.tools-v1.9.4-blue?link=https%3A%2F%2Fbio.tools%2Fbakta)](https://bio.tools/bakta)

# Bakta: rapid & standardized annotation of bacterial genomes, MAGs & plasmids

Welcome to the documentation of **Bakta** !

Bakta is a tool for the rapid & standardized annotation of bacterial genomes and plasmids from both isolates and MAGs. It provides **dbxref**-rich, **sORF**-including and taxon-independent annotations in machine-readable `JSON` & bioinformatics standard file formats for automated downstream analysis.

## Outline

This documentation contains three major sections:

- [**Bakta CLI**](./cli/installation.md): This contains the documentation how to use Bakta from the Command Line.
- [**Bakta Web**](./web.md): A quick guide for our free to use web annotation and visualization service at https://bakta.computational.bio
- [**Bakta API**](./api.md): Documentation and examples how to use the API that powers the web service

## Features

- **Comprehensive & taxonomy-independent database**
Bakta provides a large and taxonomy-independent database using UniProt's entire [UniRef](https://www.uniprot.org/uniref/) protein sequence cluster universe. Thus, it achieves favourable annotations in terms of sensitivity and specificity along the broad continuum ranging from well-studied species to unknown genomes from MAGs.

- **Protein sequence identification**
Bakta exactly identifies known identical protein sequences (**IPS**) from RefSeq and UniProt allowing the fine-grained annotation of gene alleles (`AMR`) or closely related but distinct protein families. This is achieved via an alignment-free sequence identification (**AFSI**) approach using full-length `MD5` protein sequence hash digests.

- **Fast**
This AFSI approach substantially accellerates the annotation process by avoiding computationally expensive homology searches for identified genes. Thus, Bakta can annotate a typical bacterial genome in 10 &plusmn;5 min on a laptop, plasmids in a couple of seconds/minutes.

- **Database cross-references**
Fostering the [FAIR](https://www.go-fair.org/fair-principles) principles, Bakta exploits its AFSI approach to annotate CDS with database cross-references (**dbxref**) to RefSeq (`WP_*`), UniRef100 (`UniRef100_*`) and UniParc (`UPI*`). By doing so, IPS allow the surveillance of distinct gene alleles and streamlining comparative analysis as well as posterior (external) annotations of `putative` & `hypothetical` protein sequences which can be mapped back to existing CDS via these exact & stable identifiers (*E. coli* gene [ymiA](https://www.uniprot.org/uniprot/P0CB62) [...more](https://www.uniprot.org/help/dubious_sequences)). Currently, Bakta identifies ~214.8 mio, ~199 mio and ~161 mio distinct protein sequences from UniParc, UniRef100 and RefSeq, respectively. Hence, for certain genomes, up to 99 % of all CDS can be identified this way, skipping computationally expensive sequence alignments.

- **FAIR annotations**
To provide standardized annotations adhearing to FAIR principles, Bakta utilizes a versioned custom annotation database comprising UniProt's [UniRef100 & UniRef90](https://www.uniprot.org/uniref/) protein clusters (FAIR -> [DOI](http://dx.doi.org/10.1038/s41597-019-0180-9)/[DOI](https://doi.org/10.1093/nar/gkaa1100)) enriched with dbxrefs (`GO`, `COG`, `EC`) and annotated by specialized niche databases. For each DB version we provide a comprehensive log file of all imported sequences and annotations.

- **Small proteins / short open reading frames**
Bakta detects and annotates small proteins/short open reading frames (**sORF**) which are not predicted by tools like `Prodigal`.

- **Expert annotation systems**
To provide high quality annotations for certain proteins of higher interest, *e.g.* AMR & VF genes, Bakta includes & merges different expert annotation systems. Currently, Bakta uses NCBI's AMRFinderPlus for AMR gene annotations as well as an generalized protein sequence expert system with distinct coverage, identity and priority values for each sequence, currenlty comprising the [VFDB](http://www.mgc.ac.cn/VFs/main.htm) as well as NCBI's [BlastRules](https://ftp.ncbi.nih.gov/pub/blastrules/).

- **Comprehensive workflow**
Bakta annotates ncRNA cis-regulatory regions, oriC/oriV/oriT and assembly gaps as well as standard feature types: tRNA, tmRNA, rRNA, ncRNA genes, CRISPR, CDS and pseudogenes.

- **GFF3 & INSDC conform annotations**
Bakta writes GFF3 and INSDC-compliant (Genbank & EMBL) annotation files ready for submission (checked via [GenomeTools GFF3Validator](http://genometools.org/cgi-bin/gff3validator.cgi), [table2asn_GFF](https://www.ncbi.nlm.nih.gov/genbank/genomes_gff/#run) and [ENA Webin-CLI](https://github.com/enasequence/webin-cli) for GFF3 and EMBL file formats, respectively for representative genomes of all ESKAPE species).

- **Bacteria & plasmids only**
Bakta was designed to annotate bacteria (isolates & MAGs) and plasmids, only. This decision by design has been made in order to tweak the annotation process regarding tools, preferences & databases and to streamline further development & maintenance of the software.

- **Reasoning**
By annotating bacterial genomes in a standardized, taxonomy-independent, high-throughput and local manner, Bakta aims at a well-balanced tradeoff between fully featured but computationally demanding pipelines like [PGAP](https://github.com/ncbi/pgap) and rapid highly customizable offline tools like [Prokka](https://github.com/tseemann/prokka). Indeed, Bakta is heavily inspired by Prokka (kudos to [Torsten Seemann](https://github.com/tseemann)) and many command line options are compatible for the sake of interoperability and user convenience. Hence, if Bakta does not fit your needs, please consider trying Prokka.

## License

Bakta and all of its components are licensed under the [GPL-3.0 license](https://github.com/oschwengers/bakta/blob/main/LICENSE). 

