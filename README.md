# Evaluation-of-NVIDIAs-Camera-to-Robot-Pose-Estimation-Deep-Learning-Research

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
<br>

<h3> Single-image inference </h3>
Inference on one image using DREAM-vgg-Q pretrained network:

```
python scripts/network_inference.py -i trained_models/panda_dream_vgg_q.pth -m data/real/panda-3cam_realsense/
```

<h3> Training </h3>
Below is an example for training a DREAM-vgg-Q model for the Franka Emika Panda robot:

python scripts/train_network.py -i data/synthetic/panda_synth_train_dr/ -t 0.8 -m manip_configs/panda.yaml -ar arch_configs/dream_hourglass_example.yaml -e 25 -lr 0.00015 -b 128 -w 16 -o <path/to/output_dir/>
The models below are defined in the following architecture files:

DREAM-vgg-Q: arch_configs/dream_vgg_q.yaml
DREAM-vgg-F: arch_configs/deam_vgg_f.yaml
DREAM-resnet-H: arch_configs/dream_resnet_h.yaml
DREAM-resnet-F: arch_configs/dream_resnet_f.yaml (very large network and unwieldy to train)

More information can be obtained from their official repo: [link](https://github.com/NVlabs/DREAM)

CITATION
  Lee, Timothy E and Tremblay, Jonathan and To, Thang and Cheng, Jia and Mosier, Terry and Kroemer, Oliver and Fox, Dieter and Birchfield, Stan, "Camera-to-Robot Pose Estimation from a Single Image", International Conference on Robotics and Automation (ICRA), 2020. https://arxiv.org/abs/1911.09231
