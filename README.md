# PYNQ-Classification
Python on Zynq FPGA for Convolutional Neural Networks (Alpha)

This repository presents a fast prototyping framework, which is an Open Source framework designed to enable fast deployment of embedded Convolutional Neural Network (CNN) applications on PYNQ platforms.

The project demo accepts pre-trained CNN models in either Caffe or Theano syntax, hence the step 1 and 2 introduces how to install Caffe and Theano (with Lasagne) on PYNQ. Step 3 explains how to download and run the demo for LeNet and CIFAR-10 (Caffe "quick" version) models.

For details on the project please watch my video at: 

https://youtu.be/DoA8hKBltV4

## 1. Install Caffe on PYNQ

- Install dependencies

```
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
```

- Install Protobuf 3

`pip3 install protobuf`

- Add SWAP USB

Since PYNQ has limited RAM, in order to install Caffe extra memory will be required in the form of swap memory. In my test, a USB with size 2GB is large enough.

```
mkswap /dev/sda
swapon /dev/sda
```

- Install Caffe

```
cd /home/xilinx
git clone https://github.com/BVLC/caffe.git`
```

Copy the PYNQ version of Makefile.config (provided under PYNQ-Classification/CAFFE_ON_PYNQ) to caffe root directory

```
make all
make test
make runtest
```

- Unmount SWAP USB

`swapoff /dev/sda`

- Install pycaffe with Python 3

```
cd python
for req in $(cat requirements.txt); do sudo pip install $req; done
echo "export PYTHONPATH=$(pwd):$PYTHONPATH " >> ~/.bash_profile # to be able to call "import caffe" from Python after reboot
source ~/.bash_profile # Update shell 
cd .
```

- Export PYTHONPATH

Add the following line at the end of ".bashrc" file

```
export PYTHONPATH=/home/xilinx/caffe/python
```

## 2. Install Theano with Lasagne on PYNQ

```
pip install -r https://raw.githubusercontent.com/Lasagne/Lasagne/v0.1/requirements.txt
pip install Lasagne==0.1
```

## 3. Run CNN Demos on PYNQ

```
cd /home/xilinx/jupyter_notebooks
git clone https://github.com/awai54st/PYNQ-Classification.git
```

The following two demos show the implementions of LeNet and CIFAR-10 on PYNQ:

```
/home/xilinx/jupyter_notebooks/PYNQ-Classification/PYNQ_SIDE/Theano/Lenet/Using a Caffe Pretrained Network - LeNet5.ipynb
/home/xilinx/jupyter_notebooks/PYNQ-Classification/PYNQ_SIDE/Theano/CIFAR_10/Using a Caffe Pretrained Network - CIFAR10.ipynb
```

## 4. Vivado Project Setup - How to implement more CNN models?

The VIVADO_SIDE.7z package contains the Vivado project needed to generate bitstreams for customised CNN models. It contains the "layer IP library", "base project" and "block design project" as mentioned in the tutorial video. 

## References

### 1. Caffe

Caffe is released under the [BSD 2-Clause license](https://github.com/BVLC/caffe/blob/master/LICENSE).
The BAIR/BVLC reference models are released for unrestricted use.

Please cite Caffe in your publications if it helps your research:

    @article{jia2014caffe,
      Author = {Jia, Yangqing and Shelhamer, Evan and Donahue, Jeff and Karayev, Sergey and Long, Jonathan and Girshick, Ross and Guadarrama, Sergio and Darrell, Trevor},
      Journal = {arXiv preprint arXiv:1408.5093},
      Title = {Caffe: Convolutional Architecture for Fast Feature Embedding},
      Year = {2014}
    }
    
### 2. Theano

