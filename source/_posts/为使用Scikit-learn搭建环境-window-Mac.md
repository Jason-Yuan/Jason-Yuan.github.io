title: "为使用Scikit-learn搭建环境(window/Mac)"
date: 2015-05-31 20:43:17
tags: ["Machine Learning", "Python", "Scikit-learn"]
categories: "Python"
---

***

# 安装 python2.7 (or python3.4) 
* Windows用户  
 * [下载](https://www.python.org/downloads/release/python-2710/)，安装
 * 正确设置PATH变量：进入 computer > Control Panel > All Control Panel Items > System > Advanced system setting > environment variables，然后点击 "Path" 然后添加 ";\C:\Python27"(或者Python33)  
 * 这样就可以在命令行界面(CLI)打开Python
* Mac用户
 * Mac自带Python2.7
 * 可以先安装Homebrew，命令`$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`，然后再安装Python3.4，命令`$ brew install python3`

<!-- more -->
 ***

# 安装 Ipython 
![Ipython](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-04d311b365fca69e_zpshgavqixo.png)

## 整体安装
* Windows用户
`$ python -m pip install ipython[all]`
* Mac用户
`$ pip install ipython[all]`
 * [all] 意味着会下载并安装Ipython以及相关需要的其他包：
`jinja2` needed for the notebook
`sphinx` needed for nbconvert
`pyzmq` needed for IPython’s parallel computing features, qt console and notebook
`pygments` used by nbconvert and the Qt console for syntax highlighting
`tornado` needed by the web-based notebook
`nose` used by the test suite
`readline (on OS X) or pyreadline (on Windows)` needed for the terminal

 * 还有一些不能通过pip安装，需要单独安装比如：Qt, PyQt 和 pandoc
 
## 独立安装
### 安装Ipython本身
* Windows用户
`install setuptools`
`$ python -m pip install pyreadline`
`$ python -m pip install ipython`

* Mac用户
`pip install ipython`

### 安装基础相关包(Basic optional dependencies )
* **readline** (for command line editing, tab completion, etc.) 
 `python -m pip install "ipython[terminal]"` 
 
* **nose** (to run the IPython test suite) 
`python -m pip install "ipython[test]" `
 
* **pyzmq** (for IPython.parallel (parallel computing)) 
`python -m pip install "ipython[parallel]" `
 
* **notebook** (Dependencies for the IPython HTML notebook) 
`python -m pip install "ipython[notebook]"`  
 * IPython notebook使用web的形式，使用时输入`ipython notebook`
 * 支持IPython notebook的浏览器有：
Chrome ≥ 13 
Safari ≥ 5 
Firefox ≥ 6 
 * Ipython notebook貌似还挺强大的，这里来不及深入，以后有时间会好好研究一下
 
更多信息，参考[Ipython官网](http://ipython.org/index.html)

***

# 安装 qtconsole 
> 包含：pyzmq、pygments 和 PyQt(or PySide)

`$ pip install pyzmq pygments`
Shortcut:
`$ pip install "ipython[qtconsole]"`

PyQt/PySide不能通过pip安装
* Windows用户 
下载 [PyQt4 for python2.7 windows 32bit](http://sourceforge.net/projects/pyqt/files/PyQt4/PyQt-4.11.3/PyQt4-4.11.3-gpl-Py2.7-Qt4.8.6-x32.exe)
* Mac用户
 * [下载SIP包](http://www.riverbankcomputing.co.uk/software/sip/download)
 * 下载Qt包
 * [下载PyQt4包](http://www.riverbankcomputing.co.uk/software/pyqt/download)
 * 安装 Qt
 * 安装 SIP
 * 安装 PyQt
 
***

# 安装 Numpy 
* Windows用户
[下载地址](http://sourceforge.net/projects/numpy/files/NumPy/1.9.2/numpy-1.9.2-win32-superpack-python2.7.exe/download )
* Mac用户
`$ pip install numpy`
 
***

# 安装 Scipy 
* Windows用户
[下载地址](http://sourceforge.net/projects/scipy/files/scipy/0.15.1/scipy-0.15.1-win32-superpack-python2.7.exe/download )
* Mac用户
`$ pip install scipy`
或者[下载地址](http://sourceforge.net/projects/scipy/files/latest/download?source=files)
 
***

# 安装 matplotlib 
![matplotlib](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-6f4d4729f7dfc2cd_zpsut0rpkwb.png)

> matplotlib是基于numpy的一套Python工具包，提供了丰富的数据绘图工具。

* Windows用户
[下载地址](https://downloads.sourceforge.net/project/matplotlib/matplotlib/matplotlib-1.4.3/windows/matplotlib-1.4.3.win32-py2.7.exe )
* Mac用户
[下载地址](http://sourceforge.net/projects/matplotlib/files/matplotlib/matplotlib-1.4.3/mac/matplotlib-1.4.3-cp27-none-macosx_10_6_intel.macosx_10_9_intel.macosx_10_9_x86_64.macosx_10_10_intel.macosx_10_10_x86_64.whl/download)
 
注: 使用命令`pip list`查看除了matplotlib你是否还安装了: 
`setuptools`
`numpy`
`Python-dateutil`
`pytz`
`pyparsing`  
 
***

# 安装 scikit-learn 
![scikit-learn](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-47adf5b3937ac8a4_zpsmr0mikgd.png)

> * scikit-learn是用于机器学习的Python工具包
* 为数据挖掘和数据分析提供简单高效的工具
* 基于NumPy, SciPy, 和 matplotlib
* 开源，BSD license 

* Scikit-learn需要环境： 
Python (>= 2.6 or >= 3.3) 
Numpy (>= 1.6.1) 
Scipy (>= 0.9) 

* Windows用户
`python -m pip install -U scikit-learn`
* Mac用户
`pip install -U scikit-learn`

大功告成，play with data and have fun! 