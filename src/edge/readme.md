# IoT Edge samples

This folder contains IoT Edge deployment manifest templates and sample IoT Edge modules.

## Deployment manifest templates

### deployment.template.json

This file is a deployment manifest template that has the following modules defined in it

* rtspsim - [RTSP simulator module](https://github.com/Azure/live-video-analytics/tree/master/utilities/rtspsim-live555)
* lvaEdge - Live Video Analytics on IoT Edge module

### deployment.httpExtension.template.json

In addition to the moduels defined in deployment.template.json, this deployment manifest template includes a sample httpExtension module (source code for which can be found in ./modules/httpExtension). The sample httpExtension module takes video frames as input via HTTP, calculates the average color, classifes the image as 'light' or 'dark', and returns the results in the HTTP response.


### deployment.grpc.template.json  

In addition to the modules defined in deployment.template.json, this deployment manifest template includes your [grpcExtension module](https://github.com/Azure-Samples/live-video-analytics-iot-edge-csharp/tree/master/src/edge/modules/grpcExtension). This IoT Edge module runs your custom AI behind a gRPC endpoint. 

### deployment.customvision.template.json

In addition to the modules defined in deployment.template.json, this deployment manifest template includes the toy truck detector model built using [Custom vision](https://www.customvision.ai/). This is an IoT Edge module that runs the Custom Vision model behind an HTTP endpoint.This template is used in [this](https://docs.microsoft.com/en-us/azure/media-services/live-video-analytics-edge/custom-vision-tutorial) tutorial that gives you training data for the toy truck detector custom vision model and outlines instructions on training the model, uploading to your own Azure container registry and deploying it to the edge. 

### deployment.grpcyolov3icpu.template.json  

In addition to the modules defined in deployment.template.json, this deployment manifest template includes this [yolov3 module](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/notebooks/Yolo/yolov3/yolov3-grpc-icpu-onnx/lvaextension). This IoT Edge module runs the YoloV3 ONNX model behind a gRPC endpoint. This template is used in [this](https://aka.ms/lva-grpc-quickstart) quickstart.

### deployment.objectCounter.template.json

In addition to the modules defined in deployment.yolov3.template.json, this deployment manifest template includes the sample objectCounter module (source code for which can be found in ./modules/objectCounter). This template also has message routes defined to send messages from the lvaEdge module to the objectCounter module and vice versa, to enable the scenario of recording video clips when objects of a specified type (at a confidence measure above a specified threshold value) are found. See [this](https://docs.microsoft.com/azure/media-services/live-video-analytics-edge/event-based-video-recording-tutorial) tutorial on using this template.

### deployment.openvino.grpc.cpu.template.json

In addition to the modules defined in deployment.template.json, this deployment manifest template includes the [OpenVINO™ DL Streamer – Edge AI Extension Module](https://aka.ms/lva-intel-openvino-dl-streamer) module. This module serves video analytics pipelines built with OpenVINO™ DL Streamer. Developers can send decoded video frames to the AI extension module which performs detection, classification, or tracking and returns the results. The AI extension module exposes gRPC APIs with support for high frame rates (i.e. 30fps). This template is used in [this](https://aka.ms/lva-intel-grpc) tutorial.

### deployment.openvino.grpc.gpu.template.json

This template is the same as defined in deployment.openvino.grpc.cpu.template.json, but here the GPU support is enabled so that the inferencing will use the GPU. This deployment manifest template includes the [OpenVINO™ DL Streamer – Edge AI Extension Module](https://aka.ms/lva-intel-openvino-dl-streamer) module. This module serves video analytics pipelines built with OpenVINO™ DL Streamer. Developers can send decoded video frames to the AI extension module which performs detection, classification, or tracking and returns the results. The AI extension module exposes gRPC APIs with support for high frame rates (i.e. 30fps). This template is used in [this](https://aka.ms/lva-intel-grpc) tutorial.

### deployment.openvino.template.json  

In addition to the modules defined in deployment.template.json, this deployment manifest template includes the [OpenVINO™ Model Server – AI Extension](https://aka.ms/lva-intel-ovms) module. This inference server module contains the OpenVINO™ Model Server (OVMS), an inference server powered by the OpenVINO™ toolkit, that is highly optimized for computer vision workloads and developed for Intel architectures. This template is used in [this](https://aka.ms/lva-intel-ovms-tutorial) tutorial.

### deployment.spatialAnalysis.template.json  

In addition to the modules defined in deployment.template.json, this deployment manifest template includes the [Computer Vision for spatial analysis AI module from Azure Cognitive Services](https://docs.microsoft.com/en-us/azure/cognitive-services/computer-vision/spatial-analysis-container?tabs=azure-stack-edge). This module supports operations that enable you to count people in a designated zone within the camera’s field of view, to track when a person crosses a designated line or area, or when people violate a distance rule. This template is used in [this](https://aka.ms/lva-spatial-analysis) tutorial.

### deployment.yolov3.template.json

In addition to the modules defined in deployment.template.json, this deployment manifest template includes the [yolov3 module](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx). This is an IoT Edge module that runs the YoloV3 ONNX model behind an HTTP endpoint.

### deployment.composite.template.json

In addition to the modules defined in deployment.template.json, this deployment manifest template includes the [yolov3 module](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/yolov3-onnx) and the [tinyyolov3 module](https://github.com/Azure/live-video-analytics/tree/master/utilities/video-analysis/notebooks/Yolo/tinyyolov3/tinyyolov3-grpc-icpu-onnx). This is an IoT Edge module that runs both the YoloV3 ONNX and Tiny YoloV3 ONNX models behind gRPC endpoints.

## Deployment manifest template variables

The deployment manifest templates contains several variables (look for '$' symbol). The values for these variables need to be specified in a .env file that you should create in the same folder as the template files. This file should like the following

```env
SUBSCRIPTION_ID="<your Azure subscription id>"
RESOURCE_GROUP="<your resource group name>"
AMS_ACCOUNT="<name of your Media Services account>"
IOTHUB_CONNECTION_STRING="<IoT Hub connection string>"
AAD_TENANT_ID="<your AAD tenant ID>"
AAD_SERVICE_PRINCIPAL_ID="<your AAD service principal id>"
AAD_SERVICE_PRINCIPAL_SECRET="<your AAD service principal secret>"
INPUT_VIDEO_FOLDER_ON_DEVICE="<a folder on your edge device with MKV files, used by RTSP simulator>"
OUTPUT_VIDEO_FOLDER_ON_DEVICE="<a folder on your edge device used for recording video clips>"
APPDATA_FOLDER_ON_DEVICE="<a folder on your edge device used for storing application data>"
CONTAINER_REGISTRY_USERNAME_myacr="<user name for your Azure Container Registry>"
CONTAINER_REGISTRY_PASSWORD_myacr="<password for the registry>"
```

To generate a deployment manifest from the template, open your local clone of this git repository in Visual Studio Code, have the [Azure Iot Tools](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) extension installed. Create the .env file with the above variables and their values. Then right click on the template file and select "Generate IoT Edge deployment manifest". This will create the corresponding deployment manifest file in a **./config** folder.

## Sample edge modules

### objectCounter

The folder **./modules/objectCounter** contains source code for an IoT Edge module that counts objects of a specified type and with a confidence above a specified threshold value (these are specified as twin properties in deployment.objectCounter.template.json). The module expects messages emitted by yolov3 module (referenced above).

### httpExtension

The folder **./modules/httpExtension** contains source code for an IoT Edge module that exposes a HTTP endpoint where video frames can be posted. Upon receiving the frames, the module calculates the average color and classifies the image as 'light' or 'dark' and returns the result (using the inference schema defined by LVA) in the HTTP response.

## Learn more

* [Develop IoT Edge modules](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-develop-for-linux)
