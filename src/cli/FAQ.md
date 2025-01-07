# FAQ

- **AMRFinder fails**
If AMRFinder constantly crashes even on fresh setups and Bakta's database was downloaded manually, then AMRFinder needs to setup its own internal database. This is required only once: `amrfinder_update --force_update --database <bakta-db>/amrfinderplus-db`. You could also try Bakta's internal database download logic automatically taking care of this: `bakta_db download --output <bakta-db>`

- **DeepSig not found in Conda environment**
For the prediction of signal predictions, Bakta uses DeepSig that is currently not available for MacOS and only up to Bakta v1.9.4. Therefore, we decided to exclude DeepSig from Bakta's default Conda dependencies because otherwise it would not be installable on MacOS systems. On Linux systems it can be installed via `conda install -c conda-forge -c bioconda python=3.8 deepsig`.

- **Nice, but I'm mising XYZ...**
Bakta is quite new and we're keen to constantly improve it and further expand its feature set. In case there's anything missing, please do not hesitate to open an issue and ask for it!

- **Bakta is running too long without CPU load... why?**
Bakta takes advantage of an SQLite DB which results in high storage IO loads. If this DB is stored on a remote / network volume, the lookup of IPS/PSC annotations might take a long time. In these cases, please, consider moving the DB to a local volume or hard drive.