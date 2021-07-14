|License: GPL v3| |PyPI - Python Version| |GitHub release| |PyPI| |PyPI
- Status| |Conda| |DOI|

Welcome to Bakta's documentation!
=================================

Bakta is a tool for the rapid & standardized annotation of bacterial genomes & plasmids. It provides **dbxref**-rich and **sORF**-including annotations in machine-readable ``JSON`` & bioinformatics standard file formats for automatic downstream analysis.


This documentations contains useful information how to use and install Bakta and its corresponding CLI and web versions.

Contents
--------

.. toctree::
   :maxdepth: 2
   :hidden:
   
   CLI documentation <BAKTA>

.. toctree::
   :maxdepth: 2
   :hidden:
   
   REST-API documentation <REST-API>
   
   
Features
--------

-  **Bacteria & plasmids only** Bakta was designed to annotate bacteria and plasmids, only. This decision by design has been made in order to tweak the annotation process regarding tools, preferences & databases and to streamline further development & maintenance of the software.

-  **FAIR annotations** To provide standardized annotations adhearing to `FAIR <https://www.go-fair.org/fair-principles>`__ principles, Bakta   utilizes a comprehensive & versioned custom annotation database based on UniProt's `UniRef100 & UniRef90 <https://www.uniprot.org/uniref/>`__ protein clusters (``FAIR`` -> `DOI <http://dx.doi.org/10.1038/s41597-019-0180-9>`__/`DOI <https://doi.org/10.1093/nar/gkaa1100>`__) enriched with dbxrefs (``GO``, ``COG``, ``EC``) and annotated by specialized niche databases. For each db version we provide a comprehensive log file of all imported sequences and annotations.

-  **Protein sequence identification** Fostering the FAIR aspect, Bakta identifies identical protein sequences (**IPS**) via ``MD5`` hash digests which are annotated with database cross-references (**dbxref**) to RefSeq (``WP_*``), UniRef100 (``UniRef100_*``) and UniParc (``UPI*``). By doing so, IPS allow the surveillance of distinct gene alleles and streamlining comparative analysis as well as posterior (external) annotations of ``putative`` & ``hypothetical`` protein sequences which can be mapped back to existing CDS via these exact & stable identifiers (*E. coli* gene `ymiA <https://www.uniprot.org/uniprot/P0CB62>`__ `...more <https://www.uniprot.org/help/dubious_sequences>`__). Currently, Bakta identifies ~198 mio, ~185 mio and ~150 mio distinct protein sequences from UniParc, UniRef100 and RefSeq, respectively. Hence, for certain genomes, up to 99 % of all CDS can be identified this way, skipping computationally expensive sequence alignments.

-  **Small proteins / short open reading frames** Bakta detects and annotates small proteins/short open reading frames (**sORF**) which are not predicted by tools like ``Prodigal``.

-  **Fast** Bakta can annotate a typical bacterial genome in 10 ±5 min on a laptop, plasmids in a couple of seconds/minutes.

-  **Expert annotation systems** To provide high quality annotations for certain proteins of higher interest, *e.g.* AMR & VF genes, Bakta includes & merges different expert annotation systems. Currently, Bakta uses NCBI's AMRFinderPlus for AMR gene annotations as well as a generalized protein sequence expert system with distinct coverage, identity and priority values for each sequence, currenlty comprising the `VFDB <http://www.mgc.ac.cn/VFs/main.htm>`__ as well as NCBI's `BlastRules <https://ftp.ncbi.nih.gov/pub/blastrules/>`__.

-  **Comprehensive workflow** Bakta annotates ncRNA cis-regulatory regions, oriC/oriV/oriT and assembly gaps as well as standard feature types: tRNA, tmRNA, rRNA, ncRNA genes, CRISPR, CDS.

-  **GFF3 & INSDC conform annotations** Bakta writes GFF3 and INSDC-compliant (Genbank & EMBL) annotation files ready for submission (checked via `GenomeTools GFF3Validator <http://genometools.org/cgi-bin/gff3validator.cgi>`__ and `ENA Webin-CLI <https://github.com/enasequence/webin-cli>`__ for GFF3 and EMBL file formats, respectively for representative genomes of all ESKAPE species).

-  **Reasoning** By annotating bacterial genomes in a standardized, taxon-independent, high-throughput and local manner, Bakta aims at a well-balanced tradeoff between fully-featured but computationally demanding pipelines like `PGAP <https://github.com/ncbi/pgap>`__ and rapid highly-customizable offline tools like `Prokka <https://github.com/tseemann/prokka>`__. Indeed, Bakta is heavily inspired by Prokka (kudos to `Torsten Seemann <https://github.com/tseemann>`__) and many command line options are compatible for the sake of interoperability and user convenience. Hence, if Bakta does not fit your needs, please try Prokka.
   
Citation
--------

A manuscript is in preparation. To temporarily cite our work, please transitionally refer to: 

.. admonition:: Citation

   **Schwengers et al. (2021). Bakta:** Rapid & standardized annotation of bacterial genomes & plasmids. GitHub: https://github.com/oschwengers/bakta

Bakta is standing on the shoulder of giants taking advantage of many publicly available databases. If you find any of those used within Bakta useful, please credit these primary sources, as well:

-  UniProt: https://doi.org/10.1093/nar/gky1049
-  RefSeq: https://doi.org/10.1093/nar/gkx1068
-  Rfam: https://doi.org/10.1002/cpbi.51
-  AMRFinder: https://doi.org/10.1128/AAC.00483-19
-  ISFinder: https://doi.org/10.1093/nar/gkj014
-  AntiFam: https://doi.org/10.1093/database/bas003
-  Mob-suite: https://doi.org/10.1099/mgen.0.000206
-  DoriC: https://doi.org/10.1093/nar/gky1014
-  COG: https://doi.org/10.1093/bib/bbx117
-  VFDB: https://doi.org/10.1093/nar/gky1080

--------------
* :ref:`search`

.. |License: GPL v3| image:: https://img.shields.io/badge/License-GPL%20v3-brightgreen.svg
   :target: https://github.com/oschwengers/bakta/blob/master/LICENSE
.. |PyPI - Python Version| image:: https://img.shields.io/pypi/pyversions/bakta.svg
.. |GitHub release| image:: https://img.shields.io/github/release/oschwengers/bakta.svg
.. |PyPI| image:: https://img.shields.io/pypi/v/bakta.svg
   :target: https://pypi.org/project/bakta
.. |PyPI - Status| image:: https://img.shields.io/pypi/status/bakta.svg
.. |Conda| image:: https://img.shields.io/conda/v/bioconda/bakta.svg
   :target: https://bioconda.github.io/recipes/bakta/README.html
.. |DOI| image:: https://zenodo.org/badge/DOI/10.5281/zenodo..svg
   :target: https://doi.org/10.5281/zenodo.

