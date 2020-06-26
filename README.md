# pandas
let us learn pandas easily and happily~

# 6.缺失数据
## 一、缺失观测及其类型
### 1.了解缺失信息 
isna和notna方法

但对于DataFrame我们更关心到底每列有多少缺失值,df.isna().sum()

此外，可以通过第1章中介绍的info函数查看缺失信息,df.info()

#### 查看缺失值的所在行
df[df['Physics'].isna()]
#### 挑选出所有非缺失值列
df[df.notna().all(1)]

### 2. 三种缺失符号
#### np.nan
它在numpy中的类型为浮点，由此导致数据集读入时，即使原来是整数的列，只要有缺失值就会变为浮点型

对于布尔类型的列表，如果是np.nan填充，那么它的值会自动变为True而不是False

但当修改一个布尔列表时，会改变列表类型（变为float64），而不是赋值为True
#### None
它的布尔值为False 

修改布尔列表不会改变数据类型
#### NaT
NaT是针对时间序列的缺失值，是Pandas的内置类型，可以完全看做时序版本的np.nan，与自己不等，且使用equals是也会被跳过

修改布尔列表会改变数据类型

在传入数值类型后，会自动变为np.nan

只有当传入object类型是保持不动，几乎可以认为，除非人工命名None，它基本不会自动出现在Pandas中
### 3. Nullable类型与NA符号
#### Nullable整型 float：NaN int：<NA>
它与原来标记int上的符号区别在于首字母大写：'Int’

它的好处就在于，其中前面提到的三种缺失值都会被替换为统一的NA符号，且不改变数据类型

#### Nullable布尔
对于该种类型而言，作用与上面的类似，记号为boolean

#### string类型
该类型是1.0的一大创新，目的之一就是为了区分开原本含糊不清的object类型，这里将简要地提及string，因为它是第7章的主题内容

它本质上也属于Nullable类型，因为并不会因为含有缺失而改变类型

此外，和object类型的一点重要区别就在于，在调用字符方法后，string类型返回的是Nullable类型，object则会根据缺失类型和数据类型而改变

### 4. NA的特性
#### 逻辑运算
只需看该逻辑运算的结果是否依赖pd.NA的取值，如果依赖，则结果还是NA，如果不依赖，则直接计算结果
#### 算术运算和比较运算
这里只需记住除了下面两类情况，其他结果都是NA即可
### 5.convert_dtypes方法
这个函数的功能往往就是在读取数据时，就把数据列转为Nullable类型，是1.0的新函数

## 二、缺失数据的运算与分组
### 1. 加号与乘号规则
使用加法时，缺失值为0

使用乘法时，缺失值为1

使用累计函数时，缺失值自动略过

### 2. groupby方法中的缺失值
自动忽略为缺失值的组 

## 三、填充与剔除
### 1. fillna方法
#### （a）值填充与前后向填充（分别与ffill方法和backfill方法等价）

#### （b）填充中的对齐特性

### 2. dropna方法
#### （a）axis参数 0为行，1为列
#### （b）how参数（可以选all或者any，表示全为缺失去除和存在缺失去除）
#### （c）subset参数（即在某一组列范围中搜索缺失值）
df_d.dropna(axis=0,subset=['B','C'])：axis=0是行，再利用subset来选定列

##  四、插值（interpolation）
### 1. 线性插值
#### （a）索引无关的线性插值
默认状态下，interpolate会对缺失的值进行线性插值
#### （b）与索引有关的插值
method中的index和time选项可以使插值线性地依赖索引，即插值为索引的线性函数
如果索引是时间，那么可以按照时间长短插值，对于时间序列将在第9章详细介绍

### 2. 高级插值方法
此处的高级指的是与线性插值相比较，例如样条插值、多项式插值、阿基玛插值等（需要安装Scipy）
关于这部分仅给出一个官方的例子，因为插值方法是数值分析的内容，而不是Pandas中的基本知识：

### 3. interpolate中的限制参数
#### （a）limit表示最多插入多少个
#### （b）limit_direction表示插值方向，可选forward,backward,both，默认前向
也就是根据前面还是后面的值来进行插值填充
#### （c）limit_area表示插值区域，可选inside,outside，默认None

# 7.文本数据
## 一、string类型的性质
### 1. string与object的区别
### 2. string类型的转换
如果将一个其他类型的容器直接转换string类型可能会出错。正确的方法是分两部转换，先转为str型object，在转为string类型。eg：pd.Series([1,'1.']).astype('str').astype('string').
## 二、拆分与拼接
### 1. str.split方法
#### (a）分割符与str的位置元素选取
根据某一个元素分割，默认为空格.

这里需要注意split后的类型是object，因为现在Series中的元素已经不是string，而包含了list，且string类型只能含有字符串
对于str方法可以进行元素的选择，如果该单元格元素是列表，那么str[i]表示取出第i个元素，如果是单个元素，则先把元素转为列表在取出.

#### （b）其他参数
expand参数控制了是否将列拆开，n参数代表最多分割多少次

### 2. str.cat方法
#### (a）不同对象的拼接模式
cat方法对于不同对象的作用结果并不相同，其中的对象包括：单列、双列、多列.

- 1)对于单个Series而言，就是指所有的元素进行字符合并为一个字符串.

其中可选sep分隔符参数，和缺失值替代字符na_rep参数

- 2)对于两个Series合并而言，是对应索引的元素进行合并

同样也有相应参数，需要注意的是两个缺失值会被同时替换

- 3）多列拼接可以分为表的拼接和多Series拼接

#### （b）cat中的索引对齐
当前版本中，如果两边合并的索引不相同且未指定join参数，默认为左连接，设置join='left
## 三、替换
广义上的替换，就是指str.replace函数的应用，fillna是针对缺失值的替换，上一章已经提及。提到替换，就不可避免地接触到正则表达式.
### 1. str.replace的常见用法
### 2. 子组与函数替换
### 3. 关于str.replace的注意事项
str.replace针对的是object类型或string类型，默认是以正则表达式为操作，目前暂时不支持DataFrame上使用。
replace针对的是任意类型的序列或数据框，如果要以正则表达式替换，需要设置regex=True，该方法通过字典可支持多列替换
但现在由于string类型的初步引入，用法上出现了一些问题，这些issue有望在以后的版本中修复
#### （a）str.replace赋值参数不得为pd.NA
#### （b）对于string类型Series，在使用replace函数时不能使用正则表达式替换
#### （c）string类型序列如果存在缺失值，不能使用replace替换
**综上，概况的说，除非需要赋值元素为缺失值（转为object再转回来），否则请使用str.replace方法**

## 四、子串匹配与提取
### 1. str.extract方法
#### （a）常见用法
#### （b）expand参数（默认为True）
对于一个子组的Series，如果expand设置为False，则返回Series，若大于一个子组，则expand参数无效，全部返回DataFrame。

对于一个子组的Index，如果expand设置为False，则返回提取后的Index，若大于一个子组且expand为False，报错。
### 2. str.extractall方法
与extract只匹配第一个符合条件的表达式不同，extractall会找出所有符合条件的字符串，并建立多级索引（即使只找到一个。
### 3. str.contains和str.match 
前者的作用为检测是否包含某种正则模式。

str.match与其区别在于，match依赖于python的re.match，检测内容为是否从头开始包含该正则模式。

## 五、常用字符串方法
### 1. 过滤型方法
#### （a）str.strip
常用于过滤空格
#### （b）str.lower和str.upper
#### （c）str.swapcase和str.capitalize
分别表示交换字母大小写和大写首字母

### 2. isnumeric方法
检查每一位是否都是数字。
