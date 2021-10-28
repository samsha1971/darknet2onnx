# darknet2onnx

代码是从 https://github.com/Tianxiaomo/pytorch-YOLOv4/裁剪出来的，主要裁剪内容是把darknet模型导出为ONNX模型。

## 验证通过环境

python=3.9

TensorRT=8.2.0.6

CUDA=11.0

CUDNN=8.1.0

## 从darknet模型导出ONNX模型

```shell
python demo_darknet2onnx.py ./weight/yolov4-tiny.cfg ./weight/yolov4-tiny.weights ./data/dog.jpg 1
```

如果需要的话，可以精简模型:

```shell
pip install onnx-simplifier

python -m onnxsim yolov4_1_3_416_416_static.onnx yolov4_1_3_416_416_static_sim.onnx
```

## 从ONNX模型导出TensorRT模型

```shell
# fp16
trtexec --onnx=yolov4_1_3_416_416_static.onnx --saveEngine=yolov4_-1_3_416_416_static.engine --fp16

# int8
trtexec --onnx=yolov4_1_3_416_416_static.onnx --saveEngine=yolov4_-1_3_416_416_static.engine --int8
```

## 调用TensorRT模型的示例

```shell
# 图片样例
python demo_trt.py ./yolov4_-1_3_416_416_static.engine ./data/dog.jpg

# 视频样例
python demo_trt_video.py ./yolov4_-1_3_416_416_static.engine ./data/5.mp4
```

