# Gpuserver-architecture
State-of-the-art Server architectures wirh GPUs

Among the 8 GPU Serer solutions in the market, the DGX-1 machine was tuned up specifically for deep learning. One of the major advantages compared to PCI based solution is the huge bandwidth of the NVLink .

In the table below, the NVLink specs are compared for the two generations. The DGX-1 with Tesla P100 is using NVLink Version 1 ports. That means each P100 accelerator has 4 NVLink ports with a birectional bandwidth f 40GB/s, which ends up in total 160GB/s for each P100 bidirectonal.
Each Tesla V100 has 6 NVLink connections running at a higher frequency, each capable of 50 GB/s of bidirectional bandwidth for an
aggregate of up to 300 GB/s bidirectional bandwidth. 

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/nvlink-table.JPG" width="652">

The NVLink allows for a much faster transfer over .5 time faster than the traditional solution through PCIGen3 x 16.

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/dgx-1-p100.JPG" width="652">

With 6 ports it is possible to build an 8-GPU hybrid cube-mesh network. The corners of the mesh-connected faces of the cube are connected to the PCIe tree network, which also connects to the CPUs and NICs.

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/dgx-1-v100.JPG" width="562">

The hybrid cube-mesh topology (Figure 4) can be thought of as a cube with GPUs at its corners and with
all twelve edges connected through NVLink, and with two of the six faces having their diagonals
connected as well. It can also be thought of as two interwoven rings of single NVLink connections.

The cube-mesh topology provides the highest bandwidth of any 8-GPU NVLink topology for multiple
collective communications primitives, including broadcast, gather, and all-gather, which are important to
deep learning. Using NVLink connections to span the gap between the two clusters of four GPUs relieves
pressure on the PCIe bus and on the inter-CPU SMP link, and avoids staging transfers through system
memory when transferring across the two clusters.

<img src="https://github.com/schoenemeyer/gpuserver-architecture/blob/master/figures/meshcube-dgx.JPG" width="562">
