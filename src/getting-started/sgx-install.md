# SGX Installation

Instruction on how to install SGX can be found here: [https://download.01.org/intel-sgx/sgx-linux/2.17/docs/Intel_SGX_SW_Installation_Guide_for_Linux.pdf](https://download.01.org/intel-sgx/sgx-linux/2.17/docs/Intel_SGX_SW_Installation_Guide_for_Linux.pdf). (You can check [https://download.01.org/intel-sgx/sgx-linux/](https://download.01.org/intel-sgx/sgx-linux/) to see if there is a newer version.)


Overall you need to have 3 components installed:
1. The SGX Driver. This is at the Kernel level or just above, and handles sending the exact instructions to the hardware.
2. The SGX Platform Software (PSW). This provides the required layer to run SGX application on Linux.
3. The SGX SDK, which has the necessary API to develop SGX applications.

The first two components should be installed by default as long as you have a kernel version above 5.
You can run `uname -a` to check the linux version.
This will output something similar to this:
```bash
Linux <machine-name> 5.14.0-1054-oem #61-Ubuntu SMP Fri Oct 14 13:05:50 UTC 2022 x86_64 x86_64 x86_64 GNU/Linu
```
Here the version is 5.14, which means the Driver and PSW are already in the kernel.

For the SGX SDK, you can download it [here](https://download.01.org/intel-sgx/sgx-linux/).
Go to the folder of the version of SGX supported by the rust sgx sdk.
At the moment, the rust sdk supporst 2.17, you can usually check the version by checking the last commit logs [here](https://github.com/apache/incubator-teaclave-sgx-sdk/commits/v2.0.0-preview).
Then you need to select the distribution version.
You can check your ubuntu version by running `lsb_release -a`.
This will output something similar to:
```bash
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.5 LTS
Release:        20.04
Codename:       focal
```
This means that we need the version `ubuntu20.04-server` in the `distro` folder.
In [this folder](https://download.01.org/intel-sgx/sgx-linux/2.17/distro/ubuntu20.04-server) you will find a `.bin` that corresponds to the `sdk`.
Here it is [sgx_linux_x64_sdk_2.17.100.3.bin](https://download.01.org/intel-sgx/sgx-linux/2.17/distro/ubuntu20.04-server/sgx_linux_x64_sdk_2.17.100.3.bin).
You need to download this binary file.
Then you should run it.
You can make it executable by running `chmod +x sgx_linux_x64_sdk_2.17.100.3.bin).
And then run it with `./sgx_linux_x64_sdk_2.17.100.3.bin`.
It will ask you where you want to install it.
We suggests `/opt/sgx-sdk`.

Once the sgx SDK has finished to install and you can see the files in `/opt/sgx-sdk` folder you are good to go.

Note that teaclave also has some documentation on the installation, with suggested hardware: [https://teaclave.apache.org/sgx-sdk-docs/environment-setup/](https://teaclave.apache.org/sgx-sdk-docs/environment-setup/).