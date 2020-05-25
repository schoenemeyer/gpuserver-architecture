A100 Architecture
what the press announced on Ampere in May 2020:     

German Language:
https://www.heise.de/news/Nvidia-Ampere-A100-Volle-Grafikfunktion-und-CPU-unabhaengig-4726187.html      
https://www.it-markt.ch/news/2020-05-20/nvidias-neue-profi-gpu-ist-ein-ki-monster    
https://www.golem.de/news/a100-nvidias-7-nm-monster-gpu-misst-826-mm-2005-148480.html     

NVIDIA Devblog   
https://devblogs.nvidia.com/nvidia-ampere-architecture-in-depth/        
https://devblogs.nvidia.com/introducing-hgx-a100-most-powerful-accelerated-server-platform-for-ai-hpc/     


NVIDIA Whitepaper on A100
https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/nvidia-ampere-architecture-whitepaper.pdf      






# Multi-GPU Server-architecture

Prior to Nvidia’s current solution, in order to form more powerful compute nodes, multiple GPUs were connected over a PCIe switch and directly to the CPU. One can find servers with up to 8 GPUs packed into one server.

Among the 8 GPU Server solutions in the market, the DGX-1 machine was tuned up specifically for deep learning. One of the major advantages compared to PCI based solutions is the ultrafast NVLink interconnect of the Tesla GPUs whihc outperforms the traditional PCIgen3 connect.

In the table below, the NVLink specs are compared for the two generations. The DGX-1 with Tesla P100 is using NVLink Version 1 ports. That means each P100 accelerator has 4 NVLink ports with a birectional bandwidth f 40GB/s, which ends up in total 160GB/s for each P100 bidirectonal.
Each Tesla V100 has 6 NVLink connections running at a higher frequency, each capable of 50 GB/s of bidirectional bandwidth for an
aggregate of up to 300 GB/s bidirectional bandwidth. 

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/nvlink-table.JPG" width="652">

The NVLink allows for a much faster transfer over 2.5 time faster than the traditional solution through PCIGen3 x 16.

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/dgx-1-p100.JPG" width="652">

With 6 ports it is possible to build an 8-GPU hybrid cube-mesh network. The corners of the mesh-connected faces of the cube are connected to the PCIe tree network, which also connects to the CPUs and NICs. 
The topology can be thought of as a cube with GPUs at its corners and with
all twelve edges connected through NVLink, and with two of the six faces having their diagonals
connected as well. It can also be thought of as two interwoven rings of single NVLink connections.


<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/dgx-1-v100.JPG" width="562">


The cube-mesh topology provides the highest bandwidth of any 8-GPU NVLink topology for multiple
collective communications primitives, including broadcast, gather, and all-gather, which are important to
deep learning. Using NVLink connections to span the gap between the two clusters of four GPUs relieves
pressure on the PCIe bus and on the inter-CPU SMP link, and avoids staging transfers through system
memory when transferring across the two clusters.

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/meshcube-dgx.JPG" width="362">

Keep in mind why NVIDIA did this strong investment into the NVLink Interconnect due to balancing GPU interconnect with GPU memory speed. The Tesla GPUs come with an outstanding memory bandwidth (up to 900GB/s) from GPU to HMB2 memory. 

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/nvidia-dgx-1-v100-nvlink-gpu-xeon-config.png" width="562">

The next step in this development was the NVIDIA NVSwitch which is the first on-node switch architecture to support 16 fully-connected GPUs in a single server node and drive simultaneous communication between all eight GPU pairs 300 GB/s each. The DGX-2 has two baseboards fully linked together using the NVSwitches on each of the baseboards forming a system of 16 fully-connected GPUs. These 16 GPUs can be used as a single large-scale accelerator with 0.5 Terabytes of unified memory space and 2 petaFLOPS of deep learning compute power and can outperform two DGX-1 systems connected via EDR infinband significantly. 
The DGX-2 yields 81,920 CUDA cores and an additional 12,240 Tensor cores chip for AI workloads and requires 10kW of power.

A detailed design can be found in this article. 

https://fuse.wikichip.org/news/1224/a-look-at-nvidias-nvlink-interconnect-and-the-nvswitch/



<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/dgx2-nvswitch-two-baseboards.png" width="562">

If things comes together in a nicely integrated way, you can build a very powerful supercomputer which is called Summit awith a mixed-precision capability in excess of 3 EF.

https://www.olcf.ornl.gov/for-users/system-user-guides/summit/summit-user-guide/

