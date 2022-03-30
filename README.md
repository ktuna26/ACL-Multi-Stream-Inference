# ACL Multi Stream Inference

## Introduction
This sample demonstrates how the `ACL Single Stream Inference` program to use the OpenCV and ACL to perform the `yolov3 (caffe)` object detection.

Process Framework:

```
StreamPull[OpenCV] > PreProcess[Opencv] > ObjectDetection[ACL] > PostProcess[Opencv] > WriteResult
```


## Supported Products
Atlas 800 (Model 3000), Atlas 800 (Model 3010), Atlas 300 (Model 3010), Atlas 300 (Model 3000), Atlas 500 Edge Station


## Supported ACL Version
21.0.1, 21.0.2

Run the following command to check the version in the environment where the Atlas product is installed:
```bash
npu-smi info
```


## Dependency

### Model Conversion
The input model is `yolov3 (caffe)`. Please, follow the steps below.

https://obs-model-ascend.obs.cn-east-2.myhuaweicloud.com/yolov3/yolov3.caffemodel

Download the model weight file yolov3.caffemodel from this link or simply wget to ./model dir. And then start the conversion process by entering the following command.

```bash
cd ./model
atc --model=./yolov3.prototxt \
    --weight=./yolov3.caffemodel \
    --framework=0 \
    --output=./yolov3_caffe_416_no_csc \
    --soc_version=Ascend310 \
    --insert_op_conf=./aipp_yolov3_416_no_csc.cfg
```

### Prepare Environment
Set the environment variable:  
*  `ASCEND_HOME`      Ascend installation path, which is generally `/usr/local/Ascend`
*  `ASCEND_VERSION`   acllib version number, which is used to distinguish different versions. Refer to the two-level directories in $ASCEND_HOME. Generally, the version number is <code>ascend-toolkit/*version*</code>
*  `ARCH_PATTERN`     acllib CPU architecture, view the `$ASCEND_HOME`/`$ASCEND_VERSION` folder, the value can be `x86_64-linux`, `arm64-linux`, or others

```bash
export ASCEND_HOME=/usr/local/Ascend
export ASCEND_VERSION=ascend-toolkit/latest
export ARCH_PATTERN=x86_64-linux
```

#### Build Opencv from Source
Source download address: https://github.com/FFmpeg/FFmpeg/releases

To compile and install OpenCV with source code, you can refer to Linuxize: https://linuxize.com/post/how-to-install-opencv-on-ubuntu-18-04/

For more information on Atlas cross-compilation, please refer to the product development guide, or search for resources in the internet.

Build OpenCV 4.X.X library, set OpenCV environment variable, for example OpenCV install path is "/usr/local"

```bash
export OPENCV_PATH=/usr/local
export LD_LIBRARY_PATH=$OPENCV_PATH/lib:$LD_LIBRARY_PATH
```


## configuration
Configure the device_id, model_path and model input format in `data/config/setup.cfg`

Configure DeviceId
```bash
#chip config
DeviceId = 0 #use the device to run the program
```
Configure ChannelCount
```bash
ChannelCount = 1 # number of video/rts stream
```
Configure stream path
```bash
# stream of video file/rtsp
Stream.ch0 = ./data/video/test3.mp4
```
Configure model input format
```bash
# yolov3 model input width and height
ModelWidth = 416
ModelHeight = 416
```
Configure ModelPath
```bash
# yolov3 model path
ModelPath = ./data/model/yolov3.om
```
Configure NamesPath
```bash
# yolov3 label names path
NamesPath = ./data/config/coco.names
```


## Compilation
Run the following command on the terminal for compile program
```bash
./build.sh
```


## Execution
For help
```bash
cd dist
./main -h

------------------------------help information------------------------------
--acl_cfg                     ./data/config/acl.json        the config file using for ACL init.
--cfg                         ./data/config/setup.cfg       the config file using for face recognition pipeline.
--log_level                   1                             debug level:0-debug, 1-info, 2-warn, 3-error, 4-fatal, 5-off.
--sample_command                                            ./main
-h                            help                          show helps
-help                         help                          show helps
```

Object detection for the video stream
```bash
cd dist
./main
```


## Constraint
Support input format: mp4 or avi


## Result
Print the result on the terminal: fps, position, confidence and label, then write them into log file.
```bash
[INFO] [2022-03-04 09:50:42:683318][ObjectDetection.cpp Postprocess:256] x2 is 1353
[INFO] [2022-03-04 09:50:42:683329][ObjectDetection.cpp Postprocess:257] y2 is 152
[INFO] [2022-03-04 09:50:42:683339][ObjectDetection.cpp Postprocess:258] score is 84
[INFO] [2022-03-04 09:50:42:683349][ObjectDetection.cpp Postprocess:259] label is person 84%
[INFO] [2022-03-04 09:50:42:683381][main.cpp RunDetector:177] FPS : 25.358
[INFO] [2022-03-04 09:50:42:807613][ObjectDetection.cpp Postprocess:254] x1 is 1223
[INFO] [2022-03-04 09:50:42:807655][ObjectDetection.cpp Postprocess:255] y1 is 1
[INFO] [2022-03-04 09:50:42:807667][ObjectDetection.cpp Postprocess:256] x2 is 1351
[INFO] [2022-03-04 09:50:42:807677][ObjectDetection.cpp Postprocess:257] y2 is 152
[INFO] [2022-03-04 09:50:42:807687][ObjectDetection.cpp Postprocess:258] score is 85
[INFO] [2022-03-04 09:50:42:807698][ObjectDetection.cpp Postprocess:259] label is person 85%
[INFO] [2022-03-04 09:50:42:932701][ObjectDetection.cpp Postprocess:254] x1 is 1224
[INFO] [2022-03-04 09:50:42:932734][ObjectDetection.cpp Postprocess:255] y1 is 1
[INFO] [2022-03-04 09:50:42:932746][ObjectDetection.cpp Postprocess:256] x2 is 1350
[INFO] [2022-03-04 09:50:42:932757][ObjectDetection.cpp Postprocess:257] y2 is 152
[INFO] [2022-03-04 09:50:42:932767][ObjectDetection.cpp Postprocess:258] score is 85
[INFO] [2022-03-04 09:50:42:932777][ObjectDetection.cpp Postprocess:259] label is person 85%
```

## Authors
Kubilay Tuna


## Copyright
Huawei Technologies Co., Ltd


## License
Apache License, Version 2.0