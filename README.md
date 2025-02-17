[![ExascaleSandboxTests](https://github.com/NOAA-GSL/ExascaleWorkflowSandbox/actions/workflows/docker-slurm.yml/badge.svg)](https://github.com/NOAA-GSL/ExascaleWorkflowSandbox/actions/workflows/docker-slurm.yml)

```
This repository is a scientific product and is not official communication
of the National Oceanic and Atmospheric Administration, or the United States
Department of Commerce. All NOAA GitHub project code is provided on an ‘as
is’ basis and the user assumes responsibility for its use. Any claims against
the Department of Commerce or Department of Commerce bureaus stemming from
the use of this GitHub project will be governed by all applicable Federal
law. Any reference to specific commercial products, processes, or service
by service mark, trademark, manufacturer, or otherwise, does not constitute
or imply their endorsement, recommendation or favoring by the Department of
Commerce. The Department of Commerce seal and logo, or the seal and logo of
a DOC bureau, shall not be used in any manner to imply endorsement of any
commercial product or activity by DOC or the United States Government.
```

# Overview

NOTE: If you are reading this with a plain text editor, please note that this document is
formatted with Markdown syntax elements.  See https://www.markdownguide.org/cheat-sheet/
for more information.

This repository is a collection of tools and demonstrations used to explore
and test various technologies for implementing exascale scientific workflows.
This collection of resources is not intended for production use, and is for
research purposes only.

# Installation

This software can be installed on Linux systems.  NOTE: MacOS is not supported
because the Flux scheduler does not support MacOS.  It can be used, however,
on Macs in a container.  See below for instructions for building and using
the Docker container.

## Install the Chiltepin conda environment

You will need a conda (miniconda3, anaconda, conda-forge, etc) installation.
If you do not have one, you can use the script included in the docker directory
to install it:

```
cd docker/install
./install_miniconda.sh <installation path>
```

The chiltepin conda environment can be installed using the environment
configuration in the docker directory:

```
cd docker/install
conda env create --name chiltepin --file chiltepin.yml
```

## Building and running Chiltepin container

Chiltepin provides a Docker container environment for building and running Parsl and Chiltepin
applications. It makes use of docker compose to build a multi-node Slurm cluster for use as a
backend for running the applications.  This repository is mounted from the host into the container's
chiltepin directory.

To build the container:

```
cd docker
docker compose -f docker-compose.yml up -d --build
```

To use the container after it is built and up, log in with a bash shell:

```
docker exec -it frontend bash -l
```

Once in the container, you can activate the chiltepin environment, install chiltepin in
editable mode, and run the tests

```
conda activate chiltepin
cd chiltepin
pip install -e .
cd tests
pytest --assert=plain --config=chiltepin.yaml
```

NOTE: Depending on how many cores your machine has and how many you've allocated to Docker,
you may need to modify the `cores per node` setting in the configuration yaml file to match
your machine's specifications to get all tests to pass.

# Running Parsl apps

The Chiltepin conda environment must be activated to run Parsl Chiltepin applications

```
conda activate chiltepin
```

# Running the test suite
The test suite is run with `pytest` and requires an editable installation of the Chiltepin
repository.  Before running the test suite:

```
cd <repository root>
pip install -e .
```

Once chiltepin has been installed with `pip`, the tests can be run with:

```
pytest --assert=plain --config=<config.yaml>
```

Where `<config.yaml>` is a configuration file specific to the test platform.  For examples,
look in `tests/chiltepin.yaml`, `tests/ci.yaml`, and `hercules.yaml`.
