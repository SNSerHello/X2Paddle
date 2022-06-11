# X2Paddle

## 搭建Anaconda3的x2paddle环境

```
$ conda create -n py37_x2paddle2.x python=3.7 ipython
$ pip3 install --upgrade -r requirements.txt
$ python3 setup.py bdist_wheel --universal
$ pip3 install --upgrade dist/x2paddle-1.3.6-py2.py3-none-any.whl
```

## 转化Tensorflow1.x模型 -> Paddle-Lite模型

从[Tensorflow Slim](https://github.com/tensorflow/models/tree/master/research/slim)下载Tensorflow模型文件，比如说：[mobilenet_v1_1.0_224.tgz](http://download.tensorflow.org/models/mobilenet_v1_2018_02_22/mobilenet_v1_1.0_224.tgz)
```
$ wget http://download.tensorflow.org/models/mobilenet_v1_2018_02_22/mobilenet_v1_1.0_224.tgz
$ tar xvzf mobilenet_v1_1.0_224.tgz
$ x2paddle --model mobilenet_v1_1.0_224_frozen.pb \
        --save_dir tmp \
        --framework tensorflow \
        --define_input_shape \
        --to_lite True \
        --lite_valid_places opencl \
        --lite_model_type naive_buffer
Define shape[now is [-1, 224, 224, 3]] for input tensor[tensor name: "input']
Use your keyboard type the shape of input tensor below :)
Shape of Input(e.g. None,224,224,3): None,224,224,3
$ tree /A /F tmp
Folder PATH listing for volume Data
Volume serial number is 00000002 0DE3:11B6
tmp
|   model.pdparams
|   opt.nb
|   x2paddle_code.py
|
+---inference_model
|       model.pdiparams
|       model.pdiparams.info
|       model.pdmodel
|
\---__pycache__
        x2paddle_code.cpython-37.pyc
```

## 转化ONNX模型 -> Paddle-Lite模型

可以从[mobilenet ONNX](https://github.com/onnx/models/tree/main/vision/classification/mobilenet)下载mobilenetv2版本，比如说[mobilenetv2-12.onnx](https://github.com/onnx/models/blob/main/vision/classification/mobilenet/model/mobilenetv2-12.onnx)

```
$ wget https://github.com/onnx/models/blob/main/vision/classification/mobilenet/model/mobilenetv2-12.onnx
$ x2paddle --model mobilenetv2-12.onnx \
        --save_dir tmp \
        --framework onnx \
        --define_input_shape \
        --to_lite True \
        --lite_valid_places opencl \
        --lite_model_type naive_buffer
INFO:root:paddle.__version__ = 2.3.0
INFO:root:Now translating model from onnx to paddle.
model ir_version: 7, op version: 12
Unknown shape for input tensor[tensor name: 'x2paddle_input'] -> shape: ['batch_size', 3, 224, 224], Please define shape of in
put here,
Note:you can use visualization tools like Netron to check input shape.
Shape of Input(e.g. -1,3,224,224), enter 'N' to skip: -1,3,224,224
$ tree /A /F tmp
Folder PATH listing for volume Data
Volume serial number is 00000002 0DE3:11B6
TMP
|   model.pdparams
|   opt.nb
|   x2paddle_code.py
|
+---inference_model
|       model.pdiparams
|       model.pdiparams.info
|       model.pdmodel
|
\---__pycache__
        x2paddle_code.cpython-37.pyc
```


[![PyPI - X2Paddle Version](https://img.shields.io/pypi/v/x2paddle.svg?label=pip&logo=PyPI&logoColor=white)](https://pypi.org/project/x2paddle/)
[![PyPI Status](https://pepy.tech/badge/x2paddle/month)](https://pepy.tech/project/x2paddle)
[![License](https://img.shields.io/badge/license-Apache%202-blue.svg)](LICENSE)
[![Version](https://img.shields.io/github/release/PaddlePaddle/X2Paddle.svg)](https://github.com/PaddlePaddle/X2Paddle/releases)
![python version](https://img.shields.io/badge/python-3.5+-orange.svg)  

**【问卷调查】** 为了更好的推进飞桨框架以及X2Paddle的迭代开发，诚邀您参加我们的问卷，期待您的宝贵意见:https://iwenjuan.baidu.com/?code=npyd51

## 简介

X2Paddle是飞桨生态下的模型转换工具，致力于帮助其它深度学习框架用户快速迁移至飞桨框架。目前支持**推理模型的框架转换**与**PyTorch训练代码迁移**，我们还提供了详细的不同框架间API对比文档，降低开发者上手飞桨核心的学习成本。



## 特性

- **支持主流深度学习框架**

  - 目前已经支持Caffe/TensorFlow/ONNX/PyTorch四大框架的预测模型的转换，PyTorch训练项目的转换，涵盖了目前市面主流深度学习框架

- **支持的模型丰富**

  - 在主流的CV和NLP模型上均支持转换，涵盖了19+个Caffe预测模型转换、27+个TensorFlow预测模型转换、32+个ONNX预测模型转换、27+个PyTorch预测模型转换、2+个PyTorch训练项目转换，详见 ***[支持列表](./docs/introduction/x2paddle_model_zoo.md)***

- **简洁易用**

  - 一条命令行或者一个API即可完成模型转换



## 能力

- **预测模型转换**

  - 支持Caffe/TensorFlow/ONNX/PyTorch的模型一键转为飞桨的预测模型，并使用PaddleInference/PaddleLite进行CPU/GPU/Arm等设备的部署

- **PyTorch训练项目转换**

  - 支持PyTorch项目Python代码（包括训练、预测）一键转为基于飞桨框架的项目代码，帮助开发者快速迁移项目，并可享受[AIStudio平台](https://aistudio.baidu.com/)对于飞桨框架提供的海量免费计算资源[**【新功能，试一下！】**](/docs/pytorch_project_convertor/README.md)

- **API对应文档**

  - 详细的API文档对比分析，帮助开发者快速从PyTorch框架的使用迁移至飞桨框架的使用，大大降低学习成本 [**【新内容，了解一下！】**](docs/pytorch_project_convertor/API_docs/README.md)



## 安装

### 环境依赖
- python >= 3.5  
- paddlepaddle >= 2.2.2
- tensorflow == 1.14 (如需转换TensorFlow模型)
- onnx >= 1.6.0 (如需转换ONNX模型)
- torch >= 1.5.0 (如需转换PyTorch模型)
- paddlelite >= 2.9.0 (如需一键转换成Paddle-Lite支持格式,推荐最新版本)

### pip安装(推荐）

如需使用稳定版本，可通过pip方式安装X2Paddle：
```
pip install x2paddle
```

### 源码安装

如需体验最新功能，可使用源码安装方式：
```
git clone https://github.com/PaddlePaddle/X2Paddle.git
cd X2Paddle
git checkout develop
python setup.py install
```

## 快速开始

### 功能一：推理模型转换

#### PyTorch模型转换
``` python
from x2paddle.convert import pytorch2paddle
pytorch2paddle(module=torch_module,
               save_dir="./pd_model",
               jit_type="trace",
               input_examples=[torch_input])
# module (torch.nn.Module): PyTorch的Module。
# save_dir (str): 转换后模型的保存路径。
# jit_type (str): 转换方式。默认为"trace"。
# input_examples (list[torch.tensor]): torch.nn.Module的输入示例，list的长度必须与输入的长度一致。默认为None。
```
```script```模式以及更多细节可参考[PyTorch模型转换文档](./docs/inference_model_convertor/pytorch2paddle.md)。

#### TensorFlow模型转换
```shell
x2paddle --framework=tensorflow --model=tf_model.pb --save_dir=pd_model
```

#### ONNX模型转换
```shell
x2paddle --framework=onnx --model=onnx_model.onnx --save_dir=pd_model
```

#### Caffe模型转换
```shell
x2paddle --framework=caffe --prototxt=deploy.prototxt --weight=deploy.caffemodel --save_dir=pd_model
```

#### 转换参数说明

| 参数                 | 作用                                                         |
| -------------------- | ------------------------------------------------------------ |
| --framework          | 源模型类型 (tensorflow、caffe、onnx)                         |
| --prototxt           | 当framework为caffe时，该参数指定caffe模型的proto文件路径     |
| --weight             | 当framework为caffe时，该参数指定caffe模型的参数文件路径      |
| --save_dir           | 指定转换后的模型保存目录路径                                 |
| --model              | 当framework为tensorflow/onnx时，该参数指定tensorflow的pb模型文件或onnx模型路径 |
| --caffe_proto        | **[可选]** 由caffe.proto编译成caffe_pb2.py文件的存放路径，当存在自定义Layer时使用，默认为None |
| --define_input_shape | **[可选]** For TensorFlow, 当指定该参数时，强制用户输入每个Placeholder的shape，见[文档Q2](./docs/inference_model_convertor/FAQ.md) |
| --enable_code_optim  | **[可选]** For PyTorch, 是否对生成代码进行优化，默认为True |
| --to_lite            | **[可选]** 是否使用opt工具转成Paddle-Lite支持格式，默认为False |
| --lite_valid_places  | **[可选]** 指定转换类型，可以同时指定多个backend(以逗号分隔)，opt将会自动选择最佳方式，默认为arm |
| --lite_model_type    | **[可选]** 指定模型转化类型，目前支持两种类型：protobuf和naive_buffer，默认为naive_buffer |
| --disable_feedback   | **[可选]** 是否关闭X2Paddle使用反馈；X2Paddle默认会统计用户在进行模型转换时的成功率，以及转换框架来源等信息，以便于帮忙X2Paddle根据用户需求进行迭代，不会上传用户的模型文件。如若不想参与反馈，可指定此参数为False即可 |

#### X2Paddle API
目前X2Paddle提供API方式转换模型，可参考[X2PaddleAPI](docs/inference_model_convertor/x2paddle_api.md)

#### 一键转换Paddle-Lite支持格式
可参考[使用X2paddle导出Padde-Lite支持格式](docs/inference_model_convertor/convert2lite_api.md)

### 功能二：PyTorch模型训练迁移

项目转换包括3个步骤

1. 项目代码预处理
2. 代码/预训练模型一键转换
3. 转换后代码后处理

详见[PyTorch训练项目转换文档](./docs/pytorch_project_convertor/README.md)。


## 使用教程

1. [TensorFlow预测模型转换教程](./docs/inference_model_convertor/demo/tensorflow2paddle.ipynb)
2. [MMDetection模型转换指南](./docs/inference_model_convertor/toolkits/MMDetection2paddle.md)
3. [PyTorch预测模型转换教程](./docs/inference_model_convertor/demo/pytorch2paddle.ipynb)
4. [PyTorch训练项目转换教程](./docs/pytorch_project_convertor/demo/README.md)

## 更新历史

**2021.07.09**  

1. 新增MMDetection模型库支持，包括YOLO-V3、FCOS、RetinaNet、SSD、Faster R-CNN以及FSAF，有相关AP精度对比，具体参考[MMDetection模型转换指南](./docs/inference_model_convertor/toolkits/MMDetection2paddle.md)。
2. 新增PyTorch训练代码转换对[CRAFT](https://github.com/clovaai/CRAFT-pytorch)的支持，新增PyTorch预测模型转换对Seg-Swin-Transformer的支持。
3. 优化模型预测速度，去除forward函数开头to_tensor操作。
4. 新增Tensorflow op映射（1个）：Sign。
5. 新增ONNX op映射（4个）：NMS、ReduceL1、ReduceL2、3D Interpolate。

**2021.05.13**  

- 新增PyTorch训练项目功能：
  支持转换的项目有[StarGAN](https://github.com/yunjey/stargan)、[Ultra-Light-Fast-Generic-Face-Detector-1MB](https://github.com/Linzaer/Ultra-Light-Fast-Generic-Face-Detector-1MB)。

**2021.04.30**

1. 新增支持转换的模型：[SwinTransformer](https://github.com/microsoft/Swin-Transformer/)、[BASNet](https://github.com/xuebinqin/BASNet)、[DBFace](https://github.com/dlunion/DBFace)、[EasyOCR](https://github.com/JaidedAI/EasyOCR)、[CifarNet](https://github.com/tensorflow/models/blob/master/research/slim/nets/cifarnet.py)等。
2. 支持Windows上使用本工具。
3. 新增TensorFlow op映射（4个）：SplitV、ReverseV2、BatchToSpaceND、SpaceToBatchND。
4. 新增PyTorch op映射（11个）：aten::index、aten::roll、aten::adaptive_avg_pool1d、aten::reflection_pad2d、aten::reflection_pad1d、aten::instance_norm、aten::gru、aten::norm、aten::clamp_min、aten::prelu、aten:split_with_sizes。
5. 新增ONNX op映射（1个）：DepthToSpace。
6. 新增Caffe op映射（1个）：MemoryData。

**更多版本更新记录可查阅[X2Paddle发版历史](https://github.com/PaddlePaddle/X2Paddle/releases)**

## :hugs:贡献代码:hugs:

我们非常欢迎您为X2Paddle贡献代码或者提供使用建议。如果您可以修复某个issue或者增加一个新功能，欢迎给我们提交Pull Requests，如果有PyTorch训练项目转换需求欢迎随时提issue~
