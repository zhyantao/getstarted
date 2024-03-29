# Yocto


Yocto 是**用于构建**针对嵌入式设备的**定制 Linux 发行版的**一套综合的**工具套件、模板和资源**。

学习 Yocto 应该从以下几个方面入手：

- Poky 工作流
- OpenEmbedded 构建系统（包括 BitBake 构建引擎）
- 定制操作系统栈
- 板支持包（Board Support Package，BSP）
- 应用开发工具包

本文仅介绍常用的 BitBake 语法和命令，更多请参考《嵌入式 Linux 系统开发：基于 Yocto Project》。

## BitBake 文件简介

当我们运行 `bitbake <recipe>` 时，它会自动地去找 `<recipe>.bb` 这个 `.bb` 文件，`.bb` 文件包含了一系列的任务（[Tasks](https://docs.yoctoproject.org/ref-manual/tasks.html)）：configuring、compiling、packaging software。

Bitbake 的执行流程：首先将源代码拷贝一份到 `build/tmp/work/<archname>` 目录下，然后执行 `.bb` 文件中的 [`do_compile`](https://docs.yoctoproject.org/ref-manual/tasks.html#do-compile) 和 `do_install` 函数。这两个函数体中包含了一些运行脚本，这跟我们在 Shell 中直接执行命令无异，只不过这些运行脚本使用了由 `source oe-init-build-env` 声明的环境变量，可以进行交叉编译。

`.bb` 文件的作用在于，它可以帮助我们将编写好的代码或脚本添加到 Yocto 镜像中。

下面将介绍 `.bb` 文件最基本也是最常用的编写方法。

前提：假设你已经编写好了下面这几个文件，并且你打算将它们添加到 Linux 发行版中：

```bash
startup-script  # 一个在系统启动时运行的脚本（例如，用于恢复持久状态）。
run-script      # 一个用于启动设备应用程序的脚步（设备的运行级别为 5）。
support-script  # 上面两个脚本运行所需的脚本。
```

将脚本添加到 Linux 发行版中以及它们与各种 `init` 运行级别的交互是通过 BitBake 菜谱（`.bb` 文件）控制的。模板如下所示：[^ref-cite-1]

```bash
DESCRIPTON = "Startup scripts"
LICENSE = "MIT"

# 菜谱的版本：更新菜谱后，不要忘记修改这里的版本号
PR = "r0"

# 运行时依赖
#
# 添加类似于以下内容的行，以确保运行脚本所需的所有包都安装在映像中
#
# RDEPENDS_${PN} = "parted"

# SRC_URI 指定制作菜谱所需的源文件（或脚本文件）
#
# src_uri 常用的有效格式有 files、https、git 等
# 本例假设所有的源文件都存储在 files 目录下
#
SRC_URI = "              \
   file://startup-script \
   file://run-script     \
   file://support-script \
   "

# do_compile 的功能如下：
#  1) 切换到 build 目录
#  2) 运行 oe_runmake 编译源代码
#
do_compile() {
    #
    # 查找 Makefile、makefile 或 GNUmakefile
    # 若不存在上述文件，则什么都不做
    #
    make
}

# do_install 的功能如下：
#  1) 确保映像中存在所需的目录；
#  2) 将脚本安装到映像中；
#  3) 为脚本创建符合其运行级别的软连接。
#
do_install() {
    #
    # 创建目录：
    #   ${D}${sysconfdir}/init.d - 用于保存脚本
    #   ${D}${sysconfdir}/rcS.d  - 用于保存系统启动时运行的脚本的软链接
    #   ${D}${sysconfdir}/rc5.d  - 用于保存 runlevel=5 的脚本的软链接
    #   ${D}${sbindir}           - 用于保存被上面两个脚本调用的脚本
    #
    # ${D} 实际上是目标系统的根目录
    # ${D}${sysconfdir} 是存储系统配置文件的位置（例如 /etc）
    # ${D}${sbindir} 是存储可执行文件的位置（例如 /sbin）
    #
    install -d ${D}${sysconfdir}/init.d
    install -d ${D}${sysconfdir}/rcS.d
    install -d ${D}${sysconfdir}/rc1.d
    install -d ${D}${sysconfdir}/rc2.d
    install -d ${D}${sysconfdir}/rc3.d
    install -d ${D}${sysconfdir}/rc4.d
    install -d ${D}${sysconfdir}/rc5.d
    install -d ${D}${sbindir}

    #
    # 将脚本安装到 Linux 发行版中
    #
    # 通过 SRC_URI 获取的文件会存在于 ${WORKDIR} 目录下
    # ${WORKDIR}=file://
    #
    install -m 0755 ${WORKDIR}/startup-script  ${D}${sysconfdir}/init.d/
    install -m 0755 ${WORKDIR}/run-script      ${D}${sysconfdir}/init.d/
    install -m 0755 ${WORKDIR}/support-script  ${D}${sbindir}/

    #
    # 软链接同样可以被安装到 Linux 发行版中，比如：
    #
    # ln -s support-script-link ${D}${sbindir}/support-script

    #
    # 在 runlevel 目录下创建指向脚本的软链接
    # 以 S... 和 K... 开头的文件分别表示在 entering 和 exiting 相应 runlevel 时会被调用的脚本
    # 比如：
    #   rc5.d/S90run-script 会在进入 runlevel 5 时调用 (with %1='start')
    #   rc5.d/K90run-script 会在退出 runlevel 5 时调用 (with %1='stop')
    #
    ln -sf ../init.d/startup-script  ${D}${sysconfdir}/rcS.d/S90startup-script
    ln -sf ../init.d/run-script      ${D}${sysconfdir}/rc1.d/K90run-script
    ln -sf ../init.d/run-script      ${D}${sysconfdir}/rc2.d/K90run-script
    ln -sf ../init.d/run-script      ${D}${sysconfdir}/rc3.d/K90run-script
    ln -sf ../init.d/run-script      ${D}${sysconfdir}/rc4.d/K90run-script
    ln -sf ../init.d/run-script      ${D}${sysconfdir}/rc5.d/S90run-script
}
```

注意：`.bb` 文件中好多全局变量都是在 [`poky/meta/conf/bitbake.conf`](https://git.openembedded.org/bitbake/tree/conf/bitbake.conf) 中声明的，关于这些全局变量的解释可以参考 [Variables Glossary](https://docs.yoctoproject.org/bitbake/2.6/bitbake-user-manual/bitbake-user-manual-ref-variables.html)，比如 [`SRC_URI`](https://docs.yoctoproject.org/bitbake/2.6/bitbake-user-manual/bitbake-user-manual-ref-variables.html#term-SRC_URI)。重点理解 [`Build Directory`](https://docs.yoctoproject.org/ref-manual/terms.html#term-Build-Directory) 它和 `${TOPDIR}` 以及 `${TMPDIR}` 都有关系。

## BitBake 常用命令

| 命令                                                         | 作用                                          |
| ------------------------------------------------------------ | --------------------------------------------- |
| `source oe-init-build-env`                                   | 将 `bitbake` 添加到环境变量中                 |
| `cd $BUILD_DIR && rm -Rf tmp sstate-cache`                   | 删除所有的构建                                |
| `bitbake <recipe>`                                           | 单编一个模块                                  |
| `bitbake -c clean <recipe>`<br/>`bitbake -c cleansstate <recipe>` | 删除指定模块的构建                            |
| `bitbake -e <recipe> \| grep ^S=`           | 定位源代码所在目录                            |
| `bitbake -e <recipe> \| grep ^WORKDIR=`     | 定位 `${WORKDIR}`                              |
| `bitbake-layers show-recipes "gdb*"`                         | 搜索 `<recipe>`                               |
| `bitbake -c devshell <recipe>`                               | 进入命令行交互界面进行编译                    |
| `bitbake -c devpyshell <recipe>`                             | 进入 Python 交互界面进行编译                  |
| `bitbake -c listtasks <recipe>`                              | 列出编译 `<recipe>` 所需执行的任务            |
| `bitbake -f <recipe>`                                        | 强制构建                                      |
| `bitbake -v <recipe>`                                        | 显示构建过程的细节                            |
| `bitbake -DDD <recipe>`                                      | 显示构建过程的 Debug 信息                     |
| `yocto-layer create <layer_name>`                            | 新建一个 layer                              |
| `bitbake-layers add-layer /path/to/your_meta-layer`          | 新建一个自定义的 layer                      |
| `bitbake-layers remove-layer /path/to/your_meta-layer`       | 删除自定义的 layer                          |
| `bitbake-layers show-recipes`                                | 列出所有的 `<recipe>`                         |
| `bitbake-layers show-overlayed`                              | 列出所有冲突的 `<recipe>`                     |
| `bitbake-layers show-appends`                                | 列出所有的 `.bbappend` 文件                    |
| `bitbake-layers flatten <output_dir>`                        | 将所有的 `.bb` 文件抽离出来放到 `<output_dir>` |
| `bitbake-layers show-cross-depends`                          | 列出所有 layers 的交叉依赖关系              |
| `bitbake-layers layerindex-show-depends <layer_name>`        | 根据 OE index 列出指定 layer 的依赖         |
| `bitbake-layers layerindex-fetch <layer name>`               | 使用 OE index 拉取和添加 layer              |

## BitBake 构建流程

1. 解析 `build` 目录下的 `conf/bblayers.conf`
2. 根据 `bblayers.conf` 依次解析每个 layer 中的 `layer.conf` 和 `bitbake.conf`
3. 解决 layer 的依赖关系，生成 `cache` 文件夹
4. 根据 `.bb` 文件执行任务（根据 `.bbappend` 文件执行补充任务）

## Q & A

**ERROR: No space left on device or exceeds fs.inotify.max_user_watches?**

- <https://unix.stackexchange.com/questions/13751/kernel-inotify-watch-limit-reached>
- <https://ruanyifeng.com/blog/2011/12/inode.html>

[^ref-cite-1]: [Cookbook:Appliance:Startup Scripts - Yocto Project](https://wiki.yoctoproject.org/wiki/Cookbook:Appliance:Startup_Scripts)
