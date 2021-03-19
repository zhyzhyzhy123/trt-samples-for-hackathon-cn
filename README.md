# Introduction
This is a collection of simplified TensorRT samples to get you started with TensorRT programming.
Most of the samples are written in C++ and some are in Python to show the basics.

# Build and Run
For C++ samples, please install TensorRT properly and modify the Makefile according to your setup.
For Python samples, please install TensorRT with Python wheel, and install PyTorch and onnx_graphsurgeon.

# C++ Samples
## AppBasic
This is a basic sample which shows how to build and run an engine with static-shaped input (which we'll call "static-shape engine" for short) and save the engine to disk.
This sample introduces a reusable class <code>TrtLite</code>, which will be used throughout these samples. The class is concise (~300 lines of code) yet covers most usage of TensorRT and simplifies its programming.
## AppDynamicShape
This sample shows how to build and run an engine with dynamic-shaped input ("dynamic-shape engine" for short), and how to copy data and run the engine asynchronously. You may use Nsight Systems to see the timeline of GPU events.

It's important to overlap copying data, including copying from host memory to device memory for input and vice versa for output, and running the engine. This technique make it possible to run inference tasks consecutively on GPU and maximize the throughput. The technique showed in this sample can also be applied to others.
## AppLoadRefit
This sample shows how to load an engine from disk and run it. 

It also shows how to refit an engine. To refit a FP16 engine efficiently you need to save the build logs to a file. See the inlined comments for details.
## AppInt8
This sample shows 3 use cases:
1. Build a static-shape engine and run it in int8.
2. Build a dynamic-shape engine and run it in int8.
3. Build a quantization-aware trained (QAT) networks and run it in int8.
## AppPlugin
This sample shows how to write a simple static-shape plugin. This plugin supports fp32/fp16/int8 precision. Please be noted that the plugin interface IPluginV2IOExt is for and only for static shape.
## AppPluginDynamicShape
This sample is AppPlugin in dynamic shape. Please be noted that the plugin interface IPluginV2DynamicExt is for and only for dynamic shape. 
## AppMultiContext
This sample shows how to create multiple contexts from the same engine thus saving device memory space for network weights, and how to run the contexts on their own stream. 

Like AppDynamicShape, it also runs asynchronously and you may use Nsight Systems to see the timeline of GPU events.
## AppThroughput
This sample loads an engine from disk and get a benchmark on how many QPS can be achieved for max throughput. By default the sample loads an engine created from a Python sample and a command utility (see below).

# Python Samples
## run_trt_lite.py
This sample shows how to build, save and run static-shape and dynamic-shape engines. 
TensorRT can be programmed with C++ or Python; C++ program can load and run the engine saved by Python program and vice versa.
## onnx_resnet50.py
This sample shows how to export a PyTorch model into onnx. With the trtexec command in the script, you can convert onnx into TensorRT engine file by the utility trtexec (so the engine can be loaded and run such as by AppThroughput).
## onnx_custom.py
This sample shows how to export a PyTorch model containing unsupported operator into onnx, and how to modify the onnx with graph surgeon so it can be converted into TensorRT engine smoothly.
