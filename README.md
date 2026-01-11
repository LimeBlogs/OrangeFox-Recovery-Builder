<h1 align="center">
  <img src="https://orangefox.download/fox.svg" height="60" style="vertical-align:bottom"/> 
  OrangeFox Recovery Builder
</h1>

<h3 align="center">
  <i>一个简单易用的OrangeFox构建器，使用Github Actions</i>
</h3>

## 🚢快速开始
首先Fork本仓库，你还需要设备树文件和`OrangeFoxConfig.mk`，一切准备就绪后，你可以开始构建OF。完毕后所有的产物都会在Releases中，具体步骤如下

### 📁设备树与OrangeFoxConfig.mk
你首先需要准备好设备的设备树，并且将其Fork到你的用户下，接着在项目目录下新建`OrangeFoxConfig.mk`，按照注释填写

```
# ==========================================
# 1. 基础信息 (这块随便填)
# ==========================================
OF_MAINTAINER_PATCH_VERSION := 1
OF_MAINTAINER := Lime
# 这里的 1 代表这是第一个编译版本
OF_MAINTAINER_AVATAR := /dev/null

# ==========================================
# 2. 屏幕与UI配置 (【必改】请根据你机型修改)
# ==========================================
# Redmi 12C 是 720x1650 的屏幕。如果你是别的手机，请查清楚分辨率高度
OF_SCREEN_H := 1650

# 状态栏高度：这是为了避让刘海/水滴屏出现奇奇怪怪的问题
OF_STATUS_H := 65

# 左右边距：避开屏幕圆角
OF_STATUS_INDENT_LEFT := 48
OF_STATUS_INDENT_RIGHT := 48

# 时钟位置：1=左边, 2=中间 (水滴屏建议放左边 1)
OF_CLOCK_POS := 1

# 允许在设置里禁用导航栏
OF_ALLOW_DISABLE_NAVBAR := 0

# ==========================================
# 3. 核心功能
# ==========================================
OF_USE_MAGISKBOOT := 1
OF_USE_MAGISKBOOT_FOR_ALL_PATCHES := 0
OF_NO_RELOAD_MAGISKBOOT := 1
OF_NO_TREBLE_COMPATIBILITY_CHECK := 1
OF_NO_MIUI_PATCH_WARNING := 1

# ==========================================
# 4. 【关键】Android 12+ 解密与防砖配置
# ==========================================
# 针对 Metadata 加密的等待逻辑，解决 Data 挂载慢或失败
OF_SKIP_METADATA_DECRYPTION_WAIT := 0

# 不要修改被加密的设备（防止触发 AVB 红字无法开机）
OF_DONT_PATCH_ENCRYPTED_DEVICE := 1

# 保持 DM-Verity 状态，防止修改 System 后出现傻逼提示
OF_KEEP_DM_VERITY := 1

# 强制使用 magiskboot 可以在刷入时自动修补 Boot，避免掉 Root
OF_USE_MAGISKBOOT_COMPRESSED_WEBP := 0

# ==========================================
# 5. 其他杂项
# ==========================================
# 启用很多好用的小工具
OF_ENABLE_LPTOOLS := 1
# 默认备份列表
OF_QUICK_BACKUP_LIST := /boot;/data;
```

填写完毕只需要Push即可，准备好推送过后的仓库链接

### 🚀开始构建
在顶部`Action` 标签 > `All workflows` > `OrangeFox - 构建 ` > `Run workflow`

按照要求填写信息
`OrangeFox 版本` - 保持默认12.1即可，14.1未经过实测，可能会存在许多问题<br><br>
`OrangeFox设备树` - 拷贝设备树的仓库链接<br><br>
`OrangeFox设备树分支` - 设备树的分支，一般是main或者master<br><br>
`指定设备路径` - 一般为`device/${品牌}/${设备代号}`<br><br>
`指定设备代号` - 设备代号，通过搜索引擎或者`fastboot getvar product`查询<br><br>
`构建目标` - 构建的镜像，`boot`/`recovery`，前者适用于无独立REC分区的设备，后者反之<br><br>
*对于无独立Rec的设备强行构建Rec后只会生成OrangeFox 的卡刷包 (.zip)，这需要你自备其他Rec并进行固化，如若无第三方Rec，可以构建Boot后于OrangeFox内进行固化操作*
<br><br>`设备名称` - 设备名称，随意填写，主要是存在发行版的版本信息内（如：iPhone6s Plus/Redmi K20 Pro等），无硬性要求<br><br>

点击底部绿色`Run workflow`开始构建，整个过程时间较长，耐心等待<br><br>
完毕后你可以在仓库的`Releases`中查看并下载镜像

### 📱刷入镜像
使用fastboot(无独立REC分区设备)

```
fastboot falsh boot boot.img
```
对于Rec固化你需要自备原厂Boot，即先刷入一遍原厂再使用OF固化即可

<br>

使用fastboot(有独立REC分区设备)
```
fastboot falsh boot boot.img
```

## 🌸致谢
该项目基于[**OrangeFox-Action-Builder**](OrangeFox-Action-Builder)修改并进行优化，对原有项目添加了中文支持和部分工作流代码优化。<br>
部分代码完善由Gemini 3调整并优化。
