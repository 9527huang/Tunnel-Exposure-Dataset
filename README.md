# Tunnel-Exposure-Datase

## 简介
本仓库旨在收集和共享一系列在不同曝光时间下拍摄的图像，以支持自动驾驶、机器人视觉等领域的研究。数据集涵盖了多种光照条件，有助于理解曝光对图像质量的影响。

## 数据集内容

### 数据结构

- **images/**: 存放原始图像文件。
- **annotations/**: 存放图像曝光时间信息的文件。
- **scripts/**：包含用于处理和分析数据的脚本，如计算PSNR和SSIM的代码
- **README.md**: 描述数据集的详细信息，包括数据集的来源、数据量、数据预处理步骤等。
- **LICENSE**: 描述数据集的使用许可。

### 数据处理

数据集在收集后进行了以处理步骤：

1. 调整图像大小至统一分辨率。
2. 对图像的曝光时间进行标注。
3. 对图像帧进行进一步筛选。

### 数据标注

数据集中的图像标注文件包含了对应图像的曝光时间信息。标注格式遵循[标注格式说明](#标注格式)。

为了确保数据集的高质量和实用性，我们设计了一套详细的标注方法，结合实际驾驶环境中的光照变化情况，对每帧图像的曝光时间进行精准标注。以下是具体的标注方法及流程：

#### 1. 语义分割与特征点提取

首先，对参考相机和扰动相机在每一时刻捕获的图像帧进行语义分割和特征点提取。通过语义分割技术，将图像中的不同区域按照语义类别进行分割，如道路、车辆、行人、隧道墙壁等。随后，在分割后的图像中提取特征点，并将不同类别特征点的数量表示为一个向量 \( \mathbf{m} = [m_1, m_2, \ldots, m_n] \)，其中 \( n \) 为特征点类别的数量， \( m_i \) 表示第 \( i \) 类特征点的数量。针对不同类别的特征点，设置相应的权重向量 \( \mathbf{w} = [w_1, w_2, \ldots, w_n] \)， \( w_i \) 表示第 \( i \) 类的权重。

首先，对参考相机和扰动相机在每一时刻捕获的图像帧进行语义分割和特征点提取。通过语义分割技术，将图像中的不同区域按照语义类别进行分割，如道路、车辆、行人、隧道墙壁等。随后，在分割后的图像中提取特征点，并将不同类别特征点的数量表示为一个向量 \( \mathbf{m} = [m_1, m_2, \ldots, m_n] \)，其中 \( n \) 为特征点类别的数量， \( m_i \) 表示第 \( i \) 类特征点的数量。针对不同类别的特征点，设置相应的权重向量 \( \mathbf{w} = [w_1, w_2, \ldots, w_n] \)， \( w_i \) 表示第 \( i \) 类的权重。

#### 2. 计算加权值

根据不同区域类别的特征点数量和权重信息计算加权值。在时刻 \( t \) ，计算参考相机图像帧的加权值 \( I_t^R \) 和扰动相机图像帧的加权值 \( I_{(t+a)}^P \) ，其中 \( a \in \{0, 1, 2\} \)。具体计算公式如下：
\[ I_{(t+a)}^c = \sum_{i=1}^n m_i \cdot w_i \]
其中 \( c \in \{P, R\} \)，分别表示参考相机和扰动相机。

#### 3. 曝光时间标注

选取最大加权值的相机索引对应图像帧的曝光时间，作为时刻 \( t \) 参考相机图像帧的标注数据。具体计算公式如下：
\[ (c^*, a^*) = \arg\max_{(c, a)} I_{(t+a)}^c \]
\[ E_t^* = E_{(t+a)}^{(c^*)} \]
通过上述方法，能够有效确定每一时刻参考相机的最佳曝光时间，确保图像质量和光照适应性。

#### 4. 基于图像质量的二次筛选

在初步标注曝光时间后，为进一步确保图像质量，引入了基于图像质量指标的二次筛选方法。通过计算每帧图像的峰值信噪比（PSNR）和结构相似性指数（SSIM），对图像进行质量评估。设置不同的PSNR和SSIM阈值，对图像帧进行筛选，剔除质量较差的图像。

具体阈值如下：
- **PSNR阈值**：24dB
- **SSIM阈值**：0.5

### 使用说明

1. 下载数据集至本地。
2. 解压数据集。
3. 使用提供的标注文件进行模型训练或评估。

### 数据集贡献

我们欢迎社区成员对数据集进行贡献，包括提供更多图像、更新标注文件等。请遵守[贡献指南](#贡献指南)进行贡献。

### 许可协议

本数据集根据[MIT LICENSE](LICENSE)进行发布。使用前请仔细阅读许可协议。

## 联系方式

如有任何问题或建议，请通过[邮箱](2583246961@qq.com)联系我们。
