# ACL Single Stream Inference

## Introduction

This sample demonstrates how the InferOfflineVideo program to use the OpenCV to perform the 'YoloV3' object detection.

Process Framework:

```
StreamPull > DvppVdec > DvppCrop > ObjectDetection > PostProcess > WriteResult
```

## Supported Products

Atlas 800 (Model 3000), Atlas 800 (Model 3010), Atlas 300 (Model 3010), Atlas 300 (Model 3000), Atlas 500 Edge Station

## Supported ACL Version

1.73.5.1.B050, 1.73.5.2.B050

Run the following command to check the version in the environment where the Atlas product is installed:
```bash
npu-smi info
```

## Dependency

The input model is YoloV3. Please refer to [model transformation instructions](data/models/README.md) to transform the model.


Set the environment variable:  
*  `ASCEND_HOME`      Ascend installation path, which is generally `/usr/local/Ascend`
*  `ASCEND_VERSION`   acllib version number, which is used to distinguish different versions. Refer to the two-level directories in $ASCEND_HOME. Generally, the version number is <code>ascend-toolkit/*version*</code>
*  `ARCH_PATTERN`     acllib CPU architecture, view the `$ASCEND_HOME`/`$ASCEND_VERSION` folder, the value can be `x86_64-linux_gcc7.3.0`, `arm64-linux_gcc7.3.0`, or others

```bash
export ASCEND_HOME=/usr/local/Ascend
export ASCEND_VERSION=ascend-toolkit/latest
export ARCH_PATTERN=x86_64-linux_gcc7.3.0
```

#### Opencv

Source download address: https://github.com/FFmpeg/FFmpeg/releases

To compile and install ffmpeg with source code, you can refer to Ascend developers BBS: https://bbs.huaweicloud.com/blogs/140860,

For more information on Atlas cross-compilation, please refer to the product development guide, or search for resources in the internet.


Config FFMPEG4.2 library, set FFMPEG environment variable, for example FFMPEG install path is "/usr/local/ffmpeg"
```bash
export FFMPEG_PATH=/usr/local/ffmpeg
export LD_LIBRARY_PATH=$FFMPEG_PATH/lib:$LD_LIBRARY_PATH
```

## configuration

Configure the device_id, model_path and model input format in `data/config/setup.config`

Configure device_id
```bash
#chip config
device_id = 0 #use the device to run the program
```
Configure model_path
```bash
#yolov3 model path
od_model_path = ./data/models/yolov3/yolov3_416.om
```
Configure model input format
```bash
#yolov3 model input width and height
od_model_width = 416
od_model_height = 416
```

Configure the stream format, stream number and stream path in `data/config/stream.config`

Configure stream format
```bash
#Stream Format
format = H264 #H264 or H265
```
Configure stream number
```bash
#Stream number
stream_num = 16
```
Configure stream path
```bash
#stream configure
stream0 = rtsp://xx.xx.xx.xx:xx/xxx.264
stream1 = rtsp://xx.xx.xx.xx:xx/xxx.264
stream2 = rtsp://xx.xx.xx.xx:xx/xxx.264
stream3 = rtsp://xx.xx.xx.xx:xx/xxx.264
```


## Compilation

Compile Atlas 800 (Model 3000), Atlas 800 (Model 3010), Atlas 300 (Model 3010) programs
```bash
bash build.sh
```

Compile Atlas 500 (Model 3010) program
Run the following command on the ARM server
```bash
bash build.sh
```

Run the following command on the X86 server
```bash
bash build.sh A500
```

If you want to run with the compilation result on another environment, copy the dist directory and the ffmpeg dynamic libraries.

## Execution


For help
```bash
cd dist
./main -h

------------------------------help information------------------------------
-h                            help                          show helps                    
-help                         help                          show helps 
```

Object detection for the video stream
```bash
cd dist
./main
```

## Constraint

Support input format: H264 or H265



## Result

Print the result on the terminal: number of objects, frame id, channel id, position, confidence and label, then write them into result.txt.
```bash
[Info ][YoloManager.cpp YoloPostProcess:97] Object Nums is 2
[Info ][main.cpp PostProcess:53] Test frameId: 90
[Info ][main.cpp PostProcess:54] Test channelId 0
[Info ][main.cpp PostProcess:58] x1 is 386
[Info ][main.cpp PostProcess:59] y1 is 417
[Info ][main.cpp PostProcess:60] x2 is 621
[Info ][main.cpp PostProcess:61] y2 is 548
[Info ][main.cpp PostProcess:62] score is 0.984375
[Info ][main.cpp PostProcess:63] label is 17
[Info ][main.cpp PostProcess:58] x1 is 894
[Info ][main.cpp PostProcess:59] y1 is 467
[Info ][main.cpp PostProcess:60] x2 is 1192
[Info ][main.cpp PostProcess:61] y2 is 605
[Info ][main.cpp PostProcess:62] score is 0.979492
[Info ][main.cpp PostProcess:63] label is 17
```