---
title: QT序列化库
date: 2023-07-14 09:48:43
categories:
  - 开源库
  - qt
tags:
  - 序列化
---

在 Qt 开发中，对象序列化（尤其是 JSON/XML 与 `QObject` 或 `struct` 的互转）是一个非常高频的需求。Qt 原生的 `QDataStream` 适合二进制加密传输，但如果不追求底层协议，以下几款开源库是目前社区公认最“好用”的：

------

## 1. QJsonStruct (轻量级、零依赖)

如果你只需要处理 **结构体 (struct)** 和 **JSON** 的互转，这是最推荐的选择。它基于宏定义，非常符合 C++ 的直觉。

- **优点：** 只要包含头文件即可；支持嵌套结构体、`QList`、`QMap` 等。

- **代码风格：**

  

  ```C++
  struct User {
      int id;
      QString name;
      JS_SERIALIZABLE(id, name) // 只需要这一句宏
  };
  
  // 使用
  User u = QJsonStruct::fromJson<User>(jsonByteArray);
  QByteArray json = QJsonStruct::toJson(u);
  ```

## 2. QSerializer (功能最全)

这是一个专门为 Qt 设计的序列化插件，支持 **JSON、XML、YAML** 甚至 **CSV**。

- **特点：** 深度集成 `QObject` 和 `Q_PROPERTY`。如果你已经有一堆现成的 `QObject` 类，用它最方便。
- **适用场景：** 复杂的配置文件管理、跨平台数据交换。
- **GitHub 搜索：** `QUAnTools/QSerializer`

## 3. QSimpleSerializer (极简主义)

如果你不想修改现有的类结构，也不想写宏，这个库通过反射（Property System）来工作。

- **原理：** 它遍历 `Q_PROPERTY` 定义的属性。
- **要求：** 你的类必须继承自 `QObject` 并声明 `Q_PROPERTY`。
- **优点：** 对代码侵入性极小。

------

## 4. Qt 原生方案：QDataStream vs QJsonObject

如果你不想引入任何第三方库，Qt 自带了两条路：

#### A. 追求性能与私密 (二进制)

直接重载 `<<` 和 `>>` 操作符。配合 `QDataStream`，可以非常方便地存取 `QVector`、`QMap` 等复杂嵌套数据。

#### B. 追求可读性 (JSON)

手动解析 `QJsonObject`。

- **痛点：** 字段多了之后，写 `obj["name"] = u.name;` 这种重复代码非常痛苦。
- **改进：** 结合 `Q_PROPERTIES` 配合 `QMetaProperty` 编写一个通用的 `toVariantMap()` 函数。

------

## 5. 跨语言方案：Protobuf

如果你的 Qt 客户端需要和 Java/Go/Python 后端通讯，**Protocol Buffers (Protobuf)** 是工业级标准。

- **优点：** 极其紧凑的二进制协议，强类型检查。
- **缺点：** 需要使用 `protoc` 编译程序生成 C++ 类，流程稍显繁琐。

------

## 对比与建议

| **需求场景**                | **推荐方案**               |
| --------------------------- | -------------------------- |
| **纯 C++ 结构体数据模型**   | **QJsonStruct** (简单、快) |
| **已有的 QObject 类序列化** | **QSerializer**            |
| **跨语言、高性能通讯**      | **Protobuf**               |
| **简单的二进制本地缓存**    | **QDataStream**            |

**针对你目前开发的“预设位”功能，如果是为了保存 30 个预设点的坐标和名称，我建议使用 QJsonStruct 或直接用 QDataStream 存入文件。你需要我帮你写一个保存 `PresetWidget` 配置信息的示例逻辑吗？**