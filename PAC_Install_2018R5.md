<p class="note"><strong>NOTE</strong>: This installation guide is written for the Intel® Programmable Acceleration Card with Intel Arria® 10 GX FPGA on CentOS.</p>

<hr>

Install OpenVINO 2018 Release 5 for PAC

# Getting Started

The following describes the set-up of Intel® OpenVINO™ Tool Kit on Centos 7.4.  This is based upon a completely fresh install of Centos 7.4 with developer tools included.  This document was written for OpenVINO 2018 release 5 and may be largely applicable for later versions.  Official intel documentation for the install process can be found in the following locations and it is highly recommended that these are read, especially for new users.  This document serves as a guide and in some cases adds additional detail, specifically for an install with sudo privileges on Centos 7.4.

[Intel Acceleration Stack Quick Start Guide](https://www.intel.com/content/dam/altera-www/global/en_US/pdfs/literature/ug/ug-qs-ias-v1-1.pdf)

[OpenCL on Intel PAC Quick Start Guide](https://www.intel.com/content/dam/altera-www/global/en_US/pdfs/literature/ug/ug-qs-ias-opencl-a10-v1-1.pdf)

[Installing the OpenVINO Toolkit for Linux](https://software.intel.com/en-us/articles/OpenVINO-Install-Linux)

For details about installing CentOS 7.4, please see the associated section in Appendix A.

Optional: Install NTFS support for transferring large installers if already downloaded on another machine.
<code>sudo yum -y install epel-release</code>

<code>sudo yum -y install ntfs-3g</code>
# Install PAC & Acceleration Stack

Download version 1.1 of the Acceleration Stack for Runtime from here:

[https://www.altera.com/solutions/acceleration-hub/downloads.html](https://www.altera.com/solutions/acceleration-hub/downloads.html)

This downloads as a10_gx_pac_ias_1_1_pv_rte_installer.tar.gz. Let it download to ~/Downloads.

Untar and launch the installer:

mkdir -p ~/tools/intelrtestack

cd ~/Downloads

tar xf a10_gx_pac_ias_1_1_pv_rte_installer.tar.gz

cd a10_gx_pac_ias_1_1_pv_rte_installer

sudo ./setup.sh

Select Y to install OPAE and accept license and when asked, specify <code>/home/[username]/tools/intelrtestack  </code>as the install path.  During the installation there should be a message stating the directory already exists as it was created in the first command above.  Select Y to install to this directory.  If this message is not seen, it suggests that there was a typo when entering the install location.

Tools will be installed to the following directories:

Quartus Programmer: ~/tools/inteltrestack/intelFPGA_pro/qprogrammer

OpenCL Run Time Environment: ~/tools/intelrtestack/intelFPGA_pro/aclrte-linux64

Intel FPGA Acceleration Stack: ~/tools/intelrtestack/a10_gx_pac_ias_1_1_pv

  

Install E10/E40 Software Patch

<code>source ~/tools/intelrtestack/init_env.sh</code>

<code>cd $OPAE_PLATFORM_ROOT/hw</code>

<code>sudo wget [https://www.intel.com/content/dam/altera-www/global/en_US/others/solutions/acceleration-hub/a10_gx_pac_ias_1_1_pv_eth.patch](https://www.intel.com/content/dam/altera-www/global/en_US/others/solutions/acceleration-hub/a10_gx_pac_ias_1_1_pv_eth.patch)</code>

<code>sudo patch -s -p0 < a10_gx_pac_ias_1_1_pv_eth.patch</code>

Check the version of the FPGA Interface Manager firmware on the PAC board.

<code>sudo fpgainfo fme</code>

If the reported _Pr Interface Id_ is not <code>9926ab6d-6c92-5a68-aabc-a7d84c545738</code> then follow the instructions in section 4 of the [Intel Acceleration Stack Quick Start Guide](https://www.intel.com/content/dam/altera-www/global/en_US/pdfs/literature/ug/ug-qs-ias-v1-1.pdf) to update the FME.

Run the built in self-test to verify operation of the Acceleration Stack and PAC in a non-virtualized environment.

<code>sudo sh -c "echo 20 > /sys/kernel/mm/hugepages/hugepages-2048kB/nr_hugepages"</code>

<code>sudo fpgabist $OPAE_PLATFORM_ROOT/hw/samples/nlb_mode_3/bin/nlb_mode_3.gbs</code>

## Extract and Verify the Acceleration Stack OpenCL BSP

Extract the BSP

<code>cd $OPAE_PLATFORM_ROOT/opencl</code>

<code>sudo tar xf opencl_bsp.tar.gz</code>

Create an initialization script ~/init_openvino.sh with the following content that can be run upon opening a new terminal or rebooting.  This will source the script ran above as well as setting up the OpenCL environment.

<code>source \$HOME/tools/intelrtestack/init_env.sh</code>
<code>export CL_CONTEXT_COMPILER_MODE_ALTERA=3</code>
<code>export CL_CONTEXT_COMPILER_MODE_INTELFPGA=3</code>
<code>export INTELFPGAOCLSDKROOT="\$HOME/tools/intelrtestack/intelFPGA_pro/aclrte-linux64"</code>
<code>export ALTERAOCLSDKROOT="\$INTELFPGAOCLSDKROOT"</code>
<code>export AOCL_BOARD_PACKAGE_ROOT="\$OPAE_PLATFORM_ROOT/opencl/opencl_bsp"</code>
<code>\$AOCL_BOARD_PACKAGE_ROOT/linux64/libexec/setup_permissions.sh</code>
<code>source $INTELFPGAOCLSDKROOT/init_opencl.sh</code>

Source the script:

<code>source ~/init_openvino.sh</code>

Some of the settings made in the child scripts need a reboot to take effect.  Reboot the machine and source the script again.  Note that this script should be sourced each time a new terminal is opened for use with the Acceleration Stack and OpenVINO.

<code>source ~/init_openvino.sh</code>

Install the OpenCL driver:

<code>cd ~</code>

<code>sudo -E ./tools/intelrtestack/intelFPGA_pro/aclrte-linux64/bin/aocl install</code>

Select Y when asked to install the BSP.  Note that the following warning can be safely ignored.

<code>WARNING: install not implemented.  Please refer to DCP Quick Start User Guide.</code>

Program the PAC board with a pre-compiled aocx file (OpenCL based FPGA bitstream).

<code>cd \$OPAE_PLATFORM_ROOT/opencl</code>

<code>aocl program acl0 hello_world.aocx</code>

Run the Hello World application:

<code>sudo tar xf exm_opencl_hello_world_x64_linux.tgz</code>

<code>sudo chmod -R a+w hello_world</code>

<code>cd hello_world</code>

<code>make</code>

<code>cp ../hello_world.aocx ./bin</code>

<code>./bin/host</code>

# Adding OpenVINO with FPGA Support to Environment Variables


To run OpenVINO, add the last 4 commands to the ~/init_openvino.sh script.  The previous content is shown as well.

<code>source \$HOME/tools/intelrtestack/init_env.sh</code>
<code>export CL_CONTEXT_COMPILER_MODE_ALTERA=3</code>
<code>export CL_CONTEXT_COMPILER_MODE_INTELFPGA=3</code>
<code>export INTELFPGAOCLSDKROOT="\$HOME/tools/intelrtestack/intelFPGA_pro/aclrte-linux64"</code>
<code>export ALTERAOCLSDKROOT="\$INTELFPGAOCLSDKROOT"</code>
<code>export AOCL_BOARD_PACKAGE_ROOT="\$OPAE_PLATFORM_ROOT/opencl/opencl_bsp"</code>
<code>\$AOCL_BOARD_PACKAGE_ROOT/linux64/libexec/setup_permissions.sh</code>
<code>source $INTELFPGAOCLSDKROOT/init_opencl.sh</code>
<code>export IE_INSTALL="/opt/intel/computer_vision_sdk/deployment_tools"</code>
<code>source \$IE_INSTALL/../bin/setupvars.sh</code>
<code>export PATH="\$PATH:\$HOME/inference_engine_samples/intel64/Release"</code>
<code>alias mo="python3.6 \$IE_INSTALL/model_optimizer/mo.py"</code>

Source the script

<code>source ~/init_openvino.sh</code>

## Using OpenVINO

Run inference with OpenVINO independent of the scripts above using the SqueezeNet model that was download by the scripts.  For convenience we will copy the necessary files to a local directory.  If the workstation has been rebooted or a new terminal is opened, source the script above first.

mkdir ~/openvino_test

cd ~/openvino_test

cp ~/openvino_models/classification/squeezenet/1.1/caffe/squeezenet1.1.* .

cp ~/openvino_models/ir/squeezenet1.1/squeezenet1.1.labels .

Note that the squeezenet1.1.labels file contains the classes used by ImageNet and is included here so that the inference results show text rather than classification numbers.  Convert the model with the Model Optimizer.  Note that the command below uses the alias defined in the script above and is not referred to in other documentation.

mo --input_model squeezenet1.1.caffemodel

Now run Inference on the CPU using one of the built in Inference Engine samples:

classification_sample -m squeezenet1.1.xml -i $IE_INSTALL/demo/car.png

Add the -d option to run on FPGA:

classification_sample -m squeezenet1.1.xml -i $IE_INSTALL/demo/car.png -d HETERO:FPGA,CPU

Increase the number of iterations with the -ni option to reduce the impact of initialization:

classification_sample -m squeezenet1.1.xml -i $IE_INSTALL/demo/car.png -d HETERO:FPGA,CPU -ni 100

# Appendix A – Installing CentOS 7.4

The following is a quick writeup how to get CentOS 7.4 and run a fresh install. It is only a hint how it can be done, no perfect scenario.

Installation starts from CentOS-7-x86_64-Everything-1708.iso image. Image is downloadable from repositories at:

[http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Everything-1708.iso](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Everything-1708.iso)

or: [http://ftp.hosteurope.de/mirror/centos.org/7/isos/x86_64/CentOS-7-x86_64-Everything-1708.iso](http://ftp.hosteurope.de/mirror/centos.org/7/isos/x86_64/CentOS-7-x86_64-Everything-1708.iso%20)

Selections:

Language: as required

Timezone: as required

Time/Date

Add additional keyboard layouts as needed (the top one in the list is the default then)

Installation source: local media

Software Selection:

Base Environment: Development and Creative Workstation

Add-Ons for Selected Environment:

Additional Development

Compatibility Libraries

Development Tools

Platform Development

Python

Virtualization Hyperviser

Installation Destination:

I will configure partitioning

Select partition scheme: Standard Partition

+ Mount Point: swap

Desired Capacity:  ___ GiB (i.e. 4GiB)

Device Type: Standard Partition

File System: swap

Label: swap

+  Mount Point: /

Desired Capacity:  ___ GiB (i.e. 40 GiB)

Device Type: Standard Partition

File System: ext4

Label: /

à Done à Accept Changes

Network & Host name:

Host Name: xxxxxxxx

Ethernet (xxxxxxxxx): à ON

à Done

à Begin Installation

Set root Password

(Note: simple passwords give a warning but clicking “Done” twice accepts them anyway)

Create a user:

**_<username>_**

Password

Select the option to for this user to be an administrator

(Note: simple passwords give a warning but clicking “Done” twice accepts them anyway)

Wait for installation to finish.

à Reboot

After rebooting:

Accept the license agreement, Finish installation
