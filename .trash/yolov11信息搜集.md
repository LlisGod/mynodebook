---
tags:
  - yolo
  - 实验室
---

##  需要收集的信息 ：  
  
1、YOLOv11由谁提出、哪一年/那个公司发布？

2、它相较于YOLO之前的版本（v10等等）的改进点是什么？

3、适用于哪些场景？是否仍然保持单阶段结构？

4、模型版本（如：YOLOv11n, s, m等）及其参数规模、速度表现，哪个版本最快最平衡。

5、yolov11是否跟v5一样包含骨干，颈部和头部网络，这三个部分分别是什么作用

## 答案
1.

	1. 发布日期：2024.09
	
	2. 作者:Ultralytics 团队
	
	3. 项目地址: https://github.com/ultralytics/
	
	4. 主要功能:
	
	Jltralytics YOLO11 is a cutting-edge  , state-of-the-art (SOTA) model that builds upon the success of previous YOLO versions and introduces new features and improvements to further boost performance and flexibility. YOLO11 is designed to be fast, accurate, and easy to use, making it an excellent choice for a wide range of object detection and tracking, instance segmentation, image classification and pose estimation tasks.
[YOLOv1–v11: 版本演进及其关键技术解析_yolov1-v11-CSDN博客](https://blog.csdn.net/hadoopdevelop/article/details/142677591)

2.
	
	Ultralytics YOLO 11 与前代产品相比，该版本有多项重大改进。主要改进包括

	增强型特征提取： YOLO 11 采用改进的骨干和颈部结构，增强了特征提取能力，从而实现更精确的目标检测。
	
	优化的效率和速度：经过改进的架构设计和优化的训练管道可提供更快的处理速度，同时保持准确性和性能之间的平衡。
	
	参数更少，精度更高：与 YOLOv 8 m 相比，YOLO 11 m 在 COCO 数据集上实现了更高的平均精确度 (mAP)，而参数数量却减少了 22%，这使其在不影响精确度的前提下提高了计算效率。
	
	跨环境适应性： YOLO 11 可在各种环境中部署，包括边缘设备、云平台和支持 NVIDIA GPU 的系统。
	
	支持的任务范围广泛： YOLO 11 支持多种计算机视觉任务，如物体检测、实例分割、图像分类、姿态估计和定向物体检测 (OBB)。
[Ultralytics YOLO11 -Ultralytics YOLO 文档](https://docs.ultralytics.com/zh/models/yolo11/#citations-and-acknowledgements)

3.
	物体检测：识别和定位图像中的物体。
	实例分割：检测对象并划定其边界。
	图像分类：将图像分为预定义的类别。
	姿势估计：检测和跟踪人体关键点
	定向物体检测 (OBB)：通过旋转来检测物体，精度更高。
	是的

4. ![[Pasted image 20250505164124.png]]

![[Pasted image 20250505164321.png]]

[Ultralytics YOLO11 -Ultralytics YOLO 文档](https://docs.ultralytics.com/zh/models/yolo11/#__tabbed_1_1)

5. 
Yolov 11 是根据 yolov 8 改进而来，整体的架构没有发生变化，一样包含骨干，颈部和头部网络
	**骨干网络**是模型的核心特征提取器，负责从输入图像中提取多层次、多尺度的特征信息。这些特征会传递到后续网络中进行进一步处理。
	**颈部网络**连接骨干网络和头部，负责对不同尺度的特征图进行融合与增强，以结合浅层的高分辨率细节特征和深层的语义信息。
	**头部网络**负责最终的预测任务，包括目标的位置回归、分类和置信度计算。

Yolov 11 改进：
	骨干网络在 YOLOv 8 的基础上增加了 C 2 PSA 模块，并通过 C 3 K 2 的灵活性优化特征提取流程，提升模型对小目标和复杂场景的适应能力
	在 SPPF 模块后新增 C 2 PSA 模块，通过注意力机制强化重要特征的权重，减少噪声干扰
	引入 YOLOv 10 的头部设计思想，通过深度可分离卷积进一步优化参数量和计算效率，使得模型在保持精度的同时减少 22%的参数
	
	
[YOLOv11 网络结构及YOLOv8 模型间性能对比（合适新人）-CSDN博客](https://blog.csdn.net/weixin_44445800/article/details/146273979)