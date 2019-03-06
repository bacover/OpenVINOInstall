
<p class="note"><strong>NOTE</strong>: This installation guide is written for the Intel® Vision Accelerator Design with an Intel® Arria 10 FPGA (IEI's Mustang-F100-A10) on Linux (Ubuntu/CentOS).</p>

<p>Installation Notes:</p>

<ul>
	<li>For a first-time installation, use all steps.</li>
	<li>Use steps 1 and 2 only after receiving a new FPGA card.</li>
	<li>Repeat steps 2-5 when installing a new version of the Intel Distribution of OpenVINO toolkit.</li>
	<li>Use steps 3-5 when a Neural Network topology used by an Intel Distribution of OpenVINO application changes.</li>
</ul>

<hr>
<h2>1. Configure and Install the Intel® Vision Accelerator Design with an Intel® Arria 10 FPGA<a id="configure aria 10 gx fpga" name="configure aria 10 gx fpga"></a></h2>

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
			<li>If you installed the 4.14 kernel as part of the installation script, you will need to reboot the machine and select the new kernel in the Ubuntu (grub) boot menu. You will also need to rerun setup_env.sh to set up your environmental variables again.</li>
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


<h2><a id="set-up-vision-accelerator-design-r5" name="set-up-vision-accelerator-design-r5"></a>2. Set Up the Intel® Vision Accelerator Design with Intel® Arria® 10 FPGA for R5</h2>

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

<img src="https://github.com/bacover/OpenVINOInstall/blob/master/VisionAcceleratorJTAG.png">

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
	<li>Use <code>jtagconfig</code> to slow the clock:
		<pre class="brush:bash; class-name:dark;">jtagconfig --setparam 1 JtagClock 6M</pre>
	</li>
	<li>(OPTIONAL) Confirm the clock is set to 6M:
		<pre class="brush:bash; class-name:dark;">jtagconfig --getparam 1 JtagClock</pre>

<p>You should see the following:</p>
		<code>6M</code></li>
	<br><br>

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
<h2><a id="program a bitstream" name="program a bitstream"></a>3. Program a Bitstream</h2>

<p>The bitstream you program should correspond to the topology you want to deploy. In this section, you program a SqueezeNet bitstream and deploy the classification sample with a SqueezeNet model that you used the Model Optimizer to convert in the demo above.</p>

<p class="note"><strong>IMPORTANT</strong>: Only use bitstreams from the installed version of the Intel Distribution of OpenVINO toolkit. Bitstreams from older versions of the OpenVINO™ toolkit are incompatible with later versions of the Intel Distribution of OpenVINO toolkit. For example, you cannot use the <code>1-0-1_A10DK_FP16_Generic</code> bitstream, when the Intel Distribution of OpenVINO toolkit supports the <code>2-0-1_A10DK_FP16_Generic</code> bitstream.</p>

<p>Depending on how many bitstreams you selected, there are different folders for each FPGA card type which were downloaded in the Intel Distribution of OpenVINO package:</p>

<ul>
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
			<li>Program the bitstream for the Intel® Vision Accelerator Design with Intel® Arria® 10 FPGA:
				<pre class="brush:bash; class-name:dark;">aocl program acl0 /opt/intel/computer_vision_sdk/bitstreams/a10_vision_design_bitstreams/5-0_PL1_FP11_SqueezeNet.aocx</pre>
			</li>
		</ul>
	</li>
</ol>

<h3>Optional Steps to Flash the FPGA Card</h3>

<p class="note"><strong>NOTE</strong>:<br>
	- To avoid having to reprogram the board after a power down, a bitstream will be programmed to permanent memory on the Intel® Vision Accelerator Design with Intel® Arria® 10 FPGA. This will take about 20 minutes.<br>
	- The following steps 1-5 need to be done only once for a new Intel® Arria 10 FPGA card.</p>

<ol>
	<li>Plug in the Micro USB cable to the card and your host system.</li>
	<li>Run <code>jtagconfig</code> to ensure that the cable is properly inserted:
		<pre class="brush:bash; class-name:dark;">jtagconfig</pre>
	</li>
	<li>Use <code>jtagconfig</code> to slow the clock:
		<pre class="brush:bash; class-name:dark;">jtagconfig --setparam 1 JtagClock 6M</pre>
	</li>
	<li>Store the Intel Vision Accelerator Design with Intel Arria 10 FPGA bistream on the board:
		<pre class="brush:bash; class-name:dark;">aocl flash acl0 /opt/intel/computer_vision_sdk/bitstreams/a10_vision_design_bitstreams/5-0_PL1_FP11_SqueezeNet.aocx</pre>
	</li>
</ol>

<p>Your output is similar to:</p>

<pre class="brush:bash; class-name:dark;">USB-BlasterII [1-14]
02E660DD 10AX115H1(.|E2|ES)/10AX115H2/..
020A40DD 5M(1270ZF324|2210Z)/EPM2210
		</pre>

<h2>4. Setup a Neural Network Model for FPGA</h2>

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
<h2> 5. Run a Sample Application </h2>

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
