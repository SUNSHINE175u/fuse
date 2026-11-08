FUSE 时间文件系统 (Time FS)

这是一个基于 FUSE (Filesystem in Userspace) 的简单示例文件系统。它会在挂载点创建一个特殊的文件，当你读取该文件时，它会实时显示当前的系统时间。

功能特点

实时时间显示：挂载后，访问指定文件即可获取当前系统时间。
可配置文件名：可以通过命令行参数自定义显示时间的文件名。
只读访问：文件系统为只读，确保安全性。

依赖环境

在编译之前，请确保你的系统已安装 FUSE 3 开发库。

Debian/Ubuntu:
    sudo apt-get install libfuse3-dev
  
CentOS/Fedora:
    sudo dnf install fuse3-devel
  

编译与运行

编译源代码

使用 gcc 进行编译。请确保源文件名为 hello.c（或者根据你的实际文件名调整）：

gcc -Wall hello.c pkg-config fuse3 --cflags --libs -o time_fs

创建挂载点

创建一个空目录作为文件系统的挂载点：

mkdir ~/my_mount_point

运行程序

运行编译好的可执行文件，并指定挂载点。你可以使用默认文件名，也可以自定义：

使用默认设置 (文件名为 "hello")：
./time_fs ~/my_mount_point

自定义文件名 (例如将文件命名为 "clock")：
./time_fs --name=clock ~/my_mount_point

验证功能

打开另一个终端窗口，查看挂载点下的文件并读取内容：

ls ~/my_mount_point
cat ~/my_mount_point/hello   # 或者 cat ~/my_mount_point/clock

你应该能看到类似 Current time: 2026-11-08 12:00:00 的输出。每次执行 cat 命令，时间都会更新。

卸载文件系统

使用完毕后，请记得卸载文件系统：

fusermount -u ~/my_mount_point

命令行选项
选项   描述   默认值
--name=<s>   设置显示时间的文件名   hello

-h, --help   显示帮助信息   -

代码说明

hello_getattr: 定义文件属性。这里将时间文件定义为普通文件 (S_IFREG)，权限为只读 (0444)。
hello_read: 核心逻辑。当用户读取文件时，调用 get_current_time_str() 获取格式化后的当前时间字符串，并将其复制到缓冲区返回给用户。
hello_readdir: 列出根目录下的内容，确保用户能看到我们创建的时间文件。

注意事项

如果在读取文件时遇到权限错误，请尝试在命令后添加 -o allow_other 参数（需要在 /etc/fuse.conf 中开启 user_allow_other）。
该程序主要用于学习 FUSE 的基本工作原理，生产环境请使用更健壮的实现。

需要我为你补充一个配套的Makefile，方便直接通过 make 命令编译运行吗？
