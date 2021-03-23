# Evaluation-of-NVIDIAs-Camera-to-Robot-Pose-Estimation-Deep-Learning-Research
<h3> Introduction </h3>

Single-Image Pose Estimation as introduced by NVIDIA through their [work](https://github.com/NVlabs/DREAM) is evaluated on the Jaco Gen2 Robot Arm from Kinova Robotics.
More info on the Original work from NVIDIA [here](https://sim2realai.github.io/dream-camera-calibration-sim2real/)

![image](https://user-images.githubusercontent.com/58559090/112131071-26d6fb80-8bef-11eb-907f-b330e2431472.png)


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
Blender is an opensource tool for 3D modelling and animation and is readily available for [download](https://www.blender.org/download/). <br>
Unreal Engine is an open-source game engine and [NDDS](https://github.com/NVIDIA/Dataset_Synthesizer) is a domain randomized data synthesizer plugin for Unreal Engine developed by NVIDIA. <br>
In order to obtain randomized arm configurations it is required to make the arm model moveable (Rigging) and Blender was serves that purpose. The next step in the process is generate a randomized datset for which Unreal Engine and its plugin NDDS is used. Unreal Engine requires the '.FBX' format for the rigged model.<br>
For this conversion of the model file obtained in '.STEP' format first it is converted from ‘.STEP’ to ‘.STL’ using [FreeCAD](https://www.freecadweb.org/) and then Blender was used to convert '.STL' to '.FBX'. This FBX format is the one that can be opened in both Blender and, Unreal Engine which is used to generate a randomized dataset for Jaco. <br>

![image](https://user-images.githubusercontent.com/58559090/112128509-81228d00-8bec-11eb-8218-e1b88fee850c.png)


<h3> Reference/Main Work </h3>
  Lee, Timothy E and Tremblay, Jonathan and To, Thang and Cheng, Jia and Mosier, Terry and Kroemer, Oliver and Fox, Dieter and Birchfield, Stan, "Camera-to-Robot Pose Estimation from a Single Image", International Conference on Robotics and Automation (ICRA), 2020. https://arxiv.org/abs/1911.09231
