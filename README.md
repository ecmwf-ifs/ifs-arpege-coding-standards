# IFS-Arpège coding standards

![License](https://img.shields.io/github/license/ecmwf-ifs/ifs-arpege-coding-standards)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/ecmwf-ifs/ifs-arpege-coding-standards/build.yml)

This repository contains the source for the shared IFS-Arpège coding standards
jointly developed by ECMWF and Météo-France.

The standards and their applicability are as follows:

1. `fortran` - applicable to both IFS and Arpège
1. `python` (#todo) - applicable to IFS only
1. `shell` - applicable to IFS only

## Build locally

1. Set up virtual env:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   python3 -m pip install --upgrade pip
   ```
1. Install pre-requisites:
   ```bash
   python3 -m pip install -r requirements.txt
   ```
1. Build for a given language:
   ```bash
   make -C <language> html
   ```
   where `<language>` is one of `suites`, `fortran`, `python` or `shell`.
1. View the result:
   ```bash
   open <language>/_build/html/index.html
   ```
   (or similar for your OS/platform).

## Deployment (ECMWF)

Build and deployment of all the coding standards to
<https://sites.ecmwf.int/docs/ifs-arpege-coding-standards/>
is triggered automatically by pushing to `main`. See
<https://github.com/ecmwf-ifs/ifs-arpege-coding-standards/blob/main/.github/workflows/build.yml>.

## Licence

This repository is distributed under the Apache License 2.0. In applying this
licence, ECMWF does not waive the privileges and immunities granted to it by
virtue of its status as an intergovernmental organisation nor does it submit to
any jurisdiction.

## Contributing

Contributions are welcome. Please open an issue where a proposed change can be
discussed. Then create a pull request with your contribution and sign the
contributors license agreement (CLA).
