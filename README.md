# NREL-FAST-installation-guide
NREL FAST installation guide
NREL FAST Record
1、	下载和安装
FAST是NREL组织编写的风机计算软件，主要使用FORTRAN语言。编译版本多是Windows上的exe文件。理论上可以在官网下载，FAST v7和v8应该都有compiling的版本。
网址 https://www.nrel.gov/wind/nwtc.html
然而这个网址里的链接我点不动，后来我发现是因为网络的关系，挂了个vpn之后链接就能点开了，但是仍然找不到下载项。据说是需要注册，我也没有找到注册入口。
我用的vpn是Rava (https://aicaptain.cn/ref/68B558EA)，据说是上外的大佬开发的，很好用，付费软件，注册即可获得3天免费体验。

网站上有一段话是这样：
All the source code files you need to compile FAST are included in the FAST v8 archive, but documentation on the individual modules must be downloaded from their respective web sites. The FAST v8 archives also contain compilation scripts and a Microsoft Visual Studio Project file, which are designed to run the FAST Registry and compile the included source code.

所以我猜测网站上下载的也是未编译的程序包，于是我直接在GitHub上copy了code包。GitHub上也存在一些不同的版本，官方给出了OpenFAST，是全新改版之后的版本，在此之前最新版本是v8，GitHub上还给出了一些v7的资源。

这里给出GitHub 上的一些资源Repository的Clone地址
1/ TUDelft给出的NREL FASTv8, with MATLAB GUI and Simulink integration:
(https://github.com/TUDelft-DataDrivenControl/FASTTool.git)
2/ 一个v7版本的FAST
(https://github.com/n1ywb/nrel-fast.git)
3/ 一个写得很详细的介绍m版本的tutorial
(https://github.com/felipepasquali/FAST_Tutorial.git)
4/ openfast版本
https://github.com/OpenFAST/openfast.git
配合一个官方给出的openfast的安装userguide
https://openfast.readthedocs.io/en/master/source/install/index.html

这里有一个详细的编译FAST时可能遇到的问题解析
https://wind.nrel.gov/forum/wind/viewtopic.php?t=642&start=30&view=print

compile的作用是你可以在任何文件夹下运行FAST软件。
compile完成后相应的文件夹下会出现一个FAST.exe文件，这就是可执行的FAST处理器。采用cmd运行文件即可。
To run the executable, open a command prompt window in the directory in which you want to work. The command-line syntax is: 
fast [/h] [<input file>] where: 
/h prints a help message.
<inputfile> isthenameoftheprimaryinputfile. 
The default name is primary.fst). 
举个例子:
fast [/h] primary.fst

2、	输入与输出文件

官方给出的user’s guide 里有详细的输入与输出文件的说明。
FAST使用一个主要的输入文件来描述风力涡轮机的运行参数和基本几何形状。然而，叶片、塔架、卷扬、空气动力学参数和风时历史是从单独的文件中读取的。此外，与FAST线性化有关的输入参数和仅对创建ADAMS数据集必要的参数也从单独的文件中读取（见图29）。下面提供了各种文件中各个输入的描述。输出文件将在输出文件一章中讨论。
主输入文件的默认名称为primary.fst，如果没有在命令行里指定文件名，FAST将尝试打开该命名的文件。如果你想对不同的情况使用不同的名称，你可以在命令行上指定一个不同名称的文件。文件的路径或名称中带有空格的文件必须用引号分隔。文件名限制在99个字符以内，可以包括绝对或相对路径。
参数输入文件具有简单的文本格式，可由任何文本编辑器阅读和修改。输入文件中的大多数行被分为三个部分：值、变量名和描述。对于需要多个值的行，用空格、制表符或逗号将它们分开。最后一个值之后的任何内容
行上的字符被视为注释。字符串变量的值必须用一对引号或双引号分隔。逻辑标志必须是没有引号的字符串，以t或T开头表示True，以f或F开头表示False。变量名称（Variable-Name）部分包含了程序内部和其他参数引用中使用的变量名称。该行的描述部分包含了参数的简要描述，以提醒用户其目的。该部分还包含数值的物理单位，如果合适的话。输入文件中输入参数TMAX的示例行，其部分划分如下所示。
  20.0 TMax--总运行时间(s)
注意，输入文件中没有空行。程序按顺序读取每一行。除非在允许添加或删除的部分，否则您不应该添加或删除任何行。塔台、叶片和 AeroDyn 输入文件有一些部分，在这些部分中，您可以为每个输入站分析节点输入一行。主要的FAST输入文件的末尾有一个输出参数列表，可以根据您的需要长短不一。
用户手册中采用56-69页的篇幅来说明输入文件包含的信息。其中包含求解器的信息，塔架、环境（重力）、叶片形状和材料、初始速度等信息。

该程序根据输入文件中的设置生成一个或多个输出文件。对于时间行进分析，主输出文件包含时间序列数据的列，主输入文件中要求的每个参数都有一列。该文件的名称使用主输入文件的路径和根名，并附加.out作为扩展名。例如，如果输入文件被命名为fast.fst，主输出文件将被命名为fast.out。可用的输出参数如表16至表44所示，也在FAST档案的OutList.txt文件中有所记载。图30所示是一个输出文件的例子。
如果SumPrint标志设置为True，AeroDyn还会生成一个摘要文件，该文件包含叶片元素几何数据、相应叶片元素处的空翼数据文件以及FAST/AeroDyn组合输入参数的摘要。在上面的例子中，这个文件将被命名为fast.opt。
