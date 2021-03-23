# Evaluation-of-NVIDIAs-Camera-to-Robot-Pose-Estimation-Deep-Learning-Research
<h3> Introduction </h3>

Single-Image Pose Estimation as introduced by NVIDIA through their [work](https://github.com/NVlabs/DREAM) is evaluated on the Jaco Gen2 Robot Arm from Kinova Robotics.
More info on the Original work from NVIDIA [here](https://sim2realai.github.io/dream-camera-calibration-sim2real/). Majority of the code and Data is made available by them.

![image](https://user-images.githubusercontent.com/58559090/112132364-897cc700-8bf0-11eb-8164-a21e5c7d0a60.png)

Image shows the network inference output on a synthetic image.

For achieving the target of this work (Single-Image Pose Estimation of Jaco Arm) the steps would be:
* Generation of a Synthetic Dataset with randomized lighting, positions, interfering objects, colours, and configurations for the Jaco Gen 2 robot arm.
  - Rigging of Jaco Arm Model in Blender
  - Generation of Dataset with randomization using Unreal Engine + NDDS
* Train the VGG auto-encoder network (convolutional layers pretrained on ImageNet) with this synthetic dataset and assess the performance using real images of the Jaco Robot arm.

<h3> Setup </h3>
Install the DREAM package and its dependencies using pip:

```
pip install . -r requirements.txt
```

<h3> Downloading Models and Data </h3>
Comment out the unwanted models and data in the 'DOWNLOAD.sh' scripts and run it for downloading the required data and trained models. <br>
Example:- <br>

```
cd trained_models; ./DOWNLOAD.sh; cd \..
cd data; ./DOWNLOAD.sh; cd \..
```

<h3> Single-image inference </h3>
Inference on one image using DREAM-vgg-Q pretrained network (on first image of Panda-3Cam dataset):

```
python scripts/network_inference.py -i trained_models/panda_dream_vgg_q.pth -m data/real/panda-3cam_realsense/000000.rgb.jpg
```

<h3> Training </h3>
Training a DREAM-vgg-Q model for the Jaco robot:

```
python scripts/train_network.py -i data/synthetic/jaco_synth_train_dr/ -t 0.8 -m manip_configs/jaco2.yaml -ar arch_configs/dream_vgg_q.yaml -e 25 -lr 0.00015 -b 128 -w 16 -o <path/to/output_dir/>
```

The model configurations are defined in the architecture files in 'arch_configs' directory. The synthetic dataset generated is not provided here due to the large size. <br>

More information can be obtained from their official repo: [link](https://github.com/NVlabs/DREAM)
  
<h3> Method Followed </h3>

The arm we wanted to train the network with is Jaco2. The arm model for Jaco was obtained from [Kinova Robotics](https://www.kinovarobotics.com/en/resources/gen2-technical-resources). The target is to generate a synthetic dataset for training the network. The tools going to be need are Blender, Unreal Engine and NDDS(Plugin). <br>
[Blender](https://www.blender.org/download/) is an opensource tool for 3D modelling and animation and is readily available for download. [Unreal Engine](https://www.unrealengine.com/en-US/) is an open-source game engine and [NDDS](https://github.com/NVIDIA/Dataset_Synthesizer) is a domain randomized data synthesizer plugin for Unreal Engine developed by NVIDIA. <br>
In order to obtain randomized arm configurations it is required to make the arm model moveable (Rigging) and Blender serves that purpose. The next step in the process is generate a randomized datset for which Unreal Engine and its plugin NDDS is used. Unreal Engine requires the '.FBX' format for the rigged model.<br>
For this conversion of the model file obtained in '.STEP' format first it is converted from ‘.STEP’ to ‘.STL’ using [FreeCAD](https://www.freecadweb.org/) and then Blender was used to convert '.STL' to '.FBX'. This FBX format is the one that can be opened in both Blender and, Unreal Engine which is used to generate a randomized dataset for Jaco. <br>

![image](https://user-images.githubusercontent.com/58559090/112128509-81228d00-8bec-11eb-8218-e1b88fee850c.png)
The image shows Jaco Arm model loaded in Blender.

The Blender 3D animation tool has an ‘Armature’ feature that enables us to setup a skeletal system for 3D models so that it can be moved and animated as required. This was one of the targets with using the Blender tool. <br>

Here you can see the bones are assigned one per each segment of the Jaco robot arm from the base till the fingers tips. In graphics animation terms it is called “Rigging” the Jaco Arm model. 

![image](https://user-images.githubusercontent.com/58559090/112143733-4b869f80-8bfe-11eb-8da8-ccff2c3f18cb.png)

Here is shown the skeleton being developed (rigging) for the Jaco arm model. This skeleton is needed for later moving the Jaco arm to random positions for generation of the synthentic dataset.


<h3> Reference/Main Work </h3>
  Lee, Timothy E and Tremblay, Jonathan and To, Thang and Cheng, Jia and Mosier, Terry and Kroemer, Oliver and Fox, Dieter and Birchfield, Stan, "Camera-to-Robot Pose Estimation from a Single Image", International Conference on Robotics and Automation (ICRA), 2020. https://arxiv.org/abs/1911.09231
