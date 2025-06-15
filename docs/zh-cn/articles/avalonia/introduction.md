# 介绍

> [!CAUTION]
> 在项目尚未准备适用于生产环境之前，可能会包含不稳定的 API 和功能，包括破坏性变更，请谨慎使用。

## 环境准备

> [!NOTE]
> 以下方法适用于Windows，我们尚未在 Linux/macOS 测试如下工作流

* [Zig](https://ziglang.org/) 为 [OpenHarmony-NET.PublishAotCross](https://github.com/OpenHarmony-NET/PublishAotCross?tab=readme-ov-file#openharmony-netpublishaotcross) 提供 `linker/sysroot` 支持来允许在 Windows 上交叉编译 `linux-x64/linux-arm64/linux-musl-x64/linux-musl-arm64` 目标

```shell
# 通过 winget 在 Windows 上安装 Zig
winget install zig.zig
```

> [!WARNING]
> 不建议将 `clang` 添加到环境变量中，会与构建过程冲突

* [LLVM](https://releases.llvm.org/download.html) 编译过程中需要使用 `llvm-objcopy.exe`，需要保证环境变量中存在

* .NET 9.0 SDK 或更高版本

* DevEco Studio 最新版本

* _可选_ `Visual Studio 2022`

* _可选_ `JetBrains Rider`

## 构建运行

* 拉取 [OpenHarmony.Avalonia](https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia) 的最新代码，请确保使用 `--recursive` 参数来克隆子模块

```shell
git clone https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia.git --recursive # 包括子模块
```

* 进入项目目录`OpenHarmony.Avalonia`

```shell
|-- Directory.Build.props
|-- OHOS_Project
|-- OpenHarmony.Avalonia.sln
|-- README.md
|-- Src
|-- ThirdParty

3 directories, 3 files
```

然后执行以下命令 或者 使用 `Visual Studio 2022` 打开 `OpenHarmony.Avalonia.sln` 解决方案，右键点击`Entry`项目发布

> [!NOTE]
> `arm64-v8a` 适用于真机，`x86_64` 适用于模拟器，请根据你的设备类型选择进行构建

### [真机](#tab/physical)

```shell
dotnet publish  ./Src/Entry/Entry.csproj -c Release -r linux-musl-arm64 -p:PublishAot=true  -o OHOS_Project/entry/libs/arm64-v8a
```

或者在发布页面中选择`PublishArm64.pubxml`，然后点击发布按钮

### [模拟器](#tab/virtual)

```shell
dotnet publish  ./Src/Entry/Entry.csproj -c Release -r linux-musl-x64 -p:PublishAot=true  -o OHOS_Project/entry/libs/x86_64
```

或者在发布页面中选择`PublishAmd64.pubxml`，然后点击发布按钮

---

* 打开 DevEco Studio，打开`OHOS_Project`目录，然后点击运行按钮，DevEco Studio 会自动安装应用并运行到真机/虚拟机上

> [!NOTE]
> 在真机上运行可能需要签名等流程，与正常的鸿蒙软件开发一致

## 运行你自己的项目

* 在`Entry`中添加项目引用（也就是Entry项目引用你的项目）

[并替换这个命名空间为你自己的软件的命名空间](https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia/blob/2f0af9d19832c48a69e972eb263caf4a68f381c6/Src/Entry/XComponentEntry.cs#L5)

```diff
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;
using Avalonia.OpenHarmony;
using OpenHarmony.NDK.Bindings.Native;
- using AOOH_Gallery;
+ using YourProjectNamespace;

namespace Entry;
```

[也就是更改`App`类的所属项目](https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia/blob/2f0af9d19832c48a69e972eb263caf4a68f381c6/Src/Entry/XComponentEntry.cs#L21)

```diff
        try
        {
            Ace.OH_NativeXComponent_RegisterOnFrameCallback(component, &OnSurfaceRendered);
            if (XComponents.TryGetValue((nint)component, out var xComponent))
                return;
+            xComponent = new AvaloniaXComponent<App>((nint)component, (nint)window);
            XComponents.Add((nint)component, xComponent);
            xComponent.OnSurfaceCreated();
        }
```
