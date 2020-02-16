# TigerGraph Yelp Dataset Solution

Contains the GSQL scripts and TigerGraph solution tarball to import and model the Yelp challenge dataset using
TigerGraph.

## Getting Started

### Starting TigerGraph

The TigerGraph Docker container is built from the official TigerGraph image. Original instructions can be found 
[here](https://github.com/tigergraph/ecosys/blob/master/guru_scripts/docker/README.md). The following commands
will get TigerGraph up and running:

* `docker-compose up` to get the Docker container up. The name of the container is set to `tigergraph-yelp`.
* Open a shell to the TigerGraph server using the following command: `ssh -p 14022 tigergraph@localhost` with
  password `tigergraph`. A convience script called `connect` calls this command.
* Start the TigerGraph service using `gadmin start` (may take up to 1 minute).

Now, one will have access to the following:
* GSQL shell: Make use of the GSQL shell be calling `gsql`.
* GraphStudio: Open a browser to `http://localhost:14240` for TigerGraph's GraphStudio.

### Importing the Yelp Solution File

From the GraphStudio home, select "Import An Existing Solution" and select `yelp-solution.tar.gz`. This file
contains a basic Yelp schema, a few example queries, and the geo-grid code from the Graph Gurus episode 
[Location, Location, Location](https://www.youtube.com/watch?v=gPF_SXibDxw).

Note: After importing the solution, your GraphStudio may not load Design Schema and additional menus. If this
occurs, simply reset `gadmin` by calling `gadmin stop` followed by `gadmin start`.

### Importing the Yelp Dataset

Before the Yelp dataset can be imported, it needs to be converted to CSV. `yelp-loader.gsql` is based on the 
data produced from my 
[Yelp dataset normalization and feature selection](https://github.com/DavidBakerEffendi/yelp-normalization)
script.

Once the dataset has been normalized and converted to CSV then run `./transfer_data` which will move these 
CSV files and `yelp-loader.gsql` into the Docker container. `./transfer_data` assumes this directory is on the
same level as [`yelp-normalization`](https://github.com/DavidBakerEffendi/yelp-normalization). Once SSH'd in
to the Docker container using `./connect`, run `gsql` and then the following command:
```
GSQL > @yelp-loader.gsql
```
This will begin the batch offline data loading job. Remember to edit the file paths in `yelp-loader.gsql` before
starting each job.
