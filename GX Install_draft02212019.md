<p class="note"><strong>NOTE</strong>: This installation guide is written for the Intel® Arria® 10 GX FPGA Development Kit on Linux.</p>

<p>Installation Notes:</p>

<ul>
	<li>For a first-time installation, use all steps.</li>
	<li>Use steps 1 and 2 only after receiving a new FPGA card.</li>
	<li>Repeat steps 3-6 when installing a new version of the Intel Distribution of OpenVINO toolkit.</li>
	<li>Use step 7 when a Neural Network topology used by an Intel Distribution of OpenVINO application changes.</li>
</ul>

<hr>
<h2>1. Configure the Intel® Arria® 10 GX FPGA Development Kit<a id="configure aria 10 gx fpga" name="configure aria 10 gx fpga"></a></h2>

<p>To configure the Intel® Arria® 10 GX FPGA Development Kit, use the instruction in the <a href="https://www.intel.com/content/www/us/en/programmable/documentation/tgy1490191698959.html?_ga=2.246291212.90797072.1535473709-1926199859.1534783437">Configuring the Intel® Arria 10 GX FPGA Development Kit for the Intel FPGA SDK for OpenCL</a> guide. Stop after you configure and install the Intel Arria 10 GX FPGA Development Kit board. Do not follow the steps in the rest of the document. Instead, return to this document upon completing the specified section.</p>

<p><img alt="stop after initializing the Arria 10 card" src="https://software.intel.com/sites/default/files/managed/43/e6/initialize%20STOP%20copy.png" title="shows place to return to the install guide"></p>

<h2>2. Program the Intel® Arria® 10 GX FPGA Development Kit<a id="program aria 10 gx" name="program aria 10 gx"></a></h2>

<p class="note"><strong>NOTE</strong>: You need to do this only once, after you set up the FPGA board.</p>

<ol>
	<li>Use one of the following two links to download the Intel® Quartus® software, depending on the version you want:
		<ul>
			<li>If you have an Intel® Quartus® license, use  <a href="http://fpgasoftware.intel.com/17.1/?edition=pro&amp;platform=linux&amp;download_manager=dlm3&amp;product=qprogrammer#tabs-4">QuartusProProgrammerSetup-17.1.0.240-linux</a>. This software uses about 35 GB of disk space.

<p><img alt="Quartus® Pro download screen" src="https://software.intel.com/sites/default/files/managed/7c/b9/Quartus%20Pro%20Download.png" title=""></p>
			</li>
			<li>If you do not have the license, use <a href="http://fpgasoftware.intel.com/17.1/?edition=lite&amp;platform=linux&amp;download_manager=dlm3&amp;product=qprogrammer#tabs-4">QuartusPrimeLiteProgrammerSetup-17.1.0.590-linux</a>. This software uses about 1.4 GB of disk space.
				<p><img alt="download screen for Quartus Lite" src="https://software.intel.com/sites/default/files/managed/b5/6a/Quartus%20Lite%20Download.png" title=""></p>
			</li>
		</ul>
	</li>
	<li>Go to the <code>Downloads</code> directory or the directory to which you downloaded the Intel® Quartus® package. This document assumes the software is in the <code>Downloads</code> directory:
		<pre class="brush:bash; class-name:dark;">cd ~/Downloads</pre>
	</li>
	<li>Use the command for the package you downloaded:
		<ul>
			<li><strong>Option 1: Intel® Quartus® Pro:</strong>
				<pre class="brush:bash; class-name:dark;">sudo chmod +x QuartusProProgrammerSetup-17.1.0.240-linux.run</pre>
			</li>
			<li><strong>Option 2: Intel® Quartus® Lite:</strong>
				<pre class="brush:bash; class-name:dark;">chmod +x QuartusProgrammerSetup-17.1.0.590-linux.run</pre>
			</li>
		</ul>
	</li>
	<li>Run the Intel® Quartus® Installer:
		<pre class="brush:bash; class-name:dark;">sudo ./Quartus.&lt;version&gt;.run</pre>
	</li>
	<li>Click through the installer to the end. Remove the checkmarks from all boxes at the end of the installation.<br>
		By default, the software is installed under <code>/home/user</code>. We suggest changing this directory to <code>/opt/altera</code> during the installation. A subdirectory is created with the name dependent on your version of Intel® Quartus®:
		<ul>
			<li>Intel® Quartus® Pro: <code>/opt/altera/intelFPGA_pro/17.1</code></li>
			<li>Intel® Quartus® Lite: <code>/opt/altera/intelFPGA/17.1</code></li>
		</ul>
	</li>
	<li>Download <code>fpga_support_files.tgz</code> from the <a href="http://registrationcenter-download.intel.com/akdlm/irc_nas/12954/fpga_support_files.tgz">Intel Registration Center</a>. The files in this <code>.tgz</code> archive are required to ensure your FPGA card and the Intel Distribution of OpenVINO toolkit work correctly.</li>
	<li>Go to the directory where you downloaded the <code>fpga_support_files.tgz</code> archive.</li>
	<li>Unpack the <code>.tgz</code> file:
		<pre class="brush:bash; class-name:dark;">tar -xvzf fpga_support_files.tgz</pre>
		A directory named <code>fpga_support_files</code> is created.</li>
	<li>Go to the <code>fpga_support_files</code> directory:
		<pre class="brush:bash; class-name:dark;">cd fpga_support_files</pre>
	</li>
	<li>Copy <code>setup_env.sh</code> to your home directory:
		<pre class="brush:bash; class-name:dark;">cp setup_env.sh /home/&lt;user&gt;</pre>

<pre class="brush:bash; class-name:dark;">source /home/&lt;user&gt;/setup_env.sh</pre>
	
<li>Configure the FPGA Driver Blacklist:
		<pre class="brush:bash; class-name:dark;">sudo mv config/blacklist-altera-cvp.conf /etc/modprobe.d</pre>
	</li>
	<li>Copy the USB rules:
		<pre class="brush:bash; class-name:dark;">sudo cp config/51-usbblaster.rules /etc/udev/rules.d/</pre>
	</li>
	<li>Load the USB rules:
		<pre class="brush:bash; class-name:dark;">sudo udevadm control --reload-rules &amp;&amp; udevadm trigger</pre>
	</li>
	<li>Unplug and replug the Micro-USB cable from the Intel® Arria 10 GX board for JTAG.</li>
	<li>[OPTIONAL] Validate that the cable is connected:
		<pre class="brush:bash; class-name:dark;">lsusb | grep Altera</pre>

<p>You should see a message similar to:</p>
		<code>Bus 001 Device 005: ID 09fb:6010 Altera</code></li>
	<br>
	<li>Run <code>jtagconfig</code>:
		<pre class="brush:bash; class-name:dark;">jtagconfig</pre>

<p>Your output is similar to:</p>

<pre class="brush:bash; class-name:dark;">USB-BlasterII [1-14]
02E660DD 10AX115H1(.|E2|ES)/10AX115H2/..
020A40DD 5M(1270ZF324|2210Z)/EPM2210</pre>
</li>
<br>
	<li>Use <code>jtagconfig</code> to slow the clock:
		<pre class="brush:bash; class-name:dark;">jtagconfig --setparam 1 JtagClock 6M</pre>
	</li>
	<li>(OPTIONAL) Confirm the clock is set to 6M:
		<pre class="brush:bash; class-name:dark;">jtagconfig --getparam 1 JtagClock</pre>

<p>You should see the following:</p>
		<code>6M</code></li>
	<br>
	<li>Go to the <code>config</code> directory:
		<pre class="brush:bash; class-name:dark;">cd config</pre>
	</li>
	<li>Use Intel® Quartus® software to program <code>top.sof</code> and <code>max5_150.pof</code>. These files are from the <code>fpga_support_files.tgz</code> archive:
<pre class="brush:bash; class-name:dark;">quartus_pgm -c 1 -m JTAG -o "p;max5_150.pof@2"</pre>

<pre class="brush:bash; class-name:dark;">quartus_pgm -c 1 -m JTAG -o "p;top.sof"</pre>
</li>
	<li>Restart your computer:
		<pre class="brush:bash; class-name:dark;">reboot</pre>
	</li>
	<li>Verify that you successfully programmed <code>top.sof</code>:
		<pre class="brush:bash; class-name:dark;">sudo lspci |grep Alt</pre>

<p>If successful, you see a response similar to:</p>
		<code>01:00.0 Processing accelerators: Altera Corporation Device 2494 (rev 01)</code></li>
</ol>

<p class="note"><strong>NOTE</strong>: You will finish setting up the card after you install Intel Distribution of OpenVINO toolkit.</p>
<h2><a id="complete aria 10 gx" name="complete aria 10 gx"></a>3. Complete Intel® Arria® 10 FPGA Setup</h2>

<p>For the Intel Vision Accelerator Design with Intel Arria 10 FPGA, download the <code>fpga_support_files</code> archive:</p>

<ol>
	<li>Download <code>fpga_support_files.tgz</code> from <a href="http://registrationcenter-download.intel.com/akdlm/irc_nas/12954/fpga_support_files.tgz">the Intel Registration Center.</a> The files in this <code>.tgz</code> are required to ensure your FPGA card and Intel Distribution of OpenVINO toolkit work correctly. Right click or save the file instead of letting your browser extract automatically.</li>
	<li>Go to the directory where you downloaded <code>fpga_support_files.tgz</code>.</li>
	<li>Unpack the <code>.tgz</code> file:
		<pre class="brush:bash; class-name:dark;">tar -xvzf fpga_support_files.tgz</pre>
		A directory named <code>fpga_support_files</code> is created.</li>
	<li>Go to the <code>fpga_support_files</code> directory:
		<pre class="brush:bash; class-name:dark;">cd fpga_support_files</pre>
	</li>
</ol>

<p class="note"><strong>NOTE</strong>: These steps are for both Intel Arria 10 FPGA cards.</p>

<ol>
	<li>Switch to superuser:
		<pre class="brush:bash; class-name:dark;">sudo su</pre>
	</li>
	<li>Use the <code>setup_env.sh</code> script from <code>fpga_support_files.tgz</code> to set your environment variables:
		<pre class="brush:bash; class-name:dark;">source /home/&lt;user&gt;/Downloads/fpga_support_files/setup_env.sh</pre>
	</li>
	<li>Change directory to <code>Downloads/fpga_support_files/</code>:
		<pre class="brush:bash; class-name:dark;">cd /home/&lt;user&gt;/Downloads/fpga_support_files/</pre>
	</li>
	<li>Run the FPGA dependencies script, which allows OpenCL to support Ubuntu and recent kernels:
		<pre class="brush:bash; class-name:dark;">./install_openvino_fpga_dependencies.sh</pre>

<ul>
			<li>When asked, select the FPGA card, Intel® GPU, and Movidius Neural Compute Stick, then you can install the correct dependencies.</li>
			<li>If you installed the 4.14 kernel, you will need to reboot the machine and select the new kernel in the Ubuntu (grub) boot menu. You will also need to redo steps 1 and 2 to set up your environmental variables again.</li>
		</ul>
	</li>
	<li>Install OpenCL devices. Enter <code>Y</code> when prompted to install:
		<pre class="brush:bash; class-name:dark;">aocl install</pre>
	</li>
	<li>Reboot the machine:
		<pre class="brush:bash; class-name:dark;">reboot</pre>
	</li>
	<li>Use the <code>setup_env.sh</code> script from <code>fpga_support_files.tgz</code> to set your environment variables:
		<pre class="brush:bash; class-name:dark;">source /home/&lt;user&gt;/Downloads/fpga_support_files/setup_env.sh</pre>
	</li>
	<li>Run <code>aocl diagnose</code>:
		<pre class="brush:bash; class-name:dark;">aocl diagnose</pre>
		Your screen displays <code>DIAGNOSTIC_PASSED</code>.</li>
</ol>

<p>For the Intel Distribution of OpenVINO toolkit R5 with Intel® Vision Acceleration Design with Intel® Arria® 10 FPGA, continue to the <a href="#set-up-vision-accelerator-design-r5">next section</a> to program the board before running the samples and programming bitstreams. </p>

<p>Otherwise, you completed the FPGA installation and configuration.</p>
<h2><a id="set-up-vision-accelerator-design-r5" name="set-up-vision-accelerator-design-r5"></a>4. Set Up the Intel® Vision Accelerator Design with Intel® Arria® 10 FPGA for R5</h2>

<p>For the R5 release, the Intel® Distribution of OpenVINO™ toolkit introduced a new board support package (BSP) <code>a10_1150_sg1</code> for the Intel® Vision Accelerator Design with an Intel® Arria 10 FPGA, which is included into the <code>fpga_support_files.tgz</code> archive. To program the bitstreams for the Intel Distribution of OpenVINO toolkit R5, you need to program the BSP into the board using the USB blaster:</p>

<p class="note"><strong>NOTE</strong>: These steps apply only if you update to the Intel Distribution of OpenVINO toolkit R5. Otherwise, skip them to the <a href="#run the demos">Run the Demos to Verify Installation and Compile Samples</a>.</p>

<ol>
	<li>Go to the <code> config</code> folder of the <code>fpga_support_files</code> directory where the a10_1150_sg1 is located:

		<pre class="brush:bash; class-name:dark;">cd /home/&lt;user&gt;/Downloads/fpga_support_files/config/
</pre>
	</li>
	<li>Copy the <code>a10_1150_sg1</code> folder to the <code>board</code> directory:
		<pre class="brush:bash; class-name:dark;">sudo cp -rf a10_1150_sg1 /opt/altera/aocl-pro-rte/aclrte-linux64/board/
</pre>
	</li>
	<li>Convert the BSP files from DOS to UNIX:
		<pre class="brush:bash; class-name:dark;">sudo chmod +x a10_1150_sg1
find a10_1150_sg1 -type f -print0 | xargs -0 dos2unix	</pre>
	</li>
	<li>Set up the USB Blaster:
		<ol>
			<li>Connect the cable between the board and the host system. Use the letter codes in the diagram below for the connection points:
				<ul>
					<li>Connect the B end of the cable to point B on the board.</li>
					<li>Connect the F end of the cable to point F on the FPGA download cable.</li>
				</ul>


</li>
			<li>From point F end of the cable to point F on the FPGA download cable, the connection is as shown:

</li>
		</ol>
	</li>
	<li>Source the <code>setup_env.sh</code> script from the <code>fpga_support_files</code> to set up the environment variables:
		<pre class="brush:bash; class-name:dark;">source /home/&lt;user&gt;/Downloads/fpga_support_files/setup_env.sh
</pre>
	</li>
	<li>Update the Intel FPGA Download Cable rules to program the board without root permissions and to flash the initialization bitstreams so that the Intel FPGA Download Cable can communicate with the board: 
		<pre class="brush:bash; class-name:dark;">sudo cp config/51-usbblaster.rules /etc/udev/rules.d
</pre>
	</li>
	<li>Load the USB rules:
		<pre class="brush:bash; class-name:dark;">sudo udevadm control --reload-rules &amp;&amp; udevadm trigger
</pre>
	</li>
	<li>Unplug and re-plug the Intel® FPGA Download Cable to enable JTAG connection.</li>
	<li>Run <code>jtagconfig</code> to ensure that your Intel FPFA Download Cable driver is ready to use:
		<pre class="brush:bash; class-name:dark;">jtagconfig
</pre>

<p>Your output is similar to:</p>

<pre class="brush:bash; class-name:dark;">1) USB-Blaster [1-6]
02E660DD   10AX115H1(.|E2|ES)/10AX115H2/.. 
</pre>
</li>
	<li>Download <a href="http://fpgasoftware.intel.com/17.1/?edition=lite">Quartus Prime Lite Edition 17.1</a>. Install the Quartus Prime Lite to <code>/home/&lt;user&gt;/intelFPGA/17.1</code> directory.
		<p class="note"><strong>NOTE</strong>: You will need the complete Quartus Prime Lite version when you want to program the <code>boardtest_1ddr_top.aocx</code> into the flash for permanent availability.</p>
	</li>
	<li>Export the Quartus Prime Lite environment variable:
		<pre class="brush:bash; class-name:dark;">export QUARTUS_ROOTDIR=/home/&lt;user&gt;/intelFPGA/17.1/quartus
</pre>
	</li>
	<li>Go to <code>/opt/altera/aocl-pro-rte/aclrte-linux64/board/a10_1150_sg1/bringup</code>, where <code>boardtest_1ddr_top.aocx </code>is located:
		<pre class="brush:bash; class-name:dark;">cd /opt/altera/aocl-pro-rte/aclrte-linux64/board/a10_1150_sg1/bringup
</pre>
	</li>
	<li>Program the <code>boardtest_1ddr_top.aocx</code> file to the flash to be made permanently available even after power cycle:
		<pre class="brush:bash; class-name:dark;">aocl flash acl0 boardtest_1ddr_top.aocx
</pre>

<p class="note"><strong>NOTE</strong>: You will need the USB Blaster for this.</p>
	</li>
	<li>Reboot the host system.</li>
	<li>Check if the host system recognizes the Intel® Vision Accelerator Design with Intel® Arria® 10 FPGA board. Confirm you can detect the PCIe card:
		<pre class="brush:bash; class-name:dark;">lspci | grep -i Altera
</pre>

<p>Your output is similar to:</p>

<pre class="brush:bash; class-name:dark;">01:00.0 Processing accelerators: Altera Corporation Device 2494 (rev 01)
</pre>
</li>
	<li>Source the <code>setup_env.sh</code> script from the <code>fpga_support_files</code> to setup the environment variables:
		<pre class="brush:bash; class-name:dark;">source /home/&lt;user&gt;/Downloads/fpga_support_file/setup_env.sh
</pre>
	</li>
	<li>Uninstall the previous R4 BSP before installing the OpenCL drivers for the R5 BSP:
		<pre class="brush:bash; class-name:dark;">aocl uninstall /opt/altera/aocl-pro-rte/aclrte-linux64/board/&lt;R4_BSP_package&gt;/			</pre>
	</li>
	<li>Export and source the environment script:
<pre class="brush:bash; class-name:dark;">export AOCL_BOARD_PACKAGE_ROOT=/opt/altera/aocl-pro-rte/aclrte-linux64/board/a10_1150_sg1
</pre>

<pre class="brush:bash; class-name:dark;">source /opt/altera/aocl-pro-rte/aclrte-linux64/init_opencl.sh
</pre>
</li>
	<li>Install OpenCL devices:
		<pre class="brush:bash; class-name:dark;">aocl install	</pre>
	</li>
	<li>Run the <code>diagnose</code> command:
		<pre class="brush:bash; class-name:dark;">aocl diagnose		</pre>

<p>You should see <code>DIAGNOSTIC_PASSED</code> before proceeding to the next steps.</p>
	</li>
</ol>
<h2><a id="program a bitstream" name="program a bitstream"></a>5. Program a Bitstream</h2>

<p>The bitstream you program should correspond to the topology you want to deploy. In this section, you program a SqueezeNet bitstream and deploy the classification sample with a SqueezeNet model that you used the Model Optimizer to convert in the demo above.</p>

<p class="note"><strong>IMPORTANT</strong>: Only use bitstreams from the installed version of the Intel Distribution of OpenVINO toolkit. Bitstreams from older versions of the OpenVINO™ toolkit are incompatible with later versions of the Intel Distribution of OpenVINO toolkit. For example, you cannot use the <code>1-0-1_A10DK_FP16_Generic</code> bitstream, when the Intel Distribution of OpenVINO toolkit supports the <code>2-0-1_A10DK_FP16_Generic bitstream</code>.</p>

<p>Depending on how many bitstreams you selected, there are different folders for each FPGA card type which were downloaded in the Intel Distribution of OpenVINO package:</p>

<ul>
	<li>For the Intel® Arria 10GX Developer Kit FPGA, the pre-trained bitstreams are in the <code>/opt/intel/computer_vision_sdk/bitstreams/a10_devkit_bitstreams</code> directory. This demo uses a SqueezeNet bitstream with low precision for the classification sample.</li>
	<li>For the Intel® Vision Accelerator Design with Intel® Arria® 10 FPGA the pre-trained bistreams are in <code>/opt/intel/computer_vision_sdk/bitstreams/a10_vision_design_bitstreams</code>. This demo uses a SqueezeNet bitstream with low precision for the classification sample.</li>
</ul>

<ol>
	<li>Rerun the environment setup script:
		<pre class="brush:bash; class-name:dark;">source /home/&lt;user&gt;/Downloads/fpga_support_files/setup_env.sh</pre>
	</li>
	<li>Change to your home directory:
		<pre class="brush:bash; class-name:dark;">cd /home/&lt;user&gt;</pre>
	</li>
	<li>Choose the option based on which card you have:
		<ul>
			<li>Program the bitstream for Intel® Arria® 10 FPGA Development Kit:
				<pre class="brush:bash; class-name:dark;">aocl program acl0 /opt/intel/computer_vision_sdk/a10_devkit_bitstreams/2-0-1_A10DK_FP11_SqueezeNet.aocx</pre>
			</li>
			<li>Program the bitstream for the Intel® Vision Accelerator Design with Intel® Arria® 10 FPGA:
				<pre class="brush:bash; class-name:dark;">aocl program acl0 /opt/intel/computer_vision_sdk/bitstreams/a10_vision_design_bitstreams/4-0_PL1_FP11_SqueezeNet.aocx</pre>
			</li>
		</ul>
	</li>
</ol>

<h3>Optional Steps to Flash the FPGA Card</h3>

<p class="note"><strong>NOTE</strong>:<br>
	- To avoid having to reprogram the board after a power down, a bitstream will be programmed to permanent memory on the Intel® Arria® 10 GX FPGA Development Kit. This will take about 20 minutes.<br>
	- The following steps 1-5 need to be done only once for a new Intel® Arria 10 FPGA card.</p>

<ol>
	<li>Plug in the Micro USB cable to the card and your host system.</li>
	<li>Run <code>jtagconfig</code> to ensure that the cable is properly inserted:
		<pre class="brush:bash; class-name:dark;">jtagconfig</pre>
	</li>
	<li>Use <code>jtagconfig</code> to slow the clock:
		<pre class="brush:bash; class-name:dark;">jtagconfig --setparam 1 JtagClock 6M</pre>
	</li>
	<li>Store the Intel Arria 10 FPGA Development Kit bitstream long term on the board:
		<pre class="brush:bash; class-name:dark;">aocl flash acl0 /opt/intel/computer_vision_sdk/a10_devkit_bitstreams/2-0-1_A10DK_FP11_SqueezeNet.aocx</pre>
	</li>
	<li>Store the Intel Vision Accelerator Design with Intel Arria 10 FPGA bistream on the board:
		<pre class="brush:bash; class-name:dark;">aocl flash acl0 /opt/intel/computer_vision_sdk/bitstreams/a10_vision_design_bitstreams/4-0_PL1_FP11_SqueezeNet.aocx</pre>
	</li>
</ol>

<p>Your output is similar to:</p>

<pre class="brush:bash; class-name:dark;">USB-BlasterII [1-14]
02E660DD 10AX115H1(.|E2|ES)/10AX115H2/..
020A40DD 5M(1270ZF324|2210Z)/EPM2210
		</pre>

<h3>Setup a Neural Network Model for FPGA</h3>

<p>In this section, you create an FP16 model suitable for hardware accelerators. For more information, see the <a href="https://software.intel.com/en-us/articles/OpenVINO-InferEngine#fpga-plugin">FPGA plugins</a> section in the Inference Engine Developer Guide.</p>

<ol>
	<li>Create a directory for the FP16 SqueezeNet Model:
		<pre class="brush:bash; class-name:dark;">mkdir /home/&lt;user&gt;/squeezenet1.1_FP16</pre>
	</li>
	<li>Go to <code>/home/&lt;user&gt;/squeezenet1.1_FP16</code>:
		<pre class="brush:bash; class-name:dark;">cd /home/&lt;user&gt;/squeezenet1.1_FP16</pre>
	</li>
	<li>Use the Model Optimizer to convert an FP16 SqueezeNet Caffe model into an optimized Intermediate Representation (IR):
		<pre class="brush:bash; class-name:dark;">python3 /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/mo.py --input_model /home/&lt;user&gt;/openvino_models/FP32/classification/squeezenet/1.1/caffe/squeezenet1.1.caffemodel --data_type FP16 --output_dir .</pre>
	</li>
	<li>The <code>squeezenet1.1.labels</code> file contains the classes <code>ImageNet</code> uses. This file is included so that the inference results show text instead of classification numbers. Copy <code>squeezenet1.1.labels</code> to the your optimized model location:
		<pre class="brush:bash; class-name:dark;">cp /home/&lt;user&gt;/openvino_models/ir/squeezenet1.1/FP32/squeezenet1.1.labels  .</pre>
	</li>
	<li>Copy a sample image to the release directory. You will use this with your optimized model:
		<pre class="brush:bash; class-name:dark;">sudo cp /opt/intel/computer_vision_sdk/deployment_tools/demo/car.png  ~/inference_engine_samples/intel64/Release</pre>
	</li>
</ol>
