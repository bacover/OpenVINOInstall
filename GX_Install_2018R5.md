<p class="note"><strong>NOTE</strong>: This installation guide is written for the Intel® Arria® 10 GX FPGA Development Kit on Linux (Ubuntu/CentOS).</p>

<p>Installation Notes:</p>

<ul>
	<li>For a first-time installation, use all steps.</li>
	<li>Use steps 1 and 2 only after receiving a new FPGA card.</li>
	<li>Repeat steps 3-6 when installing a new version of the Intel Distribution of OpenVINO toolkit.</li>
	<li>Use steps 4-6 when a Neural Network topology used by an Intel Distribution of OpenVINO application changes.</li>
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
			<li>If you have an Intel® Quartus® license, use  <a href="http://fpgasoftware.intel.com/17.1/?edition=pro&amp;platform=linux&amp;download_manager=dlm3&amp;product=qprogrammer#tabs-4">QuartusProProgrammerSetup-17.1.0.240-linux</a>. This software uses about 500 MB of disk space.

<p><img alt="Quartus® Pro download screen" src="https://software.intel.com/sites/default/files/managed/7c/b9/Quartus%20Pro%20Download.png" title=""></p>
			</li>
			<li>If you do not have the license, use <a href="http://fpgasoftware.intel.com/17.1/?edition=lite&amp;platform=linux&amp;download_manager=dlm3&amp;product=qprogrammer#tabs-4">QuartusPrimeLiteProgrammerSetup-17.1.0.590-linux</a>. This software uses about 500 MB of disk space.
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

<h2><a id="program a bitstream" name="program a bitstream"></a>4. Program a Bitstream</h2>

<p>The bitstream you program should correspond to the topology you want to deploy. In this section, you program a SqueezeNet bitstream and deploy the classification sample with a SqueezeNet model that you used the Model Optimizer to convert in the demo above.</p>

<p class="note"><strong>IMPORTANT</strong>: Only use bitstreams from the installed version of the Intel Distribution of OpenVINO toolkit. Bitstreams from older versions of the OpenVINO™ toolkit are incompatible with later versions of the Intel Distribution of OpenVINO toolkit. For example, you cannot use the <code>1-0-1_A10DK_FP16_Generic</code> bitstream, when the Intel Distribution of OpenVINO toolkit supports the <code>2-0-1_A10DK_FP16_Generic bitstream</code>.</p>

<p>Depending on how many bitstreams you selected, there are different folders for each FPGA card type which were downloaded in the Intel Distribution of OpenVINO package:</p>

<ul>
	<li>For the Intel® Arria 10GX Developer Kit FPGA, the pre-trained bitstreams are in the <code>/opt/intel/computer_vision_sdk/bitstreams/a10_devkit_bitstreams</code> directory. This demo uses a SqueezeNet bitstream with low precision for the classification sample.</li>
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
				<pre class="brush:bash; class-name:dark;">aocl program acl0 /opt/intel/computer_vision_sdk/a10_devkit_bitstreams/5-0-1_A10DK_FP11_SqueezeNet.aocx</pre>
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
		<pre class="brush:bash; class-name:dark;">aocl flash acl0 /opt/intel/computer_vision_sdk/a10_devkit_bitstreams/5-0-1_A10DK_FP11_SqueezeNet.aocx</pre>
	</li>
</ol>

<p>Your output is similar to:</p>

<pre class="brush:bash; class-name:dark;">USB-BlasterII [1-14]
02E660DD 10AX115H1(.|E2|ES)/10AX115H2/..
020A40DD 5M(1270ZF324|2210Z)/EPM2210
		</pre>

<h2>5. Setup a Neural Network Model for FPGA</h2>

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

<h2> 6. Run a Sample Application </h2>

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
