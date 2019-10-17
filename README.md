# ESE532fall2019_emulation
This repo is to show you how to use simulation and emulation to debug your code, which will be implemented by SDSoC.

# Description
SDSoC is an excellent tool for SoC design. It exclude a lot of gluing logic design between hardware and software. The designers can focus on some high level architection design. However, it abstracts away some essential details, which is bad for debugging. Nevertheless, it offers you some methods, by which you can do some low level debuging. Emulation is one of the useful tools, which we will focus on in the following sections.


# Buffer Lock Example
Include all the source code under ./examples/BufferLock/ into SDSoC. Move the `loss_HW` into hardware as figure below.
![](images/bufferLockConfig.jpg)
This system is to use DMA to transfer data and labels into hardware and do some calculations and return the data back into DDR ram. We will use `hls:stream` data type to connect the module `norm` and `square_loss`. If you create a `vivado_HLS` project and do the C simulation, it is tatally fine. However, if you compile the SDSoC, you will get nothing when downloading it into the board.  

![](images/buffer_lock_system.jpg)


## Emulation for the Buffer Lock
Now, let's use emulation to find the bugs. As the emulation does not support the Utral-96v2 yet, we tempararily create a ZCU102 board project to do the emulation as below. Make sure you use `A53 Linux` as the System configuration. Choose Target as `Emulation`.


![](images/zcu102_bufferlock.jpg)


After the compilation, we can open the hardware reports to see how the function and interface is implemented. From the report, we can see that the interface between `norm` and `square_loss` are 2 fifos, corresponding to `hls:stream` interfaces in software.




In the SDSoC, click `Xilinx->Start/Stop emulation`. A `vivado` software would jump out. Choose the signals you are interested in. Here we choose the IO of `X_norm_V_U` and `LABEL_norm_V_U`. Click run in the `vivado` side, and launch emulation in the software side. The fifo's full signal is asserted, but we did not see and input for the label_norm. 

![](images/fifo_full.jpg)

 

















