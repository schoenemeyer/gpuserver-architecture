# Gpuserver-architecture
State-of-the-art Server architectures wirh GPUs

Among the 8 GPU Serer solutions in the market, the DGX-1 machine was tuned up specifically for deep learning. One of the major advantages compared to PCI based solution is the huge bandwidth of the NVLink .

In the table below, the NVLink specs are compared for the two generations. The DGX-1 with Tesla P100 is using NVLink Version 1 ports. That means each P100 accelerator has 4 NVLink ports with a birectional bandwidth f 40GB/s, which ends up in total 160GB/s for each P100 bidirectonal.
Each Tesla V100 has 6 NVLink connections running at a higher frequency, each capable of 50 GB/s of bidirectional bandwidth for an
aggregate of up to 300 GB/s bidirectional bandwidth. 

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/nvlink-table.JPG" width="452">

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/dgx-1-p100.JPG" width="452">

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/dgx-1-v100.JPG" width="452">

