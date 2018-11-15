
[*第四章：pandas库简介*](./README.md)


# 4.2. 安装的pandas 

安装pandas库最简单和最通用的方法是使用预先包装好的解决方案，即通过Anaconda或Enthought的发行版安装。

## 从Anaconda安装

对于那些选择使用Anaconda发行版的人来说，管理安装非常简单。首先，您必须查看是否安装了pandas模块，如果安装了，是哪个版本。要做到这一点，从终端输入以下命令:

```commandline
conda list pandas
```

由于我的PC (Windows)上安装了这个模块，我得到如下结果:

```commandline
# packages in environment at C:\Users\Fabio\Anaconda:
#
pandas                    0.20.3               py36hce827b7_2
```

如果您没有安装pandas，您将需要安装它。输入以下命令:

```commandline
conda install pandas
```

Anaconda会立即检查所有的依赖项，管理其他模块的安装，而不用担心太多。

```commandline
Solving environment: done
## Package Plan ##

Environment location: C:\Users\Fabio\Anaconda3
added / updated specs:
   - pandas
   
    The following new packages will be installed:
    
    pandas: 0.22.0-py36h6538335_0
Proceed ([y]/n)?
Press the y key on your keyboard to continue the installation.
Preparing transaction: done
Verifying transaction: done
Executing transaction: done
```

如果您想要将包升级到较新的版本，则这样做的命令非常简单和直观：

```commandline
conda update pandas
```

系统将检查pandas的版本和它所依赖的所有模块的版本，然后建议任何更新。然后它会询问您是否要继续进行更新。


## 从PyPI安装

pandas也可以通过PyPI使用以下命令安装:
```commandline
pip install pandas
```

## Linux上的安装

如果您正在开发Linux发行版，并且选择不使用这些预先打包的发行版，您可以像安装其他包一样安装pandas模块。

在Debian和Ubuntu发行版上，使用以下命令:

```commandline
sudo apt-get install python-pandas
```

在OpenSuse和Fedora上，输入以下命令:

```commandline
zypper in python-pandas
```

## 从源代码安装

如果你想从源代码编译你的pandas模块，你可以在GitHub上找到你需要的https://github.com/pandas-dev/pandas:

```commandline
git clone git://github.com/pydata/pandas.git
cd pandas
python setup.py install
```
确保在编译时安装了Cython。要了解更多信息，请阅读Web上的文档，包括官方页面(http://pandas.pydata.org/pandas-docs/stable/install.html)。


## 用于Windows的模块存储库

如果您在Windows上工作，并且希望管理您的包，以便始终拥有最新的模块，那么您还可以在Internet上下载许多第三方模块—christoph Gohlke的Windows存储库Python扩展包(www.lfd.uci.edu/~ Gohlke /pythonlibs/)。每个模块都提供32位和64位的format archival WHL (wheel)。要安装每个模块，您必须使用pip应用程序(参见第2章的PyPI)。

```commandline
pip install SomePackege-1.0.whl
```

例如，对于pandas，你可以找到并下载以下包:

```commandline
pip install pandas-0.22.0-cp36-cp36m-win_amd64.whl
```

在选择模块时，要仔细地为您的Python版本和您工作的体系结构选择正确的版本。此外，虽然NumPy不需要安装其他包，但与此相反，pandas有许多依赖项。所以一定要把它们都弄好。安装顺序并不重要。

这种方法的缺点是，您需要单独安装包，而不需要一个包管理器来帮助管理不同包之间的版本控制和相互依赖关系。其优点是对模块及其版本有更强的掌握，因此您可以在不依赖于发行版的选择的情况下使用最新的模块。



