# Image-particle-counter-V5
荧光颗粒计数 保留自 v3.3/v4.1 的经典功能，适用于免疫荧光、病毒样颗粒、荧光微球等场景。 五种可选算法：   LoG 斑点检测：拉普拉斯高斯，精度最高，提供半径估计   DoG 快速检测：高斯差分，速度快2倍，同样提供半径   霍夫圆检测：圆形投票，对正圆细胞/颗粒极稳健   局部最大值：仅检测亮度峰值，速度最快，无半径信息   自适应阈值+连通域：应对背景不均、不规则形状   额外添加了AI推荐参数预设和SAM模块微调识图精准度的功能
## 🤖 SAM AI 分割模式（可选）

### 是什么？
SAM（Segment Anything Model）是 Meta 发布的零样本图像分割大模型。在本工具中，它作为**传统颜色阈值法的精修插件**：
1. 先用传统算法（HSV/LAB/RGB 阈值）圈出大致阳性区域
2. 再用 SAM 在该区域内提取精确的对象边界
3. 最终得到既**颜色准确**又**边缘细腻**的分割结果

### 适用场景
- ✅ 油红O脂滴**严重粘连**，传统 Watershed 分割效果差
- ✅ 茜素红/ALP矿化结节**边缘模糊**，需要精确面积统计
- ✅ 结节**互相粘连**，传统连通域连成一片

### 不适用场景
- ❌ 极小颗粒（<5px）或荧光点状颗粒（请用荧光颗粒模块）
- ❌ 需要极速批量处理（SAM 单图需 5–20 秒）
- ❌ 电脑内存 < 8GB 或没有 GPU/强 CPU

### 本地启用步骤
```bash
pip install segment-anything torch torchvision

# 下载模型文件（375MB）
mkdir -p ~/.cache/segment_anything
cd ~/.cache/segment_anything
wget https://dl.fbaipublicfiles.com/segment_anything/sam_vit_b_01ec64.pth
