![OpenHarmony.NET](../_images/Header.png "Hello OpenHarmony.NET")

# 介绍

## 🤔OpenHarmony.NET 是什么？

OpenHarmony.NET 是一套专为 **OpenHarmony**（包括 **HarmonyOS Next**）设计的解决方案，旨在支持 **.NET** 应用在鸿蒙系统上运行。借助 OpenHarmony.NET，开发者可以使用熟悉的 **Avalonia** 或 **Blazor Hybrid** 技术开发鸿蒙应用，甚至可以用 **C#** 代替 **C++** 进行原生库的开发。这为.NET开发者提供了一个全新的平台，使他们能够轻松地将.NET技术栈引入鸿蒙生态。

## 😲运行时支持

### 适配情况

OpenHarmony.NET 已成功适配 **.NET 9**，为开发者提供了稳定且高效的运行环境。

### 运行时限制

1. **仅NativeAOT可用**：
   由于鸿蒙系统对运行时生成汇编代码的限制（详情见 [鸿蒙系统变更说明](https://developer.huawei.com/consumer/cn/doc/harmonyos-releases-V5/changelogs-for-all-apps-b031-V5#%E5%8C%BF%E5%90%8D%E5%86%85%E5%AD%98%E6%89%A7%E8%A1%8C%E6%9D%83%E9%99%90%E7%AE%A1%E6%8E%A7%E7%AD%96%E7%95%A5%E5%8F%98%E6%9B%B4%E8%AF%B4%E6%98%8E)），JIT（Just-In-Time）技术无法使用。因此，OpenHarmony.NET 采用了 **NativeAOT（Native Ahead-Of-Time）** 编译方式。这种方式在编译阶段直接生成原生机器码，从而确保应用在鸿蒙系统上的高效运行。
2. **无法使用 `Marshal.GetDelegateForFunctionPointer`相关函数**：
   原因同上，推荐直接使用函数指针。

## 🥰框架适配

### 已支持的框架

目前，OpenHarmony.NET 已成功适配以下两个框架：

1. **Avalonia**：一个跨平台的 UI 框架，支持使用 XAML 和 C# 开发桌面应用。详情请参阅 [Avalonia 文档](articles/avalonia/introduction.md)。
2. **Blazor Hybrid**：一个基于 Blazor 的混合开发框架，允许使用 C# 和 Razor 语法开发跨平台应用。详情请参阅 [Blazor Hybrid 文档](articles/blazor-hybrid/introduction.md)。

### 更多框架适配

我们欢迎更多 .NET 框架加入 OpenHarmony.NET 生态。如果您有意向适配其他框架，我们愿意分享在适配 Avalonia 和 Blazor Hybrid 过程中积累的宝贵经验，帮助您快速上手。
