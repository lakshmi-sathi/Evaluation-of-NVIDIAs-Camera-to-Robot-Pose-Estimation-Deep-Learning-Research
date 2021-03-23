# Evaluation-of-NVIDIAs-Camera-to-Robot-Pose-Estimation-Deep-Learning-Research
<h3> Introduction </h3>
Single-Image Pose Estimation as introduced by NVIDIA through their [work](https://github.com/NVlabs/DREAM) is evaluated on the Jaco Gen2 Robot Arm from Kinova Robotics.
More info on the Original work from NVIDIA [here](https://sim2realai.github.io/dream-camera-calibration-sim2real/)

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

python scripts/train_network.py -i data/synthetic/jaco_synth_train_dr/ -t 0.8 -m manip_configs/jaco2.yaml -ar arch_configs/dream_vgg_q.yaml -e 25 -lr 0.00015 -b 128 -w 16 -o <path/to/output_dir/>

The model configurations are defined in the architecture files in 'arch_configs' directory.

More information can be obtained from their official repo: [link](https://github.com/NVlabs/DREAM)

CITATION
  Lee, Timothy E and Tremblay, Jonathan and To, Thang and Cheng, Jia and Mosier, Terry and Kroemer, Oliver and Fox, Dieter and Birchfield, Stan, "Camera-to-Robot Pose Estimation from a Single Image", International Conference on Robotics and Automation (ICRA), 2020. https://arxiv.org/abs/1911.09231
