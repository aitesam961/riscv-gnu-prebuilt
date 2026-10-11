# soft-sky: Pre-Built toolchain for RISCV
Up-to-date Prebuilt RISCV GNU toolchain for painless setup. No more build from source everytime. The purpose of this effort is to save time and computer resources for users and a contribution to eco-friendly computing practices.

### Supported Environment
Prebuilt riscv-gnu-toolchain binaries for x86_64 host running Linux.
Built on Ubuntu 24.04 LTS  and tested on `Ubuntu 24.04 LTS` and `Ubuntu 22.04 LTS`

This repository makes use of GitHub actions to build the `riscv-gnu-toolchain` from source. Just download the prebuilt binaries, add to path and use.

### Build revisions

Workflows build pinned source revisions. Monthly runs rebuild those revisions;
they do not automatically upgrade the compiler. Each archive includes
`build-info.txt` with source commits, configuration flags, and compiler version.
The CVA6 archive also records the tuning patch checksum.

To upgrade the standard toolchains, update `TOOLCHAIN_REF` in all standard
workflows together with the repository's toolchain submodule revision. For CVA6,
update `CVA6_REF`, `TOOLCHAIN_CONFIG`, `GCC_VERSION`, and the three component
commit IDs together. The fetch step checks that the upstream release tags resolve
to those component commits before applying the patch. Run the affected workflows
and check their smoke tests before distributing the updated archives.

### Workflow conventions

Toolchain workflows follow `rv64gc_baremetal.yaml`: Ubuntu 24.04, disk cleanup,
pinned sources, parallel builds under `$HOME/riscv`, smoke tests, build metadata,
and `.tar.gz` artifacts retained for 40 days. Tar archives preserve executable
permissions and symlinks. Existing manual-only workflows remain manual-only.
Linux workflows build with `make linux`; their smoke tests cross-link executables
without running them on the x86_64 host. Multilib checks cover both RV32 and RV64.

RV32-only standard builds use `riscv32-unknown-elf-*`; RV64 and bare-metal multilib
builds use `riscv64-unknown-elf-*`. Ibex also provides `riscv64-unknown-elf-*`
compatibility aliases. Linux builds use `riscv64-unknown-linux-gnu-*`.
`cva6_tc.yaml` retains the CVA6 builder and tuning patch (`riscv-none-elf-*`);
`rv64_gc_cva6.yaml` builds the standard RV64GC compiler without that tuning patch.

### Available Configurations

Choose a configuration below. Names link to the upstream project; **Build status**
links to its Actions runs, and **Source** opens the workflow definition.

| Configuration | ISA / ABI | Details | Build status | Source |
| :--- | :--- | :--- | :---: | :---: |
| [RV64 Newlib](https://github.com/riscv-collab/riscv-gnu-toolchain) | `rv64gc_zicsr_zifencei`<br>`lp64d` | — | [![RV64 Newlib build](https://github.com/aitesam961/riscv-gnu-prebuilt/actions/workflows/rv64gc_baremetal.yaml/badge.svg)](https://github.com/aitesam961/riscv-gnu-prebuilt/actions/workflows/rv64gc_baremetal.yaml) | [YAML](.github/workflows/rv64gc_baremetal.yaml) |
| [SCR1 Newlib](https://github.com/syntacore/scr1) | `rv32imc_zicsr_zifencei`<br>`ilp32` | Monthly + manual | [![SCR1 Newlib build](https://github.com/aitesam961/riscv-gnu-prebuilt/actions/workflows/scr1_tc.yaml/badge.svg)](https://github.com/aitesam961/riscv-gnu-prebuilt/actions/workflows/scr1_tc.yaml) | [YAML](.github/workflows/scr1_tc.yaml) |
| [Ibex Newlib](https://github.com/lowRISC/ibex) | `rv32imc_zicsr_zifencei`<br>`ilp32` | Compatibility aliases | [![Ibex Newlib build](https://github.com/aitesam961/riscv-gnu-prebuilt/actions/workflows/ibex_tc.yaml/badge.svg)](https://github.com/aitesam961/riscv-gnu-prebuilt/actions/workflows/ibex_tc.yaml) | [YAML](.github/workflows/ibex_tc.yaml) |
| [CVA6 · tuned](https://github.com/openhwfoundation/cva6) | `gcc-13.1.0-baremetal` | GCC 13.1.0; `-mtune=cva6` | [![CVA6 · tuned build](https://github.com/aitesam961/riscv-gnu-prebuilt/actions/workflows/cva6_tc.yaml/badge.svg)](https://github.com/aitesam961/riscv-gnu-prebuilt/actions/workflows/cva6_tc.yaml) | [YAML](.github/workflows/cva6_tc.yaml) |
| [Multilib · baremetal](https://github.com/riscv-collab/riscv-gnu-toolchain) | RV32 + RV64<br>Multiple ABIs | Newlib; monthly + manual | [![Multilib · baremetal build](https://github.com/aitesam961/riscv-gnu-prebuilt/actions/workflows/multi-lib.yaml/badge.svg)](https://github.com/aitesam961/riscv-gnu-prebuilt/actions/workflows/multi-lib.yaml) | [YAML](.github/workflows/multi-lib.yaml) |

**Downloading a build:** click its status badge, open a successful run, and select
its `.tar.gz` package under **Artifacts** (GitHub sign-in required). These are
compiler archives, not container images. Artifacts are retained for **40 days**.

Badges update automatically using [GitHub's workflow status](https://docs.github.com/en/actions/how-tos/monitor-workflows/add-a-status-badge).
They normally show the default branch; if it has no runs, GitHub uses the latest
run across branches. A passing badge records build success, but does **not**
guarantee that the archive is still available: it may have expired or been deleted.
A live availability checkbox would require automation that checks for a completed,
successful run and a matching, unexpired artifact through the GitHub API;
Markdown checkboxes alone do not update automatically.

If you require a custom configuration, consider building yourself or open an Issue with tag:`Enhancement`

### Installation
1- Download prebuilt binaries from GitHub [Actions](https://github.com/aitesam961/riscv-gnu-prebuilt/actions) > Artifacts

2- Create directory for sources. Preferred `/home/usr/` to avoid the possibility of toolchain affecting any of the system files.

```
$ sudo mkdir /home/usr/riscv    # Create a new directory
```
3- Extract the downloaded file and paste in newly created directory.
```
$ sudo tar -xzf riscv-toolchain.tar.gz -C /home/usr/riscv/
```
4- Add Path to your system environment variables
```
$ nano .bashrc                  # Open bashrc
$ export PATH=/home/usr/riscv/riscv-toolchain/bin:$PATH     # Add the path at the end
```

5- Save `.bashrc` (Ctrl+O, ENTER, Ctrl+X)

6- Apply Changes
```
$ source .bashrc
```
7- Add execute permissions to the source folder
```
$ sudo chmod -R +x /home/usr/riscv/riscv-toolchain/
```
### Verify installation 
Run the following Command
```
$ riscv64-unknown-elf-gcc -v
```
- You should get
```
Using built-in specs.
COLLECT_GCC=riscv64-unknown-elf-gcc
COLLECT_LTO_WRAPPER=/home/aitesam961/riscv/riscv-toolchain/bin/../libexec/gcc/riscv64-unknown-elf/13.2.0/lto-wrapper
Target: riscv64-unknown-elf
```

It is highly recommended to install the dependencies mentioned in `riscv-gnu-toolchain` readme. It will help avoid getting stuck at several stages if you move forward to building sibling tools.
```
$ sudo apt-get install autoconf automake autotools-dev curl python3 python3-pip libmpc-dev libmpfr-dev libgmp-dev gawk build-essential bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev ninja-build git cmake libglib2.0-dev
```

Congratulations! You have successfully installed the riscv-gnu-toolchain without wasting your system resources.


## Need Something Else?
If you are looking for a customized flavour of riscv-toolchain and want to use this framework, follow the instructions below:

1- Fork this repository and clone in your local machine or edit on GitHub.com
```
git clone https://github.com/aitesam961/soft-sky-riscv-gnu-prebuilt.git
cd soft-sky-riscv-gnu-prebuilt/.github/workflows
```
2- Create a new `rvxxxxx_tc_build.yaml` file.        
3- Make changes here according to your needs:
```
./configure --prefix="$HOME/riscv" --with-arch=rv32gc --with-abi=ilp32d
```
4- Commit (push if on local)      
5- Go to GitHub.com/your_forked_repo > Actions > All Workflows > click your created .yaml file and `Run Workflow`

It will take a while and if all goes well, you should see the artifacts uploaded, else find your errors in build logs.



### Contributions
Contributions from developers like you make open source a vibrant and innovative space. To contribute to `soft-sky-riscv-gcc-prebuilt`, please follow the guidelines outlined in the `CONTRIBUTING.md` file. Your efforts are highly valued, and together, we can continue to enhance and improve the project for the entire community.
