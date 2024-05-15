# Feedback for Remote Computer Graphics 


![Overview](images/2024-05-15_09-42.png)



#### Requirements

 - **GPU Passthrough** with secure Iommu Groups. (No PCI bypass)
- **tensorflow-gpu-jupyter** docker container
- Installation of **Nvidia Drivers from .run**
- **nvidia-container-toolkit** 

In this example will not be installing Nvidia Drivers with APT, although to install others such as nvidia-continer-toolkit it may be necissary to add the repository with nvidia's gpg key. 

> https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#installing-with-apt


# Install Nvidia Drivers with .run 

need to be able to run nvidia-smi within our Virtual Machine.  

> NVIDIA-Linux-x86_64-550.78.run # From Nvidia, install on virtual machine. 

#### Run nvidia-smi to confirm communication between the vm and gpu. 
```
nvidia-smi 

```

> | NVIDIA-SMI 550.78                 Driver Version: 550.78         CUDA Version: 12.4     |
|-------------

#### lists the runtimes available through docker.
```
docker info | grep Runtimes 

```

 > Runtimes: nvidia runc io.containerd.runc.v2 io.containerd.runtime.v1.linux

#### Starting the container with correct runtime and is compiled for gpu. 
1. Start the container with **--runtime=nvidia**

2. make sure to use tensorflow that is compiled for gpu use: **latest-gpu-jupyter**

```
docker run -it --rm --runtime=nvidia -v $(realpath ~/notebooks):/tf/notebooks -p 8888:8888 tensorflow/tensorflow:latest-gpu-jupyter
```

#### Verify that tensorflow has GPU's available
1. print the GPU physical device available. 

```
print(tf.__version__)
print("Num GPUs Available: ", len(tf.config.list_physical_devices('GPU')))

```


![Num Gpu Available](images/2024-05-15_09-33.png)


