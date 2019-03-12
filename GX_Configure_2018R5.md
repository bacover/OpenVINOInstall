# Configuration Guide for OpenVINO 2018R5 and the Intel® Arria® 10 GX FPGA Development Kit on Linux (Ubuntu/CentOS) 


Configuration Notes:

  * For a first-time installation, use all steps.
  * Use steps 1 and 2 only after receiving a new FPGA card.
  * Repeat steps 3-6 when installing a new version of the Intel Distribution of OpenVINO toolkit.
  * Use steps 4-6 when a Neural Network topology used by an Intel Distribution of OpenVINO application changes.


## 1. Configure the Intel® Arria® 10 GX FPGA Development Kit

To configure the Intel® Arria® 10 GX FPGA Development Kit, use the instruction in the [Configuring the Intel® Arria 10 GX FPGA Development Kit for the Intel FPGA SDK for OpenCL](https://www.intel.com/content/www/us/en/programmable/documentation/tgy1490191698959.html?_ga=2.246291212.90797072.1535473709-1926199859.1534783437) guide. Stop after you configure and install the Intel Arria 10 GX FPGA Development Kit board. Do not follow the steps in the rest of the document. Instead, return to this document upon completing the specified section.

![](https://software.intel.com/sites/default/files/managed/43/e6/initialize%20STOP%20copy.png)

## 2. Program the Intel® Arria® 10 GX FPGA Development Kit

**NOTE**: You need to do this only once, after you set up the FPGA board.


  * Use one of the following two links to download the Intel® Quartus® software, depending on the version you want:
		
  * If you have an Intel® Quartus® license, use  [QuartusProProgrammerSetup-17.1.0.240-linux](http://fpgasoftware.intel.com/17.1/?edition=pro&amp;platform=linux&amp;download_manager=dlm3&amp;product=qprogrammer#tabs-4). This software uses about 500 MB of disk space.

![](https://software.intel.com/sites/default/files/managed/7c/b9/Quartus%20Pro%20Download.png)
			
  * If you do not have the license, use [QuartusPrimeLiteProgrammerSetup-17.1.0.590-linux](http://fpgasoftware.intel.com/17.1/?edition=lite&amp;platform=linux&amp;download_manager=dlm3&amp;product=qprogrammer#tabs-4). This software uses about 500 MB of disk space.
![](https://software.intel.com/sites/default/files/managed/b5/6a/Quartus%20Lite%20Download.png)
			
		
	
  * Go to the `Downloads` directory or the directory to which you downloaded the Intel® Quartus® package. This document assumes the software is in the `Downloads` directory:
		`cd ~/Downloads`
	
  * Use the command for the package you downloaded:
		
	  * **Option 1: Intel® Quartus® Pro:**
				`sudo chmod +x QuartusProProgrammerSetup-17.1.0.240-linux.run`
			
	  * **Option 2: Intel® Quartus® Lite:**
				`chmod +x QuartusProgrammerSetup-17.1.0.590-linux.run`		
	
  * Run the Intel® Quartus® Installer:
		`sudo ./Quartus.<version>.run`
	
  * Click through the installer to the end. Remove the checkmarks from all boxes at the end of the installation.
  * By default, the software is installed under `/home/user`. We suggest changing this directory to `/opt/altera` during the installation. A subdirectory is created with the name dependent on your version of Intel® Quartus®:
		
	  * Intel® Quartus® Pro: `/opt/altera/intelFPGA_pro/17.1`
	  * Intel® Quartus® Lite: `/opt/altera/intelFPGA/17.1`
		
	
  * Download `fpga_support_files.tgz` from the [Intel Registration Center](http://registrationcenter-download.intel.com/akdlm/irc_nas/12954/fpga_support_files.tgz). The files in this `.tgz` archive are required to ensure your FPGA card and the Intel Distribution of OpenVINO toolkit work correctly.
  * Go to the directory where you downloaded the `fpga_support_files.tgz` archive.
  * Unpack the `.tgz` file:
		`tar -xvzf fpga_support_files.tgz`
		A directory named `fpga_support_files` is created.
  * Go to the `fpga_support_files` directory:
		`cd fpga_support_files`
	
  * Copy `setup_env.sh` to your home directory:
		`cp setup_env.sh /home/<user>`

		`source /home/<user>/setup_env.sh`
	
  * Configure the FPGA Driver Blacklist:
		`sudo mv config/blacklist-altera-cvp.conf /etc/modprobe.d`
	
	  * Copy the USB rules:
		`sudo cp config/51-usbblaster.rules /etc/udev/rules.d/`
	
	  * Load the USB rules:
		`sudo udevadm control --reload-rules &amp;&amp; udevadm trigger`
	
	  * Unplug and replug the Micro-USB cable from the Intel® Arria 10 GX board for JTAG.
	  * [OPTIONAL] Validate that the cable is connected:
		`lsusb | grep Altera`

You should see a message similar to:
		`Bus 001 Device 005: ID 09fb:6010 Altera`

  * Run `jtagconfig`:
		`jtagconfig`

Your output is similar to:

`USB-BlasterII [1-14]
02E660DD 10AX115H1(.|E2|ES)/10AX115H2/..
020A40DD 5M(1270ZF324|2210Z)/EPM2210`

  * Use `jtagconfig` to slow the clock:
		`jtagconfig --setparam 1 JtagClock 6M`
	
  * (OPTIONAL) Confirm the clock is set to 6M:
		`jtagconfig --getparam 1 JtagClock`

You should see the following:
		`6M`

  * Go to the `config` directory:
		`cd config`
	
  * Use Intel® Quartus® software to program `top.sof` and `max5_150.pof`. These files are from the `fpga_support_files.tgz` archive:
`quartus_pgm -c 1 -m JTAG -o "p;max5_150.pof@2"`

`quartus_pgm -c 1 -m JTAG -o "p;top.sof"`

  * Restart your computer:
		`reboot`
	
  * Verify that you successfully programmed `top.sof`:
		`sudo lspci |grep Alt`

If successful, you see a response similar to:
		`01:00.0 Processing accelerators: Altera Corporation Device 2494 (rev 01)`


**NOTE**: You will finish setting up the card after you install Intel Distribution of OpenVINO toolkit.

## 3. Complete Intel® Arria® 10 FPGA Setup



  * Download `fpga_support_files.tgz` from [the Intel Registration Center.](http://registrationcenter-download.intel.com/akdlm/irc_nas/12954/fpga_support_files.tgz) The files in this `.tgz` are required to ensure your FPGA card and Intel Distribution of OpenVINO toolkit work correctly. Right click or save the file instead of letting your browser extract automatically.
  * Go to the directory where you downloaded `fpga_support_files.tgz`.
  * Unpack the `.tgz` file:
		`tar -xvzf fpga_support_files.tgz`
		A directory named `fpga_support_files` is created.
  * Go to the `fpga_support_files` directory:
		`cd fpga_support_files`
	


**NOTE**: These steps are for both Intel Arria 10 FPGA cards.


  * Switch to superuser:
		`sudo su`
	
  * Use the `setup_env.sh` script from `fpga_support_files.tgz` to set your environment variables:
		`source /home/<user>/Downloads/fpga_support_files/setup_env.sh`
	
  * Change directory to `Downloads/fpga_support_files/`:
		`cd /home/<user>/Downloads/fpga_support_files/`
	
  * Run the FPGA dependencies script, which allows OpenCL to support Ubuntu and recent kernels:
		`./install_openvino_fpga_dependencies.sh`


  * When asked, select the FPGA card, Intel® GPU, and Movidius Neural Compute Stick, then you can install the correct dependencies.
  * If you installed the 4.14 kernel, you will need to reboot the machine and select the new kernel in the Ubuntu (grub) boot menu. You will also need to redo steps 1 and 2 to set up your environmental variables again.
		
	
  * Install OpenCL devices. Enter `Y` when prompted to install:
		`aocl install`
	
  * Reboot the machine:
		`reboot`
	
  * Use the `setup_env.sh` script from `fpga_support_files.tgz` to set your environment variables:
		`source /home/<user>/Downloads/fpga_support_files/setup_env.sh`
	
  * Run `aocl diagnose`:
		`aocl diagnose`
		Your screen displays `DIAGNOSTIC_PASSED`.


## 4. Program a Bitstream

The bitstream you program should correspond to the topology you want to deploy. In this section, you program a SqueezeNet bitstream and deploy the classification sample with a SqueezeNet model that you used the Model Optimizer to convert in the example above.

**IMPORTANT**: Only use bitstreams from the installed version of the Intel Distribution of OpenVINO toolkit. Bitstreams from older versions of the OpenVINO™ toolkit are incompatible with later versions of the Intel Distribution of OpenVINO toolkit. For example, you cannot use the `1-0-1_A10DK_FP16_Generic` bitstream, when the Intel Distribution of OpenVINO toolkit supports the `2-0-1_A10DK_FP16_Generic bitstream`.

Depending on how many bitstreams you selected, there are different folders for each FPGA card type which were downloaded in the Intel Distribution of OpenVINO package:


  * For the Intel® Arria 10GX Developer Kit FPGA, the pre-trained bitstreams are in the `/opt/intel/computer_vision_sdk/bitstreams/a10_devkit_bitstreams` directory. This example uses a SqueezeNet bitstream with low precision for the classification sample.



  * Rerun the environment setup script:
		`source /home/<user>/Downloads/fpga_support_files/setup_env.sh`
	
  * Change to your home directory:
		`cd /home/<user>`
	
  * Choose the option based on which card you have:
		
  * Program the bitstream for Intel® Arria® 10 FPGA Development Kit:
				`aocl program acl0 /opt/intel/computer_vision_sdk/a10_devkit_bitstreams/5-0-1_A10DK_FP11_SqueezeNet.aocx`
			
		
	


### Optional Steps to Flash the FPGA Card

**NOTE**:
	- To avoid having to reprogram the board after a power down, a bitstream will be programmed to permanent memory on the Intel® Arria® 10 GX FPGA Development Kit. This will take about 20 minutes.
	- The following steps 1-5 need to be done only once for a new Intel® Arria 10 FPGA card.


  * Plug in the Micro USB cable to the card and your host system.
  * Run `jtagconfig` to ensure that the cable is properly inserted:
		`jtagconfig`
	
  * Use `jtagconfig` to slow the clock:
		`jtagconfig --setparam 1 JtagClock 6M`
	
  * Store the Intel Arria 10 FPGA Development Kit bitstream long term on the board:
		`aocl flash acl0 /opt/intel/computer_vision_sdk/a10_devkit_bitstreams/5-0-1_A10DK_FP11_SqueezeNet.aocx`
	


Your output is similar to:

`USB-BlasterII [1-14]
02E660DD 10AX115H1(.|E2|ES)/10AX115H2/..
020A40DD 5M(1270ZF324|2210Z)/EPM2210
		`

## 5. Setup a Neural Network Model for FPGA

In this section, you create an FP16 model suitable for hardware accelerators. For more information, see the [FPGA plugin](https://software.intel.com/en-us/articles/OpenVINO-InferEngine#fpga-plugin) section in the Inference Engine Developer Guide.


  * Create a directory for the FP16 SqueezeNet Model:
		`mkdir /home/<user>/squeezenet1.1_FP16`
	
  * Go to `/home/<user>/squeezenet1.1_FP16`:
		`cd /home/<user>/squeezenet1.1_FP16`
	
  * Use the Model Optimizer to convert an FP16 SqueezeNet Caffe model into an optimized Intermediate Representation (IR):
		`python3 /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/mo.py --input_model /home/<user>/openvino_models/FP32/classification/squeezenet/1.1/caffe/squeezenet1.1.caffemodel --data_type FP16 --output_dir .`
	
  * The `squeezenet1.1.labels` file contains the classes `ImageNet` uses. This file is included so that the inference results show text instead of classification numbers. Copy `squeezenet1.1.labels` to the your optimized model location:
		`cp /home/<user>/openvino_models/ir/squeezenet1.1/FP32/squeezenet1.1.labels  .`
	
  * Copy a sample image to the release directory. You will use this with your optimized model:
		`sudo cp /opt/intel/computer_vision_sdk/deployment_tools/demo/car.png  ~/inference_engine_samples/intel64/Release`
	


## 6. Run a Sample Application

1. Go to the samples directory

`cd`  `/home/<user>/inference_engine_samples/intel64/Release`

2. Use an Inference Engine sample to run a sample application on the CPU:

`./classification_sample -i car.png -m ~/openvino_models/ir/squeezenet1.1/FP32/squeezenet1.1.xml`

Note the CPU throughput in Frames Per Second (FPS). This tells you how quickly the inference is done on the hardware. Now run the inference using the FPGA.

3. Add the -d option to target the FPGA:

`./classification_sample -i car.png -m ~/squeezenet1.1_FP16/squeezenet1.1.xml -d HETERO:FPGA,CPU`

The throughput on FPGA is listed and may show a lower FPS. This is due to the initialization time. To account for that, the next step increases the iterations to get a better sense of the speed the FPGA can run inference at.

4. Use -ni to increase the number of iterations, This option reduces the initialization impact:

`./classification_sample -i car.png -m ~/squeezenet1.1_FP16/squeezenet1.1.xml -d HETERO:FPGA,CPU -ni 100`

Congratulations, You are done with the OpenVINO installation for FPGA. To learn more about how OpenVINO works, the Hello World tutorial and are other resources are provided below.

## Hello World Face Detection Tutorial

Use the  [OpenVINO with FPGA Hello World Face Detection Exercise](https://github.com/fritzboyle/openvino-with-fpga-hello-world-face-detection)to learn more about how the software and hardware work together.

## Additional Resources

Intel® Distribution of OpenVINO™ home page:  [https://software.intel.com/en-us/openvino-toolkit](https://software.intel.com/en-us/openvino-toolkit)

Intel® Distribution of OpenVINO™ toolkit documentation:  [https://software.intel.com/en-us/openvino-toolkit/documentation/featured](https://software.intel.com/en-us/openvino-toolkit/documentation/featured)

Inference Engine FPGA plugin documentation:  [https://](http://software.intel.com/en-us/articles/OpenVINO-InferEngine#fpga-plugin)[software.intel.com/en-us/articles/OpenVINO-InferEngine#fpga-plugin](https://software.intel.com/en-us/articles/OpenVINO-InferEngine#fpga-plugin)
