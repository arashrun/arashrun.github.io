---
title: qml嵌入qwidget
date: 2026-01-04 09:59:08
categories:
  - qt
tags:
---


### qwidget的局限性

qt quick中的很多特效只能在qml中使用，无法直接在标准qwidget体系中运行。

这是因为 QtQuick.Particles 模块是深度绑定在 Qt Quick Scene Graph（场景图）之上的。Qt Widgets 渲染机制（基于 QPainter 和软件/2D 绘图）与 Qt Quick 的硬件加速渲染机制完全不同。

### 嵌入QQuickWidget方案

如果你必须在现有的 Widgets 界面里加入粒子效果，可以使用 QQuickWidget。

- 做法：创建一个 QML 文件编写粒子系统，然后在 C++ 代码中将其嵌入到 QQuickWidget 容器里。

- 优点：可以混合使用 Widgets 和 QML，简单省力。

- 缺点：有一定的性能开销（因为需要处理两个渲染上下文的组合）。

在一个现有的 Qt Widgets 项目中嵌入 QML 粒子系统，最稳妥的方法就是使用 QQuickWidget。它像一个普通的 QWidget 一样可以放入布局中，但内部运行着一个完整的 Qt Quick 渲染环境。

1. 准备qml粒子文件(ParticleEffect.qml)，放入Qt资源文件中。

```qml

import QtQuick 2.15
import QtQuick.Particles 2.15

Rectangle {
    width: 400
    height: 400
    color: "black" // 建议背景透明或与 Widget 颜色一致

    ParticleSystem { id: mainSystem }

    Emitter {
        system: mainSystem
        anchors.centerIn: parent
        width: 1; height: 1
        emitRate: 20
        lifeSpan: 2000
        velocity: AngleDirection { angle: 0; angleVariation: 360; magnitude: 100 }
    }

    ImageParticle {
        system: mainSystem
        source: "qrc:/images/star.png" // 需要提供一张小星星或圆点的图片
        colorVariation: 0.8
    }
}

```

2. c++嵌入QQuickWidget

```c++
QT += widgets quickwidgets
```

```c++
#include <QQuickWidget>
#include <QVBoxLayout>

// 在你的构造函数或某个初始化函数中
void MainWindow::setupParticleEffect() {
    // 1. 创建 QQuickWidget 实例
    QQuickWidget *quickWidget = new QQuickWidget(this);

    // 2. 设置其调整模式：让 QML 自动跟随 Widget 大小缩放
    quickWidget->setResizeMode(QQuickWidget::SizeRootObjectToView);

    // 3. 设置背景透明（可选，如果你希望看到 Widget 底色）
    quickWidget->setAttribute(Qt::WA_AlwaysStackOnTop);
    quickWidget->setClearColor(Qt::transparent);

    // 4. 加载 QML 文件
    quickWidget->setSource(QUrl(QStringLiteral("qrc:/ParticleEffect.qml")));

    // 5. 将其添加到你的布局中
    // 假设你有一个名为 ui->verticalLayout 的布局
    ui->verticalLayout->addWidget(quickWidget);
}

```


### QWidget调用QML方法

实现"从C++侧点击按钮，在QML侧喷发例子"。

1. qml中增加粒子发生函数

```qml

import QtQuick 2.15
import QtQuick.Particles 2.15

Item {
    id: root
    width: 400; height: 400

    ParticleSystem { id: sys }

    // 使用 ImageParticle 定义粒子外观
    ImageParticle {
        system: sys
        source: "qrc:/images/spark.png" // 替换为你自己的粒子图片路径
        color: "gold"
        alpha: 0.5
    }

    Emitter {
        id: burstEmitter
        system: sys
        enabled: false // 初始关闭，仅由 C++ 触发
        lifeSpan: 1000
        emitRate: 1000
        size: 16
        velocity: AngleDirection {
            angleVariation: 360
            magnitude: 150
            magnitudeVariation: 50
        }
    }

    // 暴露给 C++ 调用的函数
    function triggerBurst(x, y) {
        burstEmitter.x = x;
        burstEmitter.y = y;
        burstEmitter.pulse(200); // 喷发 200 毫秒
    }
}

```

2. c++端调用

```cpp

// 假设你有一个 QPushButton 的点击槽函数
void MainWindow::on_pushButton_clicked() {
    // 1. 获取 QML 根对象
    QObject *root = ui->quickWidget->rootObject();

    if (root) {
        // 2. 调用 QML 中的 triggerBurst 函数
        // 参数依次为：对象, 函数名, 返回值(无), 参数1(x), 参数2(y)
        // 这里的 (100, 100) 是喷发位置，你可以根据需要传入坐标
        QMetaObject::invokeMethod(root, "triggerBurst",
                                 Q_ARG(QVariant, 100), 
                                 Q_ARG(QVariant, 100));
    }
}

```
