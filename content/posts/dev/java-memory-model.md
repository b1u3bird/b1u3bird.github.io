+++
title = "Java 内存模型里的可见性边界"
date = 2026-06-01T09:00:00+08:00
tags = ["java", "concurrent"]
series = ["jvm notes"]
+++

这是一篇用于展示 Hugo 生成效果的示例文章。页面保留技术博客的阅读密度：标题用色带区分层级，正文留白克制，代码块使用暗色语法区域，目录固定在左侧。

# 背景

当多个线程读写同一份状态时，真正需要关注的不只是变量本身，还有写入何时对另一个线程可见。这个问题通常落在编译器优化、CPU 缓存和语言内存模型之间。

## 模型

一个实用的判断方式是先找到同步边界：锁释放到锁获取、volatile 写到 volatile 读、线程启动和线程结束等。边界之外的数据传播不能靠直觉保证。

> 技术博客的排版重点不是装饰感，而是让长标题、代码、引用和表格在同一页里仍然容易扫描。

## 代码

```java
class Counter {
    private volatile int value;

    int get() {
        return value;
    }

    void set(int next) {
        value = next;
    }
}
```

## 小结

这个 Hugo 版本会把 `content/posts` 下的 Markdown 自动生成到首页归档表格里，后续只需要继续添加文章即可。
