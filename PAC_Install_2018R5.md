# OpenVINO 2018R5 Install Guide for the Intel® Programmable Acceleration Card with Intel Arria® 10 GX FPGA on CentOS 



## Getting Started

The following describes the set-up of Intel® OpenVINO™ Tool Kit on Centos 7.4.  This is based upon a completely fresh install of Centos 7.4 with developer tools included.  This document was written for OpenVINO 2018 release 5 and may be largely applicable for later versions.  Official intel documentation for the install process can be found in the following locations and it is highly recommended that these are read, especially for new users.  This document serves as a guide and in some cases adds additional detail, specifically for an install with sudo privileges on Centos 7.4.

[Intel Acceleration Stack Quick Start Guide](https://www.intel.com/content/dam/altera-www/global/en_US/pdfs/literature/ug/ug-qs-ias-v1-1.pdf)

[OpenCL on Intel PAC Quick Start Guide](https://www.intel.com/content/dam/altera-www/global/en_US/pdfs/literature/ug/ug-qs-ias-opencl-a10-v1-1.pdf)

[Installing the OpenVINO Toolkit for Linux](https://software.intel.com/en-us/articles/OpenVINO-Install-Linux)

For details about installing CentOS 7.4, please see the associated section in Appendix A.

Optional: Install NTFS support for transferring large installers if already downloaded on another machine.
`sudo yum -y install epel-release`

`sudo yum -y install ntfs-3g`
## Install PAC & Acceleration Stack

Download version 1.1 of the Acceleration Stack for Runtime from here:

[https://www.altera.com/solutions/acceleration-hub/downloads.html](https://www.altera.com/solutions/acceleration-hub/downloads.html)

This downloads as a10_gx_pac_ias_1_1_pv_rte_installer.tar.gz. Let it download to ~/Downloads.

Untar and launch the installer:

mkdir -p ~/tools/intelrtestack

cd ~/Downloads

tar xf a10_gx_pac_ias_1_1_pv_rte_installer.tar.gz

cd a10_gx_pac_ias_1_1_pv_rte_installer

sudo ./setup.sh

Select Y to install OPAE and accept license and when asked, specify `/home/[username]/tools/intelrtestack  `as the install path.  During the installation there should be a message stating the directory already exists as it was created in the first command above.  Select Y to install to this directory.  If this message is not seen, it suggests that there was a typo when entering the install location.

Tools will be installed to the following directories:

Quartus Programmer: ~/tools/inteltrestack/intelFPGA_pro/qprogrammer

OpenCL Run Time Environment: ~/tools/intelrtestack/intelFPGA_pro/aclrte-linux64

Intel FPGA Acceleration Stack: ~/tools/intelrtestack/a10_gx_pac_ias_1_1_pv

  

Install E10/E40 Software Patch

`source ~/tools/intelrtestack/init_env.sh`

`cd $OPAE_PLATFORM_ROOT/hw`

`sudo wget [https://www.intel.com/content/dam/altera-www/global/en_US/others/solutions/acceleration-hub/a10_gx_pac_ias_1_1_pv_eth.patch](https://www.intel.com/content/dam/altera-www/global/en_US/others/solutions/acceleration-hub/a10_gx_pac_ias_1_1_pv_eth.patch)`

`sudo patch -s -p0 < a10_gx_pac_ias_1_1_pv_eth.patch`

Check the version of the FPGA Interface Manager firmware on the PAC board.

`sudo fpgainfo fme`

If the reported _Pr Interface Id_ is not `9926ab6d-6c92-5a68-aabc-a7d84c545738` then follow the instructions in section 4 of the [Intel Acceleration Stack Quick Start Guide](https://www.intel.com/content/dam/altera-www/global/en_US/pdfs/literature/ug/ug-qs-ias-v1-1.pdf) to update the FME.

Run the built in self-test to verify operation of the Acceleration Stack and PAC in a non-virtualized environment.

`sudo sh -c "echo 20 > /sys/kernel/mm/hugepages/hugepages-2048kB/nr_hugepages"`

`sudo fpgabist $OPAE_PLATFORM_ROOT/hw/samples/nlb_mode_3/bin/nlb_mode_3.gbs`

## Extract and Verify the Acceleration Stack OpenCL BSP

Extract the BSP

`cd $OPAE_PLATFORM_ROOT/opencl`

`sudo tar xf opencl_bsp.tar.gz`

Create an initialization script ~/init_openvino.sh with the following content that can be run upon opening a new terminal or rebooting.  This will source the script ran above as well as setting up the OpenCL environment.

`source \$HOME/tools/intelrtestack/init_env.sh`
`export CL_CONTEXT_COMPILER_MODE_ALTERA=3`
`export CL_CONTEXT_COMPILER_MODE_INTELFPGA=3`
`export INTELFPGAOCLSDKROOT="\$HOME/tools/intelrtestack/intelFPGA_pro/aclrte-linux64"`
`export ALTERAOCLSDKROOT="\$INTELFPGAOCLSDKROOT"`
`export AOCL_BOARD_PACKAGE_ROOT="\$OPAE_PLATFORM_ROOT/opencl/opencl_bsp"`
`\$AOCL_BOARD_PACKAGE_ROOT/linux64/libexec/setup_permissions.sh`
`source $INTELFPGAOCLSDKROOT/init_opencl.sh`

Source the script:

`source ~/init_openvino.sh`

Some of the settings made in the child scripts need a reboot to take effect.  Reboot the machine and source the script again.  Note that this script should be sourced each time a new terminal is opened for use with the Acceleration Stack and OpenVINO.

`source ~/init_openvino.sh`

Install the OpenCL driver:

`cd ~`

`sudo -E ./tools/intelrtestack/intelFPGA_pro/aclrte-linux64/bin/aocl install`

Select Y when asked to install the BSP.  Note that the following warning can be safely ignored.

`WARNING: install not implemented.  Please refer to DCP Quick Start User Guide.`

Program the PAC board with a pre-compiled aocx file (OpenCL based FPGA bitstream).

`cd \$OPAE_PLATFORM_ROOT/opencl`

`aocl program acl0 hello_world.aocx`

Run the Hello World application:

`sudo tar xf exm_opencl_hello_world_x64_linux.tgz`

`sudo chmod -R a+w hello_world`

`cd hello_world`

`make`

`cp ../hello_world.aocx ./bin`

`./bin/host`

## Adding OpenVINO with FPGA Support to Environment Variables


To run OpenVINO, add the last 4 commands to the ~/init_openvino.sh script.  The previous content is shown as well.

`source \$HOME/tools/intelrtestack/init_env.sh`
`export CL_CONTEXT_COMPILER_MODE_ALTERA=3`
`export CL_CONTEXT_COMPILER_MODE_INTELFPGA=3`
`export INTELFPGAOCLSDKROOT="\$HOME/tools/intelrtestack/intelFPGA_pro/aclrte-linux64"`
`export ALTERAOCLSDKROOT="\$INTELFPGAOCLSDKROOT"`
`export AOCL_BOARD_PACKAGE_ROOT="\$OPAE_PLATFORM_ROOT/opencl/opencl_bsp"`
`\$AOCL_BOARD_PACKAGE_ROOT/linux64/libexec/setup_permissions.sh`
`source $INTELFPGAOCLSDKROOT/init_opencl.sh`
`export IE_INSTALL="/opt/intel/computer_vision_sdk/deployment_tools"`
`source \$IE_INSTALL/../bin/setupvars.sh`
`export PATH="\$PATH:\$HOME/inference_engine_samples/intel64/Release"`
`alias mo="python3.6 \$IE_INSTALL/model_optimizer/mo.py"`

Source the script

`source ~/init_openvino.sh`

## Using OpenVINO

Run inference with OpenVINO independent of the demo scripts using the SqueezeNet model that was download by the scripts.  For convenience we will copy the necessary files to a local directory.  If the workstation has been rebooted or a new terminal is opened, source the script above first.

`mkdir ~/openvino_test`

`cd ~/openvino_test`

`cp ~/openvino_models/classification/squeezenet/1.1/caffe/squeezenet1.1.* .`

`cp ~/openvino_models/ir/squeezenet1.1/squeezenet1.1.labels .`

Note that the squeezenet1.1.labels file contains the classes used by ImageNet and is included here so that the inference results show text rather than classification numbers.  Convert the model with the Model Optimizer.  Note that the command below uses the alias defined in the script above and is not referred to in other documentation.

`mo --input_model squeezenet1.1.caffemodel`

Now run Inference on the CPU using one of the built in Inference Engine samples:

`classification_sample -m squeezenet1.1.xml -i $IE_INSTALL/demo/car.png`

Add the -d option to run on FPGA:

`classification_sample -m squeezenet1.1.xml -i $IE_INSTALL/demo/car.png -d HETERO:FPGA,CPU`

Increase the number of iterations with the -ni option to reduce the impact of initialization:

`classification_sample -m squeezenet1.1.xml -i $IE_INSTALL/demo/car.png -d HETERO:FPGA,CPU -ni 100`

Congratulations, You are done with the OpenVINO installation for FPGA. To learn more about how OpenVINO works, the Hello World tutorial and are other resources are provided below.

## Hello World Face Detection Tutorial

Use the  [OpenVINO with FPGA Hello World Face Detection Exercise](https://github.com/fritzboyle/openvino-with-fpga-hello-world-face-detection)to learn more about how the software and hardware work together.

## Additional Resources

Intel® Distribution of OpenVINO™ home page:  [https://software.intel.com/en-us/openvino-toolkit](https://software.intel.com/en-us/openvino-toolkit)

Intel® Distribution of OpenVINO™ toolkit documentation:  [https://software.intel.com/en-us/openvino-toolkit/documentation/featured](https://software.intel.com/en-us/openvino-toolkit/documentation/featured)

Inference Engine FPGA plugin documentation:  [https://](http://software.intel.com/en-us/articles/OpenVINO-InferEngine#fpga-plugin)[software.intel.com/en-us/articles/OpenVINO-InferEngine#fpga-plugin](https://software.intel.com/en-us/articles/OpenVINO-InferEngine#fpga-plugin)
