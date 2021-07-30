# Testing seattleflu/assembly pipeline

Helping a colleague reviewing/testing the [seattleflu/assembly pipeline](https://github.com/seattleflu/assembly)

## Peruse to repo

Good signs:

- Has a [separate config file for the pipeline](https://github.com/seattleflu/assembly/blob/master/config/config.json) (and a few already configured config files for ["all refs"](https://github.com/seattleflu/assembly/blob/master/config/config-all-refs.json) and ["flu only"](https://github.com/seattleflu/assembly/blob/master/config/config-flu-only.json))
- [Has a configuration file for running on a HPC](https://github.com/seattleflu/assembly/blob/master/config/cluster.json) - so should be able to be run on ESR's production network
- It uses [trimmomatic](http://www.usadellab.org/cms/?page=trimmomatic) for trimming and [bowtie2](http://bowtie-bio.sourceforge.net/bowtie2/index.shtml) for alignment which are pretty standard choices in tools
- Has several contributor's and a few stars on the github repo which is usually a good sign
- Actively maintained - it has recent commits
- Good user documentation which makes life no so treacherous
- Has a [conda env](https://github.com/seattleflu/assembly/blob/master/envs/seattle-flu-environment.yaml) to (easily) install all software the pipeline will use
- Looks like it has a [jupyter notebook for plotting/exploring results](https://github.com/seattleflu/assembly/blob/master/scripts/2019-01-30%20Seattle%20flu-seq%20coverage%20statistics.ipynb) which can be nice for the user
- Comes with a [bunch of reference genomes](https://github.com/seattleflu/assembly/tree/master/references) for alignment which is nice (so user doesn't need to find and manually download them)

## Test the pipeline

# How to guide for running this pipeline

