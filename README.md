# deepstream-test1-segmentation

**Description**

A segmentation inference sample code using NVIDIA DeepStream SDK. Modified deepstream-test1 sample app to accept segmentation models and output the masks.


This sample code only modifies the ``deepstream-test1`` sample that belongs to NVIDIA. The original sample can be found at ``/opt/nvidia/deepstream/deepstream<VERSION>/sources/apps/sample_apps/deepstream-test1``.  The modification regards the use of Unet Segmentation models with a h264 input stream. 

The segmentation sample in ``/opt/nvidia/deepstream/deepstream<VERSION>/sources/apps/sample_apps/deepstream-segmentation`` was also used as base to make these modifications, and this is a naive sample only. There is a lot to be changed if you want to use it in a real world application. This sample is only meant to test Unet segmentation models in h264 input streams.


There's also a CMake file to compile DeepStream code, both on dGPU (Desktop) applications and Jetson (TEGRA) embbeded boards. This CMake file was tested in many DeepStream samples and worked correctly. If it does not work on a specific case, it should be enough as an example. 

**Running** 

First make sure to check your DeepStream version in the ``CMakeLists.txt``. The default is 5.0. This CMake file also checks wether you're running the sample in dGPU or Jetson board. Then build the sample with:

<pre>
  <code>
    $ mkdir build && cd build
    $ cmake ..
    $ make
  </code>
</pre>

Now, check the ``dstest_segmentation_config_semantic.txt`` config file to change the details about your model path, inference dims and other aspects. Then copy this file to the `build` folder created.


Now run the sample with an h264 video as input:


<code>
  $ ./deepstream-test1-segmentation <path_to_h264_video>
</code>

