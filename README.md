# musrfit Installer for Ubuntu 24.04

This repository provides a Bash installer for setting up ROOT and musrfit on Ubuntu 24.04.

The script installs the required build dependencies, downloads the precompiled CERN ROOT package, clones the musrfit source code, checks out a fixed musrfit ref for reproducible Ubuntu 24.04 LTS builds, builds musrfit, installs it into the ROOT directory, and adds the required environment variables to the user's shell profile.

In a typical environment, this script can complete a fast musrfit installation within about ***five minutes***, depending mainly on your internet speed when downloading ROOT.

## Quick Start

1. Clone this repository.

   ```bash
   cd ~
   git clone https://github.com/empressfiona01-eng/Fast-Installation-for-Musrfit
   cd Fast-Installation-for-Musrfit
   ```

2. Run the installer as a normal user instead of `root` to avoid later permission problems when editing files in your musrfit working environment.

   ```bash
   chmod +x install_musrfit_ubuntu24.04.sh
   ./install_musrfit_ubuntu24.04.sh
   ```

3. After the success message, open a new shell or manually source `.bashrc`, then musrfit is available. Start musrfit by running `musredit`.

   ```bash
   source ~/.bashrc
   musredit
   ```

   This step is required because a `.sh` script runs in a child shell. Environment changes made there cannot directly update the parent shell you are using.

## Author

EC from Fudan University

Please leave your comments or suggestions in this GitHub project.

## ROOT and musrfit Versions

By default, the script downloads the official CERN precompiled ROOT package for Ubuntu 24.04:

```bash
https://root.cern/download/root_v6.40.02.Linux-ubuntu24.04-x86_64-gcc13.3.tar.gz
```

This package is installed under `~/apps/root` by default. You can override the download URL with `ROOT_URL` if needed:

```bash
ROOT_URL=<another_root_tarball_url> ./install_musrfit_ubuntu24.04.sh
```

The script builds musrfit from the official Bitbucket repository, but it does not automatically install the latest upstream source by default. To keep Ubuntu 24.04 LTS installation reproducible with the system Qt6 packages, the default musrfit source ref is fixed:

```bash
MUSRFIT_REF=6ed33d65
```

To try another musrfit commit or tag, override it when running the installer:

```bash
MUSRFIT_REF=<another_commit_or_tag> ./install_musrfit_ubuntu24.04.sh
```

The exact musrfit version installed is printed in the installation report as the short commit hash and the latest commit line.

## Supported System

- Ubuntu 24.04
- WSL2 Ubuntu 24.04 is recommended for musrfit users on Windows.

 If another Linux version is detected, the script will ask whether to continue and may work as well.

## What Will Be Installed

- ROOT from CERN
- musrfit from the official Bitbucket repository, checked out at the configured `MUSRFIT_REF`
- Build tools and libraries required by ROOT and musrfit
- Environment variables for ROOT and musrfit

Default installation paths:

```bash
~/apps/root
~/apps/musrfit
```

## Installed Environment Variables

The script adds the following block to `~/.bashrc` and `~/.bash_profile`:

```bash
export ROOTSYS=~/apps/root
export PATH=$ROOTSYS/bin:$PATH
export MUSRFITPATH=$ROOTSYS/bin
```

## Verify the Installation

After running `source ~/.bashrc`, check the installed programs:

```bash
root-config --version
which musrfit
which musredit
which musrview
```

Start the graphical musrfit editor:

```bash
musredit
```

## Notes

- The script uses `sudo` only when system packages or linker configuration need administrator permission.
- Do not run the script with `sudo` or as the root user. Otherwise, some installed or generated files may belong to `root`, and you may later lose permission to edit them as a normal user.
- After installation, open a new shell or run `source ~/.bashrc` before starting `musredit`. A `.sh` script runs in a child Bash process, so sourcing inside the script does not update your current parent shell.
- A stable internet connection is required to download ROOT packages and clone musrfit.
- The script does not claim to install the latest musrfit by default. It uses the configured `MUSRFIT_REF`; check the installation report for the actual commit.
