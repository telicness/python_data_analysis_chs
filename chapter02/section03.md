
[*第2章：Python世界简介*](./)


# 2.3. PyPI-Python包索引

Python包索引(PyPI)是一个软件资源库，它包含Python编程所需的所有软件，例如，属于其他Python库的所有Python包。内容存储库由各个包的开发人员直接管理，这些包处理用发布库的最新版本更新存储库。要查看存储库中包含的包的列表，请访问PyPI的官方页面:https://pypi.python.org/pypi。

就这些包的管理而言，您可以使用pip应用程序，它是PyPI的包管理器。

通过从命令行启动它，您可以管理所有包，并单独决定是否应该安装、升级或删除包。pip将检查包是否已经安装，或者是否需要更新以进行控制依赖关系，并评估是否需要其他包。此外，它还管理下载和安装过程。

```commandline
$ pip install <<package_name>>
$ pip search <<package_name>>
$ pip show <<package_name>>
$ pip unistall <<package_name>>
```

关于安装，如果您的系统上已经安装了Python 3.4+(发布于2014年3月)和Python 2.7.9+(发布于2014年12月)，那么pip软件已经包含在这些Python版本中。但是，如果您仍然使用较旧版本的Python，则需要在系统上安装pip。pip在系统上的安装取决于您所工作的操作系统。

在Linux Debian-Ubuntu上，使用以下命令:

```commandline
$ sudo apt-get install python-pip
```

在Linux Fedora上，使用以下命令:

```commandline
$ sudo yum install python-pip
```

在windows上，访问https://pip.pypa.io/en/latest/installing/并将get-pip.py下载到您的PC上。下载文件后，运行以下命令：

```commandline
python get-pip.py
```

这样，您将安装包管理器。记住将C：\Python3.X\Scripts 添加到PATH环境变量中。


## Python 的集成开发环境

虽然大多数Python开发人员都使用直接从shell (Python或IPython)实现代码，但也有一些ide(交互式开发环境)可用。实际上，除了文本编辑器之外，这些图形编辑器还提供了一系列工具，这些工具在编写代码时非常有用。例如，自动完成代码、查看与命令相关的文档、调试和断点只是这种应用程序能够提供的一些工具。

