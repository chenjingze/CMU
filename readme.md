# 在昇腾训练平台上适配Hunyuan3D 2.0 模型的推理

Hunyuan3D模型是腾讯混元系列在2025年推出的一款3D资产创作模型，它可以用于生成带有高分辨率纹理贴图的高保真度3D模型。本项目旨在提供Hunyuan3D的NPU适配版本，方便用户能够在昇腾生态上直接使用Hunyuan3d

本项目基于Hunyuan3D 2.0 模型在NPU上的适配主要完成了以下优化点，具体内容可至[性能优化章节](#4-性能优化)查看： 

易用性优势体现：
- 支持pytorch前端框架
- 对于原生的模型示例改动很小

## 1 执行样例

### 1.1 环境准备

1. 请参考[版本要求](../../README.md)和[安装指导](../../README.md)完成NPU环境配置

2. 本样例以来的torch及torch_npu版本为2.5.1

3. 本仓库通过import torch_npu 
from torch_npu.contrib import transfer_to_npu  这句代码自动把torch.cuda系列api替换成torch.npu的底层实现

### 1.2 依赖安装

```shell
# 创建虚拟环境
CANN 8.1.RC1
conda create -n hunyuan3d python=3.10
conda activate hunyuan3d

# 安装Python依赖
pip3 install opencv-python-headless==4.10.0.84
pip3 install torch==2.5.1
pip3 install torch-npu==2.5.1 # 要和CANN版本对应
pip3 install torchvision==2.1.0
pip install -r requirements.txt
```

### 1.3 准备模型权重

| 模型      | 下载链接                                          |
|---------|----------------------------------------------|
| Hunyuan3D-DiT-v2-0 | (https://huggingface.co/tencent/Hunyuan3D-2/tree/main/hunyuan3d-dit-v2-0) |
| Hunyuan3D-Paint-v2-0 | (https://huggingface.co/tencent/Hunyuan3D-2/tree/main/hunyuan3d-paint-v2-0) |
| Hunyuan3D-Delight-v2-0 | (https://huggingface.co/tencent/Hunyuan3D-2/tree/main/hunyuan3d-delight-v2-0) |
| Hunyuan3D-DiT-v2-mv| (https://huggingface.co/tencent/Hunyuan3D-2/tree/main/hunyuan3d-delight-v2-0) |
使用huggingface-cli等工具下载

## 2. 执行推理
### 2.1 shape生成
- Hunyuan3D架构分为两个部分（1）基于图像引导的mesh生成网络 （2）基于图像和mesh网格的多视角图像生成及纹理贴图
- 运行shape_gen.py 实现 由单一图像引导的mesh生成
- 运行shape_gen_multiview.py实现由多面图像引导的mesh生成
python shape_gen.py
python shape_gen_multiview.py
shape_gen.py 中
