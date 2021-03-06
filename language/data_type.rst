NCL数据类型
====================
基本上，NCL的数据类型可以分为两类，一类为数值类型（numeric），另一类则是非数值类
型（Non-numeric）。

数值类型
-------------------------
数值类型说白点就是常用的整数、浮点数（或许你叫小数），也是在各类语言中都存在的基
本类型。numeric类型下面分属的各种类型支持NCL中所有的代数函数。由于NCL的数据结构参
照NetCDF，随着NetCDF中数据类型的改变，NCL在5.2.0版本中也增添了几种数值类型（int64，
uint64，ulong，uint，ushort，ubyte）以保持与NetCDF数据结构的同步。为了保持向后的兼容性，
NCL有增加了两个关键词，分别是enumeric (extra-numeric)和snumeric (super-numeric)，
前者（enumeric）特指新增加的这几种类型，而后者则指代所有的数值型，包括最初的numeric
和新增加的enumeric。这些关键词通常出现在函数和程序定义的形参类型制定中。对于初学
者而言，这些复杂的内部细节尚不需要完全知悉，稍作了解即可。

整型
^^^^^^^^^^^^^^^^^
也就是整数，当你在NCL命令行中输入 :code:`a = 1` ，此时变量a就被定义为
一个整型变量，值为1。你可以通过 :code:`print(a)` 来查看变量的类型。你在shell中看
到的应该是这样的一个输出::

    Variable: a  ; 这里指示输出的变量名为a
    Type: integer  ; 这里指示变量a类型为整型
    Total Size: 4 bytes ; 这里指示变量a占用的内存大小为4个字节
                1 values  ; 这里指示变量a总共有1个值
    Number of Dimensions: 1  ; 这里指示变量a的维数为1维
    Dimensions and sizes:   [1]  ; 这里指示变量a的每一维的大小，第一维上只有一个值 
    Coordinates:  ; 这里表示变量a的坐标，此时并没有坐标，所以空着
    (0) 1  ; 这里打印变量a的每一个值，和值的索引序号

.. note:: 你可能很好奇，为什么简单的打印一个变量会出来这么多东西。实际上这是因为NCL变量参照NetCDF模型，所以一个变量确定后就自动地拥有了维数、大小、坐标等等。这种我们称之为 **元数据** 的东西，在绘制图形型显得尤为重要。目前，你还不需要完全知悉这些细节。

事实上，整型数据涵盖的子类型较广泛，依据所占的内存大小，可以排列为

+------------+------------+-----------------+------------+
|   类型     |  类型大小  |    名称         |  文字后缀  |
+============+============+=================+============+
|  int64     | 64位/8字节 |  64位整型       |      q     |
+------------+------------+-----------------+------------+
|  uint64    | 64位/8字节 |  64位无符号整型 |      Q     |
+------------+------------+-----------------+------------+
|  long      | 32位| 64位 |  长整型         |      l     |
+------------+------------+-----------------+------------+
|  ulong     | 32位| 64位 |  无符号长整型   |      L     |
+------------+------------+-----------------+------------+
|  integer   | 32位/4字节 |  整型           |   i (可选) |
+------------+------------+-----------------+------------+
|  uinteger  | 32位/4字节 |  无符号整型     |      I     |
+------------+------------+-----------------+------------+
|  short     | 16位/2字节 |  短整型         |      h     |
+------------+------------+-----------------+------------+
|  ushort    | 16位/2字节 |  无符号短整型   |      H     |
+------------+------------+-----------------+------------+
|  byte      | 8位/1字节  |  字节型         |      b     |
+------------+------------+-----------------+------------+
|  ubyte     | 8位/1字节  |  无符号字节型   |      B     |
+------------+------------+-----------------+------------+

浮点型
^^^^^^^^^^^^^^^^^
也就是小数（实际上跟小数还有差别的），依据所占内存大小分为单精度（32位）和双精度
浮点数（64位）。对于浮点数的直接赋值定义不仅可以使用小数点，如 :code:`a = 0.11` ；
同时也可以使用科学记数法，如 :code:`b = 1.2E-2` ，注意这里的 **E** 也可以用小写的
**e** 替代。

+------------+------------+----------------+------------+
|   类型     |  类型大小  |    名称        |  文字后缀  |
+============+============+================+============+
|  double    | 64位/8字节 |  双精度浮点数  |    d或D    |
+------------+------------+----------------+------------+
|  float     | 32位/4字节 |  单精度浮点数  |     无     |
+------------+------------+----------------+------------+

通常而言，我们不需要关心这么多具体的数据类型，因为你会发现最常用的只有
:code:`integer` 和 :code:`float` 这两种。只有在考虑设计数据结构或者读取的二进制
数据时，你才能接触到这些恼人的东西。比如当你读取的一个NetCDF文件变量，而有时你会
发现这个变量的类型竟是 :code:`short` ，此时你可能就需要做做转换了。

________________________________________________________________________________

非数值类型
--------------------
非数值类型指的是那些没有数值的且不能被转换到数值的类型。所以一般来说非数值型只能
支持等于或者不等于的逻辑运算。有两个特殊的情况是，加号 :code:`+` 运算符在字符串
类型中被重载为字符串连接算符；而逻辑型支持所有的逻辑运算符。非数值类型有以下，

+------------+------------+----------------+
|   类型     |  类型大小  |    名称        |
+============+============+================+
|  logical   |    N/A     |    逻辑型      |
+------------+------------+----------------+
|  string    |    N/A     |    字符串型    |
+------------+------------+----------------+
| character  |  8位/1字节 |    字符型      |
+------------+------------+----------------+
|  graphic   |    N/A     |     图形       |
+------------+------------+----------------+
|    file    |    N/A     |     文件       |
+------------+------------+----------------+
|    list    |    N/A     |     列表       |
+------------+------------+----------------+

逻辑型
^^^^^^^^^^^^^^^^^
也就是你可能熟悉的真（ :code:`True` ）或假（ :code:`False`），有点不
同的是，NCL中加强了对缺测值的处理操作，所以在逻辑类型中新增了一个值 **缺测** （
:code:`Missing` ）。对于逻辑运算的知识会在稍后介绍。

字符串型
^^^^^^^^^^^^^^^^^
即变长字符串，如 :code:`a = "My name is wqshen"` 。字符串的值是用
双引号 :code:`""` （英文输入法哦）引起来一段字符。比较可惜的是NCL并不支持Unicode编码，
这使得其在中文文本处理和绘制包含中文的图形上受到了很大限制。
值得注意的是，NCL中的字符串类型是可变长度的。这意味着，你在定义一个字符串变量后
可重新赋值一个不等长度的字符串而并不引发异常。

需要认真提到的是，你必须使用双引号来定义字符串，单引号是不被允许的。并且NCL
中并没有转义字符，这多少会带来一些麻烦，比如你的字符串中本身包含双引号。官方
提出的办法是你可以使用函数 :code:`str_get_dq` 或者 :code:`tochar(34)` （这将涉及
了类型自动转换）来生成一个引号，并用 :code:`+` 来连接各字符串，比如
    
.. code::

    dq = str_get_dq()
    a = "My name is "+dq+"wqshen"+dq

字符型
^^^^^^^^^^^^^^^
要在NCL中直接赋值定义一个字符变量并不是件容易的事情，你必须使用字符
对应的ASCII值并才上 **C** 后缀来实现。一个更人性化的方法是使用函数 :code:`tochar`
或者在6.0.0版本以下，你可能需要使用 :code:`stringtocharacter`。比如创建一个 *c*
字符你可以 :code:`a = 99C` 或者 :code:`a = tochar("c")`。


图形
^^^^^^^^^^^^^^^^^
指的是HLU对象的实例。图形类型的值额可以是creat语句，HLU函数和getValues
语句的返回。在实际的应用中，通常我们使用的是预定义的图形函数，特别是对于普通用户
而言，创建自定义图形变量或获取图形变量编程基本不会被用到。

文件
^^^^^^^^^^^^^^^^^
值得是NCL直接支持的文件，使用函数 :code:`addfile` 的返回值的类型就是文
件。

列表
^^^^^^^^^^^^^^^^^^
相当于一个容器，可以支持任何类型对象。然而，目前来看NCL中的列表类型能
提供的用途还很鸡肋。大多数情况下，你很少去定义列表变量，通常是在你不自觉的情况下，
使用到的。比如使用函数 :code:`addfiles` 读取多个文件，其返回的就是一个文件列表。

________________________________________________________________________________

类型转换
----------------------


类型自动转换
^^^^^^^^^^^^^^^^^^^
类型自动转换指数据内在地从一个类型转换到另一个类型，这可能发生在以下几种场合

1. 将一个类型值赋值到另一个不同类型的变量

.. code::

    a = 1.2  ; 将浮点数1.2赋值到a，a的类型为浮点型
    a = 2    ; 将整型变量赋值给浮点型变量a，程序自动将2转换为浮点数

以上代码若颠倒顺序将引发异常，因为浮点数转为整型将可能丢失数据，因此NCL中并不支
持从浮点型到整型的自动类型转换

2. 传入函数/程序的实参不是函数/程序形参声明的类型

.. code::
    
    ;; 定义函数area求圆的面积，参数为半径radius，指定radius的类型为浮点型
    function area(radius:float)
    begin
        area_circle = 2*3.14159*radius^2
        return area_circle
    end

    r = 1h  ; 赋值短整型数1到变量r
    print(area(r))  ; 调用area函数计算面积

上述代码中，短整型实参在传递给函数area的浮点型形参radius时，NCL首先会将传入的参
数转换为浮点型。你可以试试在 :code:`begin` 下加一句 :code:`print(radius)` 查看哦。

不用困惑于函数的定义和使用，在以后的章节中会深入涉及。

3. 两个不同类型的值或变量参与运算

.. code::
    
    c = 1.5 + "mcs"

以上代码中，浮点数1.5和字符串"mcs"相加，NCL自动将1.5转化为字符串后，与"mcs"串接，
之后得到结果 "1.5mcs" 。

类型强制转换
^^^^^^^^^^^^^^^^^^
在自动转换失败的情况下，我们可以使用NCL标准库中的类型转换函数集。在5.2.0版本以前，
NCL中的类型转换函数只能将某一特定数据类型转换到另一特定数据类型，使用起来较为繁琐。
从5.2.0版开始，一批新的类型转换函数集被引入，这类函数以 :code:`to` 开头，可以将任
何可能的数据类型转换到指定的数据类型。

+-----------------------+---------------------------------------------+
|   函数                |   说明                                      |
+=======================+=============================================+
| todouble_             |                                             |
+-----------------------+---------------------------------------------+
| tofloat_              |                                             |
+-----------------------+---------------------------------------------+
| tolong_               |                                             |
+-----------------------+---------------------------------------------+
| toulong_              |                                             |
+-----------------------+---------------------------------------------+
| toint64_              |                                             |
+-----------------------+---------------------------------------------+
| touint64_             |                                             |
+-----------------------+---------------------------------------------+
| tointeger_            |                                             |
+-----------------------+---------------------------------------------+
| toint_                |                                             |
+-----------------------+---------------------------------------------+
| touint_               |                                             |
+-----------------------+---------------------------------------------+
| toshort_              |                                             |
+-----------------------+---------------------------------------------+
| toushort_             |                                             |
+-----------------------+---------------------------------------------+
| tobyte_               |                                             |
+-----------------------+---------------------------------------------+
| toubyte_              |                                             |
+-----------------------+---------------------------------------------+
| tostring_             |                                             |
+-----------------------+---------------------------------------------+
| tochar_               |                                             |
+-----------------------+---------------------------------------------+
| tosigned_             |                                             |
+-----------------------+---------------------------------------------+
| tounsigned_           |                                             |
+-----------------------+---------------------------------------------+
| totype_               |                                             |
+-----------------------+---------------------------------------------+
| tostring_with_format_ |                                             |
+-----------------------+---------------------------------------------+


.. _todouble: http://www.ncl.ucar.edu/Document/Functions/Built-in/todouble.shtml
.. _tofloat: http://www.ncl.ucar.edu/Document/Functions/Built-in/tofloat.shtml
.. _tolong: http://www.ncl.ucar.edu/Document/Functions/Built-in/tolong.shtml
.. _toulong: http://www.ncl.ucar.edu/Document/Functions/Built-in/toulong.shtml
.. _toint64: http://www.ncl.ucar.edu/Document/Functions/Built-in/toint64.shtml
.. _touint64: http://www.ncl.ucar.edu/Document/Functions/Built-in/touint64.shtml
.. _tointeger: http://www.ncl.ucar.edu/Document/Functions/Built-in/tointeger.shtml
.. _toint: http://www.ncl.ucar.edu/Document/Functions/Built-in/toint.shtml
.. _touint: http://www.ncl.ucar.edu/Document/Functions/Built-in/touint.shtml
.. _toshort: http://www.ncl.ucar.edu/Document/Functions/Built-in/toshort.shtml
.. _toushort: http://www.ncl.ucar.edu/Document/Functions/Built-in/toushort.shtml
.. _tobyte: http://www.ncl.ucar.edu/Document/Functions/Built-in/tobyte.shtml
.. _toubyte: http://www.ncl.ucar.edu/Document/Functions/Built-in/toubyte.shtml
.. _tostring: http://www.ncl.ucar.edu/Document/Functions/Built-in/tostring.shtml
.. _tochar: http://www.ncl.ucar.edu/Document/Functions/Built-in/tochar.shtml
.. _tosigned: http://www.ncl.ucar.edu/Document/Functions/Built-in/tosigned.shtml
.. _tounsigned: http://www.ncl.ucar.edu/Document/Functions/Built-in/tounsigned.shtml
.. _totype: http://www.ncl.ucar.edu/Document/Functions/Built-in/totype.shtml
.. _tostring_with_format: http://www.ncl.ucar.edu/Document/Functions/Built-in/tostring_with_format.shtml



________________________________________________________________________________

类型测试
--------------
NCL中有一类函数集用于对变量进行类型测试，这类函数接收一个输入（值或变量），结果
返回输入是否属于指定类型，如果属于则返回真（ ``True`` ），不属于则返回
假（ ``False`` ）。

+-------------+---------------------------------------------+
|   函数      |   说明                                      |
+=============+=============================================+
| issnumeric_ | 测试输入是否为snumeric型                    |     
+-------------+---------------------------------------------+
| isnumeric_  | 测试输入是否为数值型（numeric）             |     
+-------------+---------------------------------------------+
| isenumeric_ | 测试输入是否为附加数值型（snumeric）        |     
+-------------+---------------------------------------------+
| isdouble_   | 测试输入是否为双精度浮点型                  |
+-------------+---------------------------------------------+
| isfloat_    | 测试输入是否为双精度浮点型                  |
+-------------+---------------------------------------------+
| isint64_    | 测试输入是否为64位整型                      |
+-------------+---------------------------------------------+
| isuint64_   | 测试输入是否为64位无符号整型                |
+-------------+---------------------------------------------+
| islong_     | 测试输入是否为长整型                        |
+-------------+---------------------------------------------+
| isulong_    | 测试输入是否为无符号长整型                  |
+-------------+---------------------------------------------+
| isinteger_  | 测试输入是否为整型                          |
+-------------+---------------------------------------------+
| isint_      | 测试输入是否为整型                          |
+-------------+---------------------------------------------+
| isuint_     | 测试输入是否为无符号整型                    |
+-------------+---------------------------------------------+
| isshort_    | 测试输入是否为短整型                        |
+-------------+---------------------------------------------+
| isushort_   |  测试输入是否为无符号短整型                 |
+-------------+---------------------------------------------+
| isbyte_     | 测试输入是否为字节型                        |
+-------------+---------------------------------------------+
| isubyte_    | 测试输入是否为无符号字节型                  |
+-------------+---------------------------------------------+
| isunsigned_ | 测试输入是否为无符号数                      |
+-------------+---------------------------------------------+
| islogical_  | 测试输入是否为逻辑型                        |
+-------------+---------------------------------------------+
| isstring_   | 测试输入是否为字符串型                      |
+-------------+---------------------------------------------+
| ischar_     | 测试输入是否为字符型                        |
+-------------+---------------------------------------------+
| isgraphic_  | 测试输入是否为图形                          |
+-------------+---------------------------------------------+
| isfile_     | 测试输入是否为文件类型                      |
+-------------+---------------------------------------------+
| isscalar_   | 测试输入是否为标量                          |
+-------------+---------------------------------------------+

.. _issnumeric: http://www.ncl.ucar.edu/Document/Functions/Built-in/issnumeric.shtml
.. _isnumeric: http://www.ncl.ucar.edu/Document/Functions/Built-in/isnumeric.shtml
.. _isenumeric: http://www.ncl.ucar.edu/Document/Functions/Built-in/isenumeric.shtml
.. _isdouble: http://www.ncl.ucar.edu/Document/Functions/Built-in/isdouble.shtml 
.. _isfloat: http://www.ncl.ucar.edu/Document/Functions/Built-in/isfloat.shtml  
.. _isint64: http://www.ncl.ucar.edu/Document/Functions/Built-in/isint64.shtml 
.. _isuint64: http://www.ncl.ucar.edu/Document/Functions/Built-in/isuint64.shtml 
.. _islong: http://www.ncl.ucar.edu/Document/Functions/Built-in/islong.shtml 
.. _isulong: http://www.ncl.ucar.edu/Document/Functions/Built-in/isulong.shtml 
.. _isinteger: http://www.ncl.ucar.edu/Document/Functions/Built-in/isinteger.shtml 
.. _isint: http://www.ncl.ucar.edu/Document/Functions/Built-in/isint.shtml 
.. _isuint: http://www.ncl.ucar.edu/Document/Functions/Built-in/isuint.shtml 
.. _isshort: http://www.ncl.ucar.edu/Document/Functions/Built-in/isshort.shtml 
.. _isushort: http://www.ncl.ucar.edu/Document/Functions/Built-in/isushort.shtml 
.. _isbyte: http://www.ncl.ucar.edu/Document/Functions/Built-in/isbyte.shtml 
.. _isubyte: http://www.ncl.ucar.edu/Document/Functions/Built-in/isubyte.shtml 
.. _isunsigned: http://www.ncl.ucar.edu/Document/Functions/Built-in/isunsigned.shtml 
.. _islogical: http://www.ncl.ucar.edu/Document/Functions/Built-in/islogical.shtml 
.. _isstring: http://www.ncl.ucar.edu/Document/Functions/Built-in/isstring.shtml 
.. _ischar: http://www.ncl.ucar.edu/Document/Functions/Built-in/ischar.shtml 
.. _isgraphic: http://www.ncl.ucar.edu/Document/Functions/Built-in/isgraphic.shtml 
.. _isfile: http://www.ncl.ucar.edu/Document/Functions/Built-in/isfile.shtml    
.. _isscalar:  http://www.ncl.ucar.edu/Document/Functions/Built-in/isscale.shtml
