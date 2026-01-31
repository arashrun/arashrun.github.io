---
title: qt多语言翻译
date: 2026-01-20 15:14:48
categories:
  - qt
tags:
  - i18
---

在 Qt 中实现多语言翻译（国际化，简称 **i18n**）主要遵循一个标准流程：**代码标记 -> 提取文件 -> 翻译文本 -> 发布并加载**。

以下是详细的操作指南：

---

## 1. 在代码中标记待翻译文本

为了让 Qt 识别哪些字符串需要翻译，必须使用 `tr()` 函数包裹字符串。

* **C++ 中：** 使用 `tr("Hello")`
* **QML 中：** 使用 `qsTr("Hello")`
* **UI 文件中：** 在 Qt Designer 里输入的文本默认会被标记。

```cpp
// 示例
label->setText(tr("Welcome to my Application"));

```

---

## 2. 生成翻译源文件 (.ts)

你需要告诉 Qt 构建系统要生成哪些语言的文件。

### 如果使用 CMake (推荐)

在 `CMakeLists.txt` 中添加以下指令：

```cmake
find_package(Qt6 REQUIRED COMPONENTS LinguistTools)

# 定义要生成的语言文件
set(TS_FILES
    languages/app_zh_CN.ts
    languages/app_en_US.ts
)

# 自动处理翻译文件
qt_add_translations(${PROJECT_NAME} TS_FILES ${TS_FILES})

```

### 如果使用 qmake (.pro)

在 `.pro` 文件中添加：

```pro
TRANSLATIONS += languages/app_zh_CN.ts \
                languages/app_en_US.ts

```

然后运行菜单栏的：**工具 -> 外部 -> Qt 语言家 -> Update Translations (lupdate)**。这会扫描代码并生成 `.ts` 文件。

---

## 3. 使用 Qt Linguist (语言家) 进行翻译

`.ts` 文件是 XML 格式，手动编辑很麻烦，建议使用 Qt 自带的工具。

1. 找到并打开 **Qt Linguist** 程序。
2. 打开生成的 `app_zh_CN.ts`。
3. 在界面中输入对应的翻译文本，并点击确认（打勾）。
4. 保存文件。

---

## 4. 编译翻译文件 (.qm)

Qt 程序运行速度很快，它不直接读取 `.ts` 文件，而是读取二进制的 `.qm` 文件。

* **方法：** 在 Qt Creator 中点击 **工具 -> 外部 -> Qt 语言家 -> Release Translations (lrelease)**。
* 这会在目录下生成同名的 `.qm` 文件。

---

## 5. 在代码中加载翻译文件

在 `main.cpp` 中，你需要在创建窗口之前安装翻译器。

```cpp
#include <QApplication>
#include <QTranslator>
#include <QLocale>

int main(int argc, char *argv[]) {
    QApplication a(argc, argv);

    QTranslator translator;
    // 尝试根据系统语言自动加载
    if (translator.load(QLocale(), "app", "_", ":/languages")) {
        a.installTranslator(&translator);
    }

    MainWindow w;
    w.show();
    return a.exec();
}

```

---

## 6. 进阶：动态切换语言

如果你希望在程序运行中切换语言（不重启程序）：

1. **重新加载：** 卸载旧的 `QTranslator`，加载新的。
2. **刷新界面：**
* **C++：** 必须重写 `changeEvent` 事件，并在 `Event::LanguageChange` 发生时重新调用 `ui->retranslateUi(this)`。
* **QML：** 需要配合一个 `EmptyString` 的信号或者特定的语言管理类。



```cpp
void MainWindow::changeEvent(QEvent *event) {
    if (event->type() == QEvent::LanguageChange) {
        ui->retranslateUi(this); // 重新刷新界面上的所有 tr() 文本
    }
    QMainWindow::changeEvent(event);
}

```

---

### 常见注意事项

* **占位符：** 翻译包含变量的字符串，请使用 `%1`, `%2`。
* `tr("File %1 of %2").arg(current).arg(total)`


* **单复数：** `tr()` 支持第三个参数用于处理单复数。
* **上下文：** 如果同一个词在不同地方意思不同，可以在 `tr()` 的第二个参数加注释：`tr("Open", "File menu")`。

**你想让我为你提供一个完整的动态切换语言的“语言管理器”类代码示例吗？**



## 指定语言

如果你不想根据系统语言（`QLocale()`）自动切换，而是想**强制指定**某种语言（例如强制显示中文或英文），你需要手动构造 `QLocale` 或者直接指定文件名。

以下是三种常见的指定方式：

---

### 1. 手动构造 QLocale 指定（推荐）

这是最规范的方法，通过传递语言和地区枚举值来加载对应的 `.qm` 文件。

```cpp
QTranslator translator;
// 强制指定为中文 (中国)
QLocale locale(QLocale::Chinese, QLocale::China); 

// load 会根据 locale 自动寻找名为 "app_zh_CN.qm" 的文件
if (translator.load(locale, "app", "_", ":/languages")) {
    a.installTranslator(&translator);
}

```

### 2. 直接根据文件名加载

如果你知道翻译文件的确切路径和名称，可以直接加载。

```cpp
QTranslator translator;
// 直接加载 :/languages/app_zh_CN.qm
if (translator.load(":/languages/app_zh_CN.qm")) {
    a.installTranslator(&translator);
}

```

### 3. 在运行过程中动态切换

如果你想在程序运行代码中，点击一个按钮就切换语言，需要注意**对象生命周期**和**界面刷新**。

**步骤 A：封装一个切换函数**
由于 `QTranslator` 必须在程序运行期间一直存在，建议将其定义为 `main` 函数中的局部变量或全局单例。

```cpp
// 在 MainWindow 某个槽函数中切换
void MainWindow::on_btnChinese_clicked() {
    static QTranslator* translator = new QTranslator(qApp); // 静态变量防止销毁
    
    if (translator->load(":/languages/app_zh_CN.qm")) {
        qApp->installTranslator(translator);
    }
}

```

**步骤 B：重写 `changeEvent` 刷新 UI**
这是新手最容易忽略的一步。`installTranslator` 只是更新了底层的翻译词库，**已经显示在界面上的文字不会自动改变**。你必须手动触发刷新：

```cpp
// 在 MainWindow.cpp 中添加
void MainWindow::changeEvent(QEvent *event) {
    if (event->type() == QEvent::LanguageChange) {
        // retranslateUi 是 Qt 设计师自动生成的函数
        // 它会把所有 ui->label->setText(tr(...)) 重新执行一遍
        ui->retranslateUi(this); 
    }
    QMainWindow::changeEvent(event);
}

```

---

### 常见 QLocale 构造速查表

| 目标语言 | 构造代码 | 预期文件名示例 |
| --- | --- | --- |
| **简体中文** | `QLocale(QLocale::Chinese, QLocale::China)` | `app_zh_CN.qm` |
| **美式英文** | `QLocale(QLocale::English, QLocale::UnitedStates)` | `app_en_US.qm` |
| **日文** | `QLocale(QLocale::Japanese, QLocale::Japan)` | `app_ja_JP.qm` |

---

### 关键排查点

* **资源文件路径**：确保你的 `.qm` 文件已经添加到 `.qrc` 资源文件中，并且前缀（Prefix）确实是 `:/languages`。
* **文件加载顺序**：`installTranslator` 必须在 UI 界面实例化（即 `new MainWindow`）**之前**调用，否则首屏文字不会被翻译。

**你想让我演示一下如何通过一个 QComboBox 下拉框实现多国语言的一键实时切换吗？**