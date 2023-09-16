# CST LAB0 实验报告 朱志杭 2022012069
## 2.0思考题
### 问题一：
$10n^2=0.5\div10^{-8}$  
$n=\sqrt{5}×10^3≈2236$  
应准备n最大为2236的黑盒数据
### 问题二:  
$20×n×log_2⁡n=(1÷2)÷10^{-8}$  
$n≈145746$  
应准备n最大为145746的黑盒数据
## 3.2节内容
### 代码bug
#### solution_1.cpp
##### 1.将sum定义在询问循环外并且在每次计算后没有将sum归0。
找到方式：静态检查
##### 2.数组matrix不够大，可能造成数组越界
找到方式：静态检查
##### 3.sum应使用long long 类型，否则可能导致数据溢出。
找到方式：静态检查
#### solution_2.cpp
##### 1.计算范围错误，计算sum时应计算`rowsum[x + j][y + b -1 ] - rowsum[x + j][y - 1 ];`
找到方式：输出调试
##### 2.将sum定义在询问循环外并且在每次计算后没有将sum归0。
找到方式：静态检查
##### 3.数组matrix不够大，可能造成数组越界。
找到方式：静态检查
##### 4.sum应使用long long 类型，否则可能导致数据溢出。
找到方式：静态检查

### 调试方法 
编译选项：`g++ -g -o solution_2 solution_2.cpp`  
启动GDB：`lldb solution_2`  
设置断点：`b 36`  
运行程序：`run`  
查看sum值：`print sum`  
退出：`quit`  
图形化界面调试![图形化界面调试](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/图形化界面.png?raw=true)

### rand_input.cpp 中调用 srand(time(0)) 的作用
`srand(time(0)) `的作用是初始化伪随机数生成器，`time(0)`函数返回当前系统时间的秒数（从1970年1月1日起的秒数），它在每一秒都会变化。`srand()`是C/C++中的一个函数，用于设置伪随机数生成器的种子（seed）。伪随机数生成器是一个算法，它根据种子生成一系列看似随机的数字。种子的不同值将导致不同的随机数序列。因为`time(0)`的值每秒都在变化，从而生成不同的随机数序列。

### 代码解释
`system("g++ rand_input.cpp -o rand_input");`这个命令编译名为rand_input.cpp的C++源代码文件，并将可执行文件的名称设置为rand_input。

`system("g++ check_input.cpp -o check_input");`这个命令编译名为check_input.cpp的C++源代码文件，并将可执行文件的名称设置为check_input。

`system("g++ solution_1.cpp -o solution_1");`这个命令编译名为solution_1.cpp的C++源代码文件，并将可执行文件的名称设置为solution_1。

`system("g++ solution_2.cpp -o solution_2");`这个命令编译名为solution_2.cpp的C++源代码文件，并将可执行文件的名称设置为solution_2。

`system("./rand_input > rand.in");`这个命令运行名为rand_input的可执行文件，并将其输出重定向到名为rand.in的文件中。

`if(system("./check_input < rand.in")!=0)`这个条件语句运行名为check_input的可执行文件，将rand.in文件的内容作为输入，并检查其返回值是否为0。如果返回值不是0，它会打印"invalid input!"并退出循环，否则表示输入数据有效。

`system("./solution_1 < rand.in > 1.out");`这个命令运行名为solution_1的可执行文件，将rand.in文件的内容作为输入，并将其输出重定向到名为1.out的文件中。

`system("./solution_2 < rand.in > 2.out");`这个命令运行名为solution_2的可执行文件，将rand.in文件的内容作为输入，并将其输出重定向到名为2.out的文件中。

`if(system("diff 1.out 2.out")!=0)`这个条件语句运行diff命令来比较1.out和2.out两个文件的内容。如果这两个文件的内容不同，它会打印"different output!"并退出循环，否则表示两个解决方案的输出相同。

### 回答 main 函数的 argv 的参数的首项（即 `argv[0]`）的含义
`argv[0]`是一个指向表示程序名称或路径的字符串的指针。这个字符串通常包括执行程序的文件名，包括完整的路径。

## 4.0算法思路
先将数据读入一个数组，在另一个辅助数组cornsum中，每一个位置存储从数组左上角到该位置上所有数据的总和，该值可以通过该位置的数，辅助数组中该位置左侧，上方及左上方位置的数据得出。处理查询时可以通过等式`sum = cornsum[ x + a -1 ][ y + b - 1 ] - cornsum[ x - 1 ][ y + b - 1 ] - cornsum[ x + a - 1 ][ y - 1 ] + cornsum[ x - 1 ][ y - 1 ];`直接得出答案，原理是`cornsum[ x + a -1 ][ y + b - 1 ]`的值含有所有要求的值，减去`cornsum[ x - 1 ][ y + b - 1 ]`和`cornsum[ x + a - 1 ][ y - 1 ]`后已经只包含要求的值，但`cornsum[ x - 1 ][ y - 1 ]`被减了两次，因此需要再加一次，由上，每次询问只需O（1）时间。

## 4.1性能比较
通过在  
n, m：10-100，900-1000，1800-2000  
q：10-100，900-1000，9000-10000,90000-100000  
情况下的比较，发现solution3在n，m，q均较大的时候表现出明显优势，同等情况下q提高时solution3的优势更明显。  
在n，m：10-100，q：10-100 时solution3没有明显优势。  
在n，m：1800-2000，q：90000-100000 时solution3有明显优势。  
### n，m：10-100，q：10-100  
![n，m：10-100，q：10-100](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm%2010-100%20q%2010-100.png?raw=true)    
### n，m：1800-2000，q：90000-100000  
![n，m：1800-2000，q：90000-100000](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm900-1000%20q90000-100000.png?raw=true)   
### 后附其他情况下的运行时间截图  
#### n，m：10-100，q：900-1000   
![n，m：10-100，q：900-1000](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm10-100%20q%20900-1000.png?raw=true)  
#### n，m：10-100，q：9000-10000
![n，m：10-100，q：9000-10000](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm10-100%20q%209000-10000.png?raw=true)
#### n，m：10-100，q：90000-100000
![n，m：10-100，q：90000-100000](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm10-100%20q90000-100000.png?raw=true)
#### n，m：900-1000，q：10-100
![n，m：900-1000，q：10-100](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm900-1000%20q10-100.png?raw=true) 
#### n，m：900-1000，q：900-1000
![n，m：900-1000，q：900-1000](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm900-1000%20q900-1000.png?raw=true)
#### n，m：900-1000，q：9000-10000
![n，m：900-1000，q：9000-10000](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm900-1000%20q9000-10000.png?raw=true)
#### n，m：900-1000，q：90000-100000
![n，m：900-1000，q：90000-100000](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm900-1000%20q90000-100000.png?raw=true)
#### n，m：1800-2000，q：10-100
![n，m：1800-2000，q：10-100](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm1800-2000%20q10-100.png?raw=true)
#### n，m：1800-2000，q：900-1000
![n，m：1800-2000，q：900-1000](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm1800-2000%20q900-1000.png?raw=true)
#### n，m：1800-2000，q：9000-10000
![n，m：1800-2000，q：9000-10000](https://github.com/David-Zhu03/Pictures_for_CST_LAB0_report/blob/main/nm1800-2000%20q9000-10000.png?raw=true)