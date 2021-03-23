# Evaluation-of-NVIDIAs-Camera-to-Robot-Pose-Estimation-Deep-Learning-Research

Install the DREAM package and its dependencies using pip:

pip install . -r requirements.txt

Download the pre-trained models and (optionally) data. In the scripts below, be sure to comment out files you do not want, as they are very large. Alternatively, you can download files manually

cd trained_models; ./DOWNLOAD.sh; cd ..
cd data; ./DOWNLOAD.sh; cd ..


Training
Below is an example for training a DREAM-vgg-Q model for the Franka Emika Panda robot:

python scripts/train_network.py -i data/synthetic/panda_synth_train_dr/ -t 0.8 -m manip_configs/panda.yaml -ar arch_configs/dream_hourglass_example.yaml -e 25 -lr 0.00015 -b 128 -w 16 -o <path/to/output_dir/>
The models below are defined in the following architecture files:

DREAM-vgg-Q: arch_configs/dream_vgg_q.yaml
DREAM-vgg-F: arch_configs/deam_vgg_f.yaml
DREAM-resnet-H: arch_configs/dream_resnet_h.yaml
DREAM-resnet-F: arch_configs/dream_resnet_f.yaml (very large network and unwieldy to train)

CITATION
@inproceedings{lee2020icra:dream,
  title={Camera-to-Robot Pose Estimation from a Single Image},
  author={Lee, Timothy E and Tremblay, Jonathan and To, Thang and Cheng, Jia and Mosier, Terry and Kroemer, Oliver and Fox, Dieter and Birchfield, Stan},
  booktitle={International Conference on Robotics and Automation (ICRA)},
  year=2020,
  url={https://arxiv.org/abs/1911.09231}
}
