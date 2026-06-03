# 基于 TensorFlow Lite 的 Android 花卉识别应用

## 项目简介

这是一个基于 TensorFlow Lite 的实时花卉识别 Android 应用，使用 CameraX 进行相机预览和图像分析，支持 GPU 加速。

## 功能特性

- 🌟 实时花卉识别
- 📷 使用 CameraX 相机库
- 🚀 支持 GPU 加速
- 📊 显示 Top 3 识别结果及置信度
- 🎨 使用 ViewModel 进行数据管理

## 支持识别的花卉

- 玫瑰 (roses)
- 雏菊 (daisies)
- 向日葵 (sunflowers)
- 郁金香 (tulips)
- 蒲公英 (dandelions)

## 技术栈

| 技术 | 版本 |
|------|------|
| Android Studio | 4.1+ |
| Kotlin | - |
| TensorFlow Lite | 2.3.0 |
| CameraX | 1.0.0-beta10 |
| 最低 SDK | API 21 |
| 目标 SDK | API 30 |

## 项目结构

```
TFLClassify/
├── start/                    # 起始项目（需要完成 TODO）
├── finish/                   # 完整项目
├── 实验报告.md               # 详细实验报告
└── README.md                 # 本文件
```

## 快速开始

### 1. 环境要求

- Android Studio 4.1 或更高版本
- Android 手机（支持 USB 调试）
- JDK 8 或更高版本

### 2. 运行步骤

1. 克隆仓库
   ```bash
   git clone https://github.com/hzx20/-APP.git
   ```

2. 用 Android Studio 打开项目

3. 等待 Gradle 同步完成

4. 连接 Android 手机并开启 USB 调试

5. 选择 `start` 或 `finish` 模块

6. 点击运行按钮

## 核心技术

### CameraX 相机库

CameraX 是 AndroidX 提供的现代化相机库，具有以下特点：
- 生命周期感知
- 简化的用例 API
- 设备兼容性好

### TensorFlow Lite

- 使用 ML Model Binding 自动生成模型绑定类
- 支持 GPU 加速提高推理速度
- 轻量级，适合移动端部署

### ViewModel

- 生命周期感知的数据管理
- 配置更改时保留数据
- 实现 UI 与数据的分离

## 实验报告

详细的实验报告请查看 [实验报告.md](./实验报告.md)

## 许可证

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

## 参考资料

- [TensorFlow Lite 官方文档](https://www.tensorflow.org/lite)
- [CameraX 官方文档](https://developer.android.com/training/camerax)
- [原项目地址](https://github.com/hoitab/TFLClassify)
- [教程参考](https://blog.csdn.net/llfjfz/article/details/123899673)
