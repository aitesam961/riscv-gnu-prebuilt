# soft-sky: Pre-Built toolchain for RISCV
Up-to-date Prebuilt RISCV GNU toolchain for painless setup. No more build from source everytime. The purpose of this effort is to save time and computer resources for users and a contribution to eco-friendly computing practices.

### Supported Environment
Prebuilt riscv-gnu-toolchain binaries for x86_64 host running Linux.
Built on Ubuntu Latest  and tested on `Ubuntu 24.04 LTS` and `Ubuntu 22.04 LTS`

This repository makes use of GitHub actions to build the `riscv-gnu-toolchain` from source. Just download the prebuilt binaries, add to path and use.

### Available Configurations
- [x]   RV64 Newlib
- [x]   RV64GC
- [x]   RV64 Multilib
- [x]   RV32 Newlib
- [x]   RV32 Newlib
- [x]   OpenHWGroup/CVA6
- [x]   Pulp-Platform/Cheshire SoC
- [x]   Pulp-Platform/Culsans
- [x]   Syntacore/SCR1
- [x]   lowRISC/IBEX





If you require a custom configuration, consider building yourself or open an Issue with tag:`Enhancement`

### Installation
1- Download prebuilt binaries from GitHub [Actions](https://github.com/aitesam961/soft-sky-riscv-gnu-prebuilt/actions) > Artifacts

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
./configure --prefix=/opt/riscv --with-arch=rv32gc --with-abi=ilp32d
```
4- Commit (push if on local)      
5- Go to GitHub.com/your_forked_repo > Actions > All Workflows > click your created .yaml file and `Run Workflow`

It will take a while and if all goes well, you should see the artifacts uploaded, else find your errors in build logs.



### Contributions
Contributions from developers like you make open source a vibrant and innovative space. To contribute to `soft-sky-riscv-gcc-prebuilt`, please follow the guidelines outlined in the `CONTRIBUTING.md` file. Your efforts are highly valued, and together, we can continue to enhance and improve the project for the entire community.
