# Introduction

> [!CAUTION]
> Before the project is ready for production, it may contain unstable APIs and features, including breaking changes. Please use with caution.

## Environment Setup

> [!NOTE]
> The following methods apply to Windows. We have not yet tested this workflow on Linux/macOS.

* [Zig](https://ziglang.org/) provides `linker/sysroot` support for [OpenHarmony-NET.PublishAotCross](https://github.com/OpenHarmony-NET/PublishAotCross?tab=readme-ov-file#openharmony-netpublishaotcross) to allow cross-compilation for `linux-x64/linux-arm64/linux-musl-x64/linux-musl-arm64` targets on Windows

```shell
# Install Zig on Windows via winget
winget install zig.zig
```

> [!WARNING]
> It is not recommended to add `clang` to the environment variables, as it may conflict with the build process.

* [LLVM](https://releases.llvm.org/download.html) is needed for the compilation process that uses `llvm-objcopy.exe`, ensure it exists in your environment variables

* .NET 9.0 SDK or greater

* Latest version of DevEco Studio

* _Optional_ `Visual Studio 2022`

* _Optional_ `JetBrains Rider`

## Build and Run

* Pull the latest code from [OpenHarmony.Avalonia](https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia), ensure to clone with the `--recursive` option to include submodules

```shell
git clone https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia.git --recursive # Include submodules
```

* Enter the project directory `OpenHarmony.Avalonia`

```shell
|-- Directory.Build.props
|-- OHOS_Project
|-- OpenHarmony.Avalonia.sln
|-- README.md
|-- Src
|-- ThirdParty

3 directories, 3 files
```

Then execute the following command or use `Visual Studio 2022` to open the `OpenHarmony.Avalonia.sln` solution, right-click on the `Entry` project and publish

> [!NOTE]
> `arm64-v8a` is suitable for physical devices, while `x86_64` is for emulators. Please choose according to your device type.

### [Physical Device](#tab/physical)

```shell
dotnet publish  ./Src/Entry/Entry.csproj -c Release -r linux-musl-arm64 -p:PublishAot=true  -o OHOS_Project/entry/libs/arm64-v8a
```

Or select `PublishArm64.pubxml` in the publish page, then click the publish button

### [Emulator](#tab/virtual)

```shell
dotnet publish  ./Src/Entry/Entry.csproj -c Release -r linux-musl-x64 -p:PublishAot=true  -o OHOS_Project/entry/libs/x86_64
```

Or select `PublishAmd64.pubxml` in the publish page, then click the publish button

---

* Open DevEco Studio, open the `OHOS_Project` directory, then click the run button. DevEco Studio will automatically install and run the application on your physical device/emulator

> [!NOTE]
> Running on a physical device may require signing and other procedures, consistent with normal HarmonyOS software development

## Run Your Own Project

* Add your project reference to `Entry` (i.e., the Entry project references your project)

[And replace this namespace with your own software's namespace](https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia/blob/2f0af9d19832c48a69e972eb263caf4a68f381c6/Src/Entry/XComponentEntry.cs#L5)

```diff
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;
using Avalonia.OpenHarmony;
using OpenHarmony.NDK.Bindings.Native;
- using AOOH_Gallery;
+ using YourProjectNamespace;

namespace Entry;
```

[Also change the `App` class's project affiliation](https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia/blob/2f0af9d19832c48a69e972eb263caf4a68f381c6/Src/Entry/XComponentEntry.cs#L21)

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
