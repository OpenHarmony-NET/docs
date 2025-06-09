![OpenHarmony.NET](../_images/Header.png "Hello OpenHarmony.NET")

# Introduction

## 🤔What is OpenHarmony.NET?

OpenHarmony.NET is a solution designed specifically for **OpenHarmony** (including **HarmonyOS Next**), aimed at supporting **.NET** applications on the Harmony operating system. With OpenHarmony.NET, developers can use familiar **Avalonia** or **Blazor Hybrid** technologies to develop Harmony applications, and even use **C#** instead of **C++** for native library development. This provides .NET developers with a new platform, enabling them to easily introduce the .NET technology stack into the Harmony ecosystem.

## 😲Runtime Support

### · Adaptation Status
OpenHarmony.NET has been successfully adapted to **.NET 9**, providing developers with a stable and efficient runtime environment.

### · Runtime Limitations
1. **Only NativeAOT Available**:
    Due to Harmony system restrictions on runtime-generated assembly code (see [Harmony System Change Notes](https://developer.huawei.com/consumer/cn/doc/harmonyos-releases-V5/changelogs-for-all-apps-b031-V5#%E5%8C%BF%E5%90%8D%E5%86%85%E5%AD%98%E6%89%A7%E8%A1%8C%E6%9D%83%E9%99%90%E7%AE%A1%E6%8E%A7%E7%AD%96%E7%95%A5%E5%8F%98%E6%9B%B4%E8%AF%B4%E6%98%8E)), JIT (Just-In-Time) technology cannot be used. Therefore, OpenHarmony.NET adopts **NativeAOT (Native Ahead-Of-Time)** compilation. This method generates native machine code directly during compilation, ensuring efficient application execution on the Harmony system.

2. **Cannot Use `Marshal.GetDelegateForFunctionPointer` Related Functions**:
    For the same reason as above, direct use of function pointers is recommended.

## 🥰Framework Adaptation

### Supported Frameworks
Currently, OpenHarmony.NET has successfully adapted the following two frameworks:
1. **Avalonia**: A cross-platform UI framework that supports desktop application development using XAML and C#. For details, please refer to the [Avalonia Documentation](articles/avalonia/introduction.md).
2. **Blazor Hybrid**: A hybrid development framework based on Blazor, allowing cross-platform application development using C# and Razor syntax. For details, please refer to the [Blazor Hybrid Documentation](articles/blazor-hybrid/introduction.md).

### More Framework Adaptations
We welcome more .NET frameworks to join the OpenHarmony.NET ecosystem. If you are interested in adapting other frameworks, we are willing to share valuable experience accumulated during the adaptation of Avalonia and Blazor Hybrid to help you get started quickly.

