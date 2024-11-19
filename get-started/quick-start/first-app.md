## 熟悉 DevEco-Studio

DevEco-Studio 它的全称是 Developer Eco Studio，Eco 是 Ecosystem 生态的简写。

DevEco-Studio 创建的项目类型有 Application 和 Atomic Service 两种。Application 是最原始的 App 形式，需要用户主动安装，而 Atomic Service 的主体是服务，无需进行安装，通过 HarmonyOS 系统的 OneHop（手机碰一碰）或者扫码调用。

## 创建一个 Empty Application Project

创建的时候需要填写：

- Project name
  项目名，应用名称默认和项目名相同
- Bundle name
  应用的唯一标识符，通常采用反域名的形式，例如 com.boen.beagle，其中第一级为域名后缀 com，第二级为厂商或者个人名，这里就是 boen，
  第三极为应用名 beagle。bundle 的中文意思是捆绑，应用最终会将各类资源打包到一起，形成一个 bundle 供应用市场进行分发。因此这里的 bundle 可以理解为 App 在应用市场上的表现形式。
- Save Location 项目存储地址
- Compatible SDK
  最低兼容的 SDK 版本
- Module name
  主包名称，鸿蒙项目采用模块化的结构设计，可以划分不同的功能模块，一个应用只有一个主包。
- Device type
  用户标识当前 Module，这里是主包，可以运行在哪些设备类型上，目前的类型有 phone,tablet 平板，2in1 设备（电脑的屏幕和键盘可拆卸触摸），tv 智慧屏，wearable 智能手表，car 车机。

## 熟悉 Application Project 的目录结构

创建好项目之后，让我们熟悉一下项目结构。

项目结构属于开发态，我们从开发的整体流程来理解会清晰很多。开发流程可以分为：源码，配置，测试，编译构建。

源码代表应用的核心逻辑，根据功能划分到不同的模块，鸿蒙应用中有一个入口模块（默认为 entry）和多个 feature 功能模块：

- entry 入口模块

  - src 源码
    - main
      - ets：ets 源码目录
        - entryability：模块的入口
        - entrybackupability：备份恢复能力
        - pages：页面
      - resources：该模块的资源文件目录
        - base：没有限定词匹配时使用的资源文件
          - element：元素资源，常用于表示在应用前端显示的元素信息，例如文本，颜色，按文本类型划分
            - string.json
            - boolean.json
            - color.json
          - media：非文本格式的媒体资源文件，例如图标，音频，视频
          - profile：自定义配置文件
        - dark：限定词目录，在暗黑模式下使用的资源文件
        - en_US：限定词目录，表示语言环境为英文，且地区为 US 时使用的资源文件
        - rawfile：原始文件目录，不会被编译，直接打包进应用。
        - resfile：原始文件目录，不经过编译直接打包进应用，在应用安装后，会被解压到应用沙箱路径。
        - zh_CN：限定词目录，语言为中文，地区为中国时使用的资源文件
      - module.json5：模块配置文件，例如模块名，入口文件位置
    - mock：测试时用到的 mock 能力
    - ohosTest：Instrument Test，在模拟器或设备上运行的测试用例
    - test：Local Test 在本地虚拟机执行的测试
  - build-profile.json5 模块编译配置
  - hvigorfile.ts 模块编译脚本
  - oh-package.json5 模块依赖配置
  - ohfuscation-rules.txt 代码混淆配置

- featureA
  其他模块的结构和 entry 模块类似

配置可以分为：

- 应用元数据

  - `AppScope`：存放应用的元数据，例如图标，名称，版本，描述

- 编译构建配置

  构建流程包括编译，测试，打包等一系列任务，为了将这个流程自动化，鸿蒙为开发者提供了编译构建工具`DevEco Hvigor`（简称`Hvigor`）,`Hvigor`是`HarmonyOS vigor`的缩写。

  - `hvigor`目录用于存放`hvigor`工具的使用配置，例如是否开启日志，是否以后台任务的形式运行。
  - `build-profile.json5`其中定义了具体的构建配置，例如针对不同设备平台的构建任务。
  - `hvigorfile.ts`是 hvigor 最终的运行脚本，定义了整体构建的逻辑流程。

- 开发配置
  - `oh-package.json5` 定义了该项目的依赖关系，例如使用哪些三方包。
  - `oh-package-lock.json5` 锁定依赖关系和版本，保证每次构建都使用相同的依赖关系和版本。
  - `code-linter.json5` linter 是静态代码分析工具，该文件为静态代码分析配置，例如扫描或者互联哪些文件，使用哪套规则。

其他目录或文件：

- `.hvigor`：`hvigor`工具所使用到的缓存数据
- `build`：构建后最终输出的目录
- `oh_modules`：存放项目依赖的三方库
- `Extenal Libraries`：在开发过程中使用到的外部库
