# 基于 TensorFlow Lite 的 Android 花卉识别应用

![Android](https://img.shields.io/badge/Android-4.1+-green.svg)
![TensorFlow Lite](https://img.shields.io/badge/TensorFlow%20Lite-2.3.0-orange.svg)
![Kotlin](https://img.shields.io/badge/Kotlin-1.3.72-blue.svg)
![License](https://img.shields.io/badge/License-Apache%202.0-red.svg)

这是一个基于 TensorFlow Lite 的实时花卉识别 Android 应用，使用 CameraX 进行相机预览和图像分析，支持 GPU 加速。本项目包含完整的 Android 应用代码和 TensorFlow 模型训练 Notebook。

---

## 🌟 功能特性

- 📷 **实时花卉识别** - 使用手机相机实时识别花卉
- 🚀 **GPU 加速支持** - 自动检测 GPU 兼容性并加速推理
- 📊 **Top 3 结果显示** - 展示置信度最高的 3 个识别结果
- 🏛️ **MVVM 架构** - 使用 ViewModel 和 LiveData 进行数据管理
- 🎯 **5 种花卉识别** - 支持识别玫瑰、雏菊、向日葵、郁金香、蒲公英

---

## 🌸 支持识别的花卉

| 花卉 | 英文 |
|------|------|
| 🌹 玫瑰 | roses |
| 🌼 雏菊 | daisies |
| 🌻 向日葵 | sunflowers |
| 🌷 郁金香 | tulips |
| 🟡 蒲公英 | dandelions |

---

## 🛠️ 技术栈

| 技术 | 版本/说明 |
|------|-----------|
| **开发环境** | Android Studio 4.1+ |
| **编程语言** | Kotlin 1.3.72 |
| **机器学习框架** | TensorFlow Lite 2.3.0 |
| **相机库** | CameraX 1.0.0-beta10 |
| **架构模式** | MVVM + LiveData |
| **最低 SDK** | API 21 (Android 5.0) |
| **目标 SDK** | API 30 (Android 11) |

---

## 📁 项目结构

```
Intelligent-image-classification-APP/
├── start/                              # 起始模块（需要完成 TODO）
│   ├── src/main/
│   │   ├── java/org/tensorflow/lite/examples/classification/
│   │   │   ├── MainActivity.kt          # 主活动（相机 + 推理逻辑）✅已完成
│   │   │   ├── ui/RecognitionAdapter.kt # RecyclerView 适配器
│   │   │   ├── util/YuvToRgbConverter.kt# YUV→RGB 图像转换
│   │   │   └── viewmodel/
│   │   │       ├── Recognition.kt            # 数据类
│   │   │       └── RecognitionViewModel.kt   # ViewModel
│   │   ├── ml/FlowerModel.tflite        # TensorFlow Lite 模型
│   │   ├── res/                         # 布局/资源文件
│   │   │   ├── layout/
│   │   │   │   ├── activity_main.xml         # 主界面布局
│   │   │   │   └── recognition_item.xml      # 识别项布局
│   │   │   ├── drawable/
│   │   │   ├── mipmap-*/                 # 图标资源
│   │   │   └── values/
│   │   │       ├── strings.xml               # 字符串资源
│   │   │       ├── colors.xml                # 颜色资源
│   │   │       └── themes.xml                # 主题配置
│   │   └── AndroidManifest.xml          # 应用清单
│   └── build.gradle                     # 模块级配置（含依赖）
│
├── finish/                             # 完整参考实现
│   └── (与 start 模块结构相同)
│
├── flower_model_training.ipynb         # 🔥 TensorFlow 模型训练 Notebook
├── 实验报告.md                          # 📋 详细中文实验报告
├── README.md                            # 本文件
├── build.gradle                         # 项目级 Gradle 配置
├── settings.gradle                      # Gradle 模块设置
├── gradle.properties                    # Gradle 属性
└── gradlew / gradlew.bat               # Gradle 包装器
```

---

## 🚀 快速开始

### 1. 环境要求

- ✅ Android Studio 4.1 或更高版本
- ✅ Android SDK (API 21+)
- ✅ Android 手机（支持 USB 调试）
- ✅ JDK 8 或更高版本

### 2. 克隆仓库

```bash
git clone https://github.com/hzx20/Intelligent-image-classification-APP.git
cd Intelligent-image-classification-APP
```

### 3. 打开项目

1. 启动 **Android Studio**
2. 选择 **Open an Existing Project**
3. 选择 `Intelligent-image-classification-APP` 目录
4. 等待 Gradle 同步完成（首次会下载依赖，需要联网）

### 4. 运行应用

1. 连接 Android 手机（开启 **开发者选项** 和 **USB 调试**）
2. 在 Android Studio 顶部工具栏的设备列表中选择你的手机
3. 点击 **Run** 按钮（绿色三角 ▶️）
4. 应用将安装并启动
5. 授予相机权限
6. 将相机对准花朵，查看实时识别结果！

---

## 💡 核心技术解析

### 📷 CameraX 相机库

CameraX 是 Jetpack 提供的现代化相机库，具有以下特点：

| 特点 | 说明 |
|------|------|
| **生命周期感知** | 自动管理相机的打开/关闭，避免资源泄漏 |
| **简化的用例 API** | Preview（预览） + ImageAnalysis（图像分析） |
| **设备兼容性好** | 一致的行为覆盖绝大多数 Android 设备 |

在 `MainActivity.kt` 中的关键代码：

```kotlin
// 1. 创建预览用例 - 在屏幕上显示相机画面
preview = Preview.Builder().build()

// 2. 创建图像分析用例 - 用于 ML 推理
imageAnalyzer = ImageAnalysis.Builder()
    .setTargetResolution(Size(224, 224))       // 目标分辨率：224x224
    .setBackpressureStrategy(
        ImageAnalysis.STRATEGY_KEEP_ONLY_LATEST // 只保留最新帧
    )
    .build()
    .also {
        // 设置分析器：每一帧都调用 ImageAnalyzer.analyze()
        it.setAnalyzer(cameraExecutor, ImageAnalyzer(this) { items ->
            recogViewModel.updateData(items)
        })
    }

// 3. 绑定到 Activity 生命周期
camera = cameraProvider.bindToLifecycle(
    this, cameraSelector, preview, imageAnalyzer
)
```

### 🧠 TensorFlow Lite 与 ML Model Binding

**ML Model Binding** 是 Android Studio 4.1+ 的特性：
- 将 `.tflite` 文件放入 `ml/` 目录
- Android Studio **自动生成** Kotlin 绑定类 `FlowerModel`
- 无需手动编写底层推理代码

使用示例：

```kotlin
// 1. 初始化模型（支持 GPU 加速）
private val flowerModel: FlowerModel by lazy {
    val compatList = CompatibilityList()
    val options = if(compatList.isDelegateSupportedOnThisDevice) {
        Model.Options.Builder().setDevice(Model.Device.GPU).build()
    } else {
        Model.Options.Builder().setNumThreads(4).build()
    }
    FlowerModel.newInstance(ctx, options)
}

// 2. 运行推理
val tfImage = TensorImage.fromBitmap(bitmap)
val outputs = flowerModel.process(tfImage)
    .probabilityAsCategoryList.apply {
        sortByDescending { it.score }  // 按置信度排序
    }.take(MAX_RESULT_DISPLAY)          // 取 Top 3

// 3. 获取结果
for (output in outputs) {
    items.add(Recognition(output.label, output.score))
}
```

### 🏛️ MVVM 架构与 ViewModel

项目采用 **Model-View-ViewModel (MVVM)** 架构：

| 层 | 角色 | 文件 |
|----|------|------|
| **Model** | 机器学习模型 | `FlowerModel`（自动生成） |
| **View** | 用户界面 | `MainActivity.kt` + XML 布局 |
| **ViewModel** | 数据管理 | `RecognitionListViewModel.kt` |

**ViewModel 的优势：**
- ✅ 配置更改（屏幕旋转）时数据不丢失
- ✅ UI 与数据解耦
- ✅ 使用 `LiveData` 实现响应式更新

```kotlin
// RecognitionListViewModel.kt
class RecognitionListViewModel : ViewModel() {
    private val _recognitionList = MutableLiveData<List<Recognition>>()
    val recognitionList: LiveData<List<Recognition>> = _recognitionList

    fun updateData(recognitions: List<Recognition>) {
        _recognitionList.postValue(recognitions)
    }
}
```

### 🔄 完整的图像处理流程

```
┌─────────────────────────────────────────────────────┐
│  CameraX 摄像头                                       │
└─────────────────────────────────────────────────────┘
                         ↓
          ImageProxy (YUV_420_888 格式)
                         ↓
    YuvToRgbConverter.kt  ────>  RenderScript 加速
                         ↓
              Bitmap (ARGB_8888)
                         ↓
      TensorImage.fromBitmap() - 自动预处理
                         ↓
       FlowerModel.process() - TensorFlow Lite 推理
                         ↓
          Category 列表 (label + score)
                         ↓
    sortByDescending { it.score } - 按置信度排序
                         ↓
        take(3) - 取 Top 3 结果
                         ↓
      Recognition(label, confidence) 数据类
                         ↓
    ViewModel.postValue()  ────>  LiveData
                         ↓
     Observer.onChanged() - RecyclerView 更新 UI
                         ↓
             📱 屏幕显示识别结果
```

---

## 📝 TODO 项完成说明

`start` 模块是起始项目，需要完成以下 TODO 项（**现已全部完成**）：

| # | 位置 | 任务 | 状态 |
|---|------|------|------|
| **TODO 1** | `MainActivity.kt` | 添加 `FlowerModel` 变量，使用 `by lazy` 初始化 | ✅ 已完成 |
| **TODO 2** | `MainActivity.kt` | 将 `ImageProxy` 转换为 `Bitmap`，再转为 `TensorImage` | ✅ 已完成 |
| **TODO 3** | `MainActivity.kt` | 调用 `flowerModel.process()` 进行推理，排序并取 Top 结果 | ✅ 已完成 |
| **TODO 4** | `MainActivity.kt` | 将推理结果封装为 `Recognition` 对象列表 | ✅ 已完成 |
| **TODO 5** | `build.gradle` | 添加 GPU 委托依赖 `tensorflow-lite-gpu` | ✅ 已完成 |
| **TODO 6** | `MainActivity.kt` | 实现 GPU 加速检测，自动选择 GPU/CPU | ✅ 已完成 |

---

## 🧪 TensorFlow 模型训练

### 📓 Jupyter Notebook

项目包含完整的模型训练 Notebook：**[flower_model_training.ipynb](./flower_model_training.ipynb)**

#### Notebook 内容概览

| 步骤 | 内容 |
|------|------|
| **1. 环境准备** | 导入 TensorFlow/Keras/Numpy/Matplotlib 等库 |
| **2. 准备数据** | 下载并加载花卉数据集（5 类，共 3670 张图片） |
| **3. 构建模型** | 使用 **MobileNetV2** 预训练模型进行迁移学习 |
| **4. 编译模型** | Adam 优化器 + 稀疏交叉熵损失 |
| **5. 训练模型** | 训练 10 个 epoch |
| **6. 评估模型** | 验证准确率、混淆矩阵、分类报告 |
| **7. 保存模型** | 保存为 H5 格式和 SavedModel 格式 |
| **8. TFLite 转换** | 使用 `TFLiteConverter` 生成 `.tflite` |
| **9. 验证推理** | 测试 TFLite 模型的图像推理 |

#### 模型架构

```
输入 (224, 224, 3)
     ↓
数据增强 (随机翻转/旋转/缩放)
     ↓
MobileNetV2 预训练模型 (冻结权重)
     ↓
GlobalAveragePooling2D
     ↓
Dropout(0.2) - 防止过拟合
     ↓
Dense(5, softmax) - 5 类分类输出
```

#### 运行 Notebook

**方式一：本地运行**
```bash
pip install tensorflow matplotlib numpy pillow scikit-learn seaborn
jupyter notebook flower_model_training.ipynb
```

**方式二：Google Colab（推荐，免费 GPU）**
1. 访问 [colab.research.google.com](https://colab.research.google.com)
2. 上传 `flower_model_training.ipynb`
3. **Runtime → Change runtime type → GPU** （启用 GPU 加速）
4. 按顺序执行所有单元格

训练完成后，将生成的 `FlowerModel.tflite` 复制到 Android 项目的 `start/src/main/ml/` 目录即可使用自定义模型。

---

## 📖 详细实验报告

请查看 **[实验报告.md](./实验报告.md)** 获取完整的实验报告，包括：

- 📋 实验目的与原理
- 🔬 实验环境与工具
- 📝 实验步骤详解（Android 部分）
- 🧪 实验步骤详解（TensorFlow 训练部分）
- 📊 实验结果与分析
- 💡 实验心得与总结
- 🔍 常见问题排查

---

## ❓ 常见问题 FAQ

### Q1: Gradle 同步失败怎么办？

**A:** 检查以下几点：
- Android Studio 版本 ≥ 4.1
- 网络连接正常（需下载依赖）
- 清理缓存：`File → Invalidate Caches / Restart`

### Q2: 相机预览正常但没有识别结果？

**A:** 排查步骤：
1. 确认已授予相机权限
2. 检查 `ml/FlowerModel.tflite` 文件是否存在
3. 查看 Logcat 日志（过滤 `TFL Classify`）

### Q3: GPU 加速不起作用？

**A:** 代码已自动检测 GPU 兼容性：
- 支持 GPU → 使用 GPU 加速
- 不支持 → 自动降级到 4 线程 CPU
- 查看 Logcat 日志会输出 `GPU Compatible` 或 `GPU Incompatible`

### Q4: 如何训练自己的模型？

**A:** 
1. 准备图像数据集（按类别组织在不同子文件夹）
2. 修改 `flower_model_training.ipynb` 中的数据路径
3. 运行 Notebook 进行训练
4. 将生成的 `.tflite` 替换 `start/src/main/ml/` 中的模型

### Q5: 如何增加识别的花卉类别？

**A:** 
1. 收集新类别的训练图片
2. 在 Notebook 中重新训练模型（修改 `num_classes`）
3. 替换 Android 项目中的 `.tflite` 文件
4. 如果模型元数据包含标签，无需更改代码

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

## 📜 许可证

```
Copyright (C) 2020 The Android Open Source Project

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

---

## 📚 参考资料

| 链接 | 说明 |
|------|------|
| [TensorFlow Lite 官方文档](https://www.tensorflow.org/lite) | TensorFlow Lite 开发指南 |
| [CameraX 官方文档](https://developer.android.com/training/camerax) | Android 相机开发 |
| [TensorFlow Hub](https://tfhub.dev/) | 预训练模型库 |
| [原项目 TFLClassify](https://github.com/hoitab/TFLClassify) | 原始开源项目 |
| [教程参考 - CSDN](https://blog.csdn.net/llfjfz/article/details/123899673) | 中文教程 |

---

**如果这个项目对你有帮助，别忘了给个 ⭐ Star 哦！**
