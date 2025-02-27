---
title: qt-test
date: 2025-01-16 10:50:25
categories:
  - qt
tags:
---

#### ui测试

```cpp


#define qUI(statement) setupUI(__FUNCTION__, []() statement)
    template <typename Func>
    void setupUI(const QString& funcName, Func func)
    {
        QStringList args = QCoreApplication::instance()->arguments();
        if (!args.contains(funcName)) {
            QSKIP("自动测试时，跳过该UI测试用例");
        }
        QWidget* widget = func();
        widget->setAttribute(Qt::WA_DeleteOnClose);

        QEventLoop loop;
        connect(widget, &QWidget::destroyed, &loop, &QEventLoop::quit);
        loop.exec();
}
```

