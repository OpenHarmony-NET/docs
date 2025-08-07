# Вступление

> [!CAUTION]
> До того, как проект будет иметь стабильную версию, он может содержать нестабильные приложения и функции, включая критические изменения. Пожалуйста, используйте его с осторожностью.

## Настройка окружения

> [!NOTE]
> Последующие действия были протестированы только на Windows. Это не было протестировано на Linux/macOS.

* [Zig](https://ziglang.org/) предоставляет `linker/sysroot` поддержку для [OpenHarmony-NET.PublishAotCross](https://github.com/OpenHarmony-NET/PublishAotCross?tab=readme-ov-file#openharmony-netpublishaotcross), чтобы разрешить кросс-компиляцию для `linux-x64/linux-arm64/linux-musl-x64/linux-musl-arm64` в Windows.

```shell
# Установка Zig в Windows с помощью winget
winget install zig.zig
```

> [!WARNING]
> Не рекомендуется добавлять `clang` к переменным среды, так как это может привести к конфликту с процессом сборки.

* [LLVM](https://releases.llvm.org/download.html) необходим для процесса компиляции, в котором используется `llvm-objcopy.exe`. Убедитесь, что он существует в переменных вашего окружения

* .NET 9.0 SDK или выше

* Последняя версия DevEco Studio

* _Необязательно_ `Visual Studio 2022`

* _Необязательно_ `JetBrains Rider`

## Сборка и запуск

* Склонируйте [OpenHarmony.Avalonia](https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia). Убедитесь, что для клонирования используется параметр `--recursive`, чтобы включить подмодули.

```shell
git clone https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia.git --recursive # Включение подмодулей
```

* Откройте каталог проекта `OpenHarmony.Avalonia`

```shell
|-- Directory.Build.props
|-- OHOS_Project
|-- OpenHarmony.Avalonia.sln
|-- README.md
|-- Src
|-- ThirdParty

3 directories, 3 files
```

Затем выполните следующую команду или используйте `Visual Studio 2022`, чтобы открыть `OpenHarmony.Решение Avalonia.sln`, щелкните правой кнопкой мыши на проекте `Entry` и опубликуйте.

> [!NOTE]
> `arm64-v8a` подходит для физических устройств, в то время как `x86_64` предназначен для эмуляторов. Пожалуйста, выбирайте в соответствии с типом вашего устройства.

### [Физическое устройство](#tab/physical)

```shell
dotnet publish  ./Src/Entry/Entry.csproj -c Release -r linux-musl-arm64 -p:PublishAot=true  -o OHOS_Project/entry/libs/arm64-v8a
```

Или выберите `PublishArm64.pubxml` на странице публикации, затем нажмите кнопку опубликовать.

### [Эмулятор](#tab/virtual)

```shell
dotnet publish  ./Src/Entry/Entry.csproj -c Release -r linux-musl-x64 -p:PublishAot=true  -o OHOS_Project/entry/libs/x86_64
```

Или выберите `PublishAmd64.pubxml` на странице публикации, затем нажмите кнопку опубликовать.

---

* Откройте DevEco Studio, откройте каталог `OHOS_Project`, затем нажмите кнопку запустить. DevEco Studio автоматически установит и запустит приложение на вашем физическом устройстве/эмуляторе.

> [!NOTE]
> Запуск на физическом устройстве может потребовать подписания и других процедур, соответствующих обычной разработке программного обеспечения HarmonyOS.

## Запуск вашего проекта

* Добавьте свой проект в зависимости `Entry` проекта (т.е `Entry` проект должен ссылаться на ваш проект)

[И замените это пространство имен пространством имен вашего проекта](https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia/blob/2f0af9d19832c48a69e972eb263caf4a68f381c6/Src/Entry/XComponentEntry.cs#L5)

```diff
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;
using Avalonia.OpenHarmony;
using OpenHarmony.NDK.Bindings.Native;
- using AOOH_Gallery;
+ using YourProjectNamespace;

namespace Entry;
```

[Также измените принадлежность класса `App` к проекту](https://github.com/OpenHarmony-NET/OpenHarmony.Avalonia/blob/2f0af9d19832c48a69e972eb263caf4a68f381c6/Src/Entry/XComponentEntry.cs#L21)

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