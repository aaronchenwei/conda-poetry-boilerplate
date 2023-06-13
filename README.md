# conda-poetry-boilerplate <!-- omit in toc -->

This repos is inspired by a [stackoverflow thread](https://stackoverflow.com/questions/70851048/does-it-make-sense-to-use-conda-poetry).

## 1. First-time setup

We can avoid playing with the bootstrap env and simplify the example below if we have `conda-lock`, `mamba` and `poetry` already installed outside your target environment.

Below is the bootstrap environment with example envrionment specified in file `environment.yaml`.

```yaml
name: conda_poetry_boilerplate
channels:
  - conda-forge
  # We want to have a reproducible setup, so we don't want default channels,
  # which may be different for different users. All required channels should
  # be listed explicitly here.
  - nodefaults
dependencies:
  - python=3.11.*  # or don't specify the version and use the latest stable Python
  - mamba
  - pip  # pip must be mentioned explicitly, or conda-lock will fail
  - poetry  # or 1.1.*, or no version at all -- as you want

# Non-standard section listing target platforms for conda-lock:
platforms:
  - linux-64
```

Execute follow instructions.

```sh
# Create a bootstrap env
$ conda create -p /tmp/bootstrap -c conda-forge mamba conda-lock poetry
$ conda activate /tmp/bootstrap

# Create Conda lock file(s) from environment.yml
$ conda-lock -k explicit --conda mamba
# Set up Poetry
$ poetry init --python=~3.11 # version spec should match the one from environment.yml
# Add conda-lock (and other packages, as needed) to pyproject.toml and poetry.lock
$ poetry add --lock conda-lock --group dev

# Remove the bootstrap env
$ conda deactivate
$ rm -rf /tmp/bootstrap
```

## 2. Usage

### 2.1. Creating the environment

```sh
$ conda create --name conda_poetry_boilerplate --file conda-linux-64.lock
$ conda activate conda_poetry_boilerplate
$ poetry install
```

### 2.2. Activating the environment

```sh
$ conda activate conda_poetry_boilerplate
```

### 2.3. Updating the environment

```sh
# Re-generate Conda lock file(s) based on environment.yml
$ conda-lock -k explicit --conda mamba
# Update Conda packages based on re-generated lock file
$ mamba update --file conda-linux-64.lock
# Update Poetry packages and re-generate poetry.lock
$ poetry update
```
