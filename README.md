# pandas
let us learn pandas easily and happily~

将每章重要的点都总结了放在README当中，10.final是关于这几章的三个综合练习，分别是端午粽子购买数据、墨尔本温度数据以及摩拜单车骑行数据等等
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
利用?P<....>表达式可以对子组命名调用
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
使用子组名作为列名

利用?正则标记选择部分提取
#### （b）expand参数（默认为True）
对于一个子组的Series，如果expand设置为False，则返回Series，若大于一个子组，则expand参数无效，全部返回DataFrame。

对于一个子组的Index，如果expand设置为False，则返回提取后的Index，若大于一个子组且expand为False，报错。
### 2. str.extractall方法
与extract只匹配第一个符合条件的表达式不同，extractall会找出所有符合条件的字符串，并建立多级索引（即使只找到一个。

如果想查看第i层匹配，可使用xs方法。

s = pd.Series(["a1a2", "b1b2", "c1c2"], index=["A", "B", "C"],dtype="string")
s.str.extractall(two_groups).xs(1,level='match')
### 3. str.contains和str.match 
前者的作用为检测是否包含某种正则模式。

str.match与其区别在于，match依赖于python的re.match，检测内容为是否**从头**开始包含该正则模式。

## 五、常用字符串方法
### 1. 过滤型方法
#### （a）str.strip
常用于过滤空格
#### （b）str.lower和str.upper 将大写转小写和小写转大写
#### （c）str.swapcase和str.capitalize
分别表示交换字母大小写和大写首字母

### 2. isnumeric方法
检查每一位是否都是数字。 判断是否是整数？带有小数在判断时候就为False？

# 8.分类数据
## 一、category的创建及其性质
### 1. 分类变量的创建
#### （a）用Series创建
#### （b）对DataFrame指定类型创建
#### （c）利用内置Categorical类型创建
#### （d）利用cut函数创建
默认使用区间类型为标签：pd.cut(np.random.randint(0,60,5), [0,10,30,60])

可指定字符为标签：pd.cut(np.random.randint(0,60,5), [0,10,30,60], right=False, labels=['0-10','10-30','30-60'])
### 2. 分类变量的结构
一个分类变量包括三个部分，元素值（values）、分类类别（categories）、是否有序（order）
从上面可以看出，使用cut函数创建的分类变量默认为有序分类变量
#### （a）describe方法
该方法描述了一个分类序列的情况，包括非缺失值个数、元素值类别数（不是分类类别数）、最多次出现的元素及其频数
#### （b）categories和ordered属性
查看分类类别和是否排序
### 3. 类别的修改
#### （a）利用set_categories修改
修改分类，但本身值不会变化
#### （b）利用rename_categories修改
需要注意的是该方法会把值和分类同时修改
#### （c）利用add_categories添加
#### （d）利用remove_categories移除
#### (e）删除元素值未出现的分类类型
## 二、分类变量的排序
前面提到，分类数据类型被分为有序和无序，这非常好理解，例如分数区间的高低是有序变量，考试科目的类别一般看做无序变量
### 1. 序的建立
#### （a）一般来说会将一个序列转为有序变量，可以利用as_ordered方法
退化为无序变量，只需要使用as_unordered
#### （b）利用set_categories方法中的order参数
#### (c）利用reorder_categories方法
这个方法的特点在于，新设置的分类必须与原分类为同一集合
### 2. 排序
## 三、分类变量的比较操作
### 1. 与标量或等长序列的比较
#### （a）标量比较
其实就是将序列的每个元素与变量进行比较，相等则为True，不相等则为False。
#### （b）等长序列比较
两个序列的元素逐一进行比对是否相同。
### 2. 与另一分类变量的比较
#### a）等式判别（包含等号和不等号）
两个分类变量的等式判别需要满足分类完全相同
#### （b）不等式判别（包含>=,<=,<,>）
两个分类变量的不等式判别需要满足两个条件：① 分类完全相同 ② 排序完全相同

# 9.时序数据
## 一、时序的创建
### 1. 四类时间变量
#### Timestamp：日期或时间点
#### Period：由时间点定义的一段时期
#### DateOffset：一段时间的相对大小（与夏/冬令时无关）
#### Timedelta：一段时间的绝对大小（与夏/冬令时有关）
### 2. 时间点的创建
#### （a）to_datetime方法
Pandas在时间点建立的输入格式规定上给了很大的自由度，下面的语句都能正确建立同一时间点。

此时可利用format参数强制匹配。

使用列表可以将其转为时间点索引：pd.Series(range(2),index=pd.to_datetime(['2020/1/1','2020/1/2']))

#### （b）时间精度与范围限制
事实上，Timestamp的精度远远不止day，可以最小到纳秒ns。

同时，它带来范围的代价就是只有大约584年的时间点是可用的

#### （c）date_range方法
一般来说，start/end/periods（时间点个数）/freq（间隔方法）是该方法最重要的参数，给定了其中的3个，剩下的一个就会被确定。

其中freq参数有许多选项，下面将常用部分罗列如下
符号 | D/B | W | M/Q/Y | BM/BQ/BY | MS/QS/YS | BMS/BQS/BYS | H | T | S
:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:
描述 | 日/工作日 | 周 | 月末 | 月/季/年末日 | 月/季/年末工作日 | 月/季/年初日 | 月/季/年初工作日 | 小时 | 分钟 |秒

bdate_range是一个类似与date_range的方法，特点在于可以在自带的工作日间隔设置上，再选择weekmask参数和holidays参数
它的freq中有一个特殊的'C'/'CBM'/'CBMS'选项，表示定制，需要联合weekmask参数和holidays参数使用
例如现在需要将工作日中的周一、周二、周五3天保留，并将部分holidays剔除

### 3. DateOffset对象
#### （a）DataOffset与Timedelta的区别
Timedelta绝对时间差的特点指无论是冬令时还是夏令时，增减1day都只计算24小时
DataOffset相对时间差指，无论一天是23\24\25小时，增减1day都与当天相同的时间保持一致
例如，英国当地时间 2020年03月29日，01:00:00 时钟向前调整 1 小时 变为 2020年03月29日，02:00:00，开始夏令时

#### （b）增减一段时间
DateOffset的可选参数包括years/months/weeks/days/hours/minutes/seconds

pd.Timestamp('2020-01-01') + pd.DateOffset(minutes=20) - pd.DateOffset(weeks=2)
#### （c）各类常用offset对象
freq | D/B | W | (B)M/(B)Q/(B)Y | (B)MS/(B)QS/(B)YS | H | T | S | C |
:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:
offset | DateOffset/BDay | Week | (B)MonthEnd/(B)QuarterEnd/(B)YearEnd | (B)MonthBegin/(B)QuarterBegin/(B)YearBegin | Hour | Minute | Second | CDay(定制工作日)

#### （d）序列的offset操作
利用apply函数

## 二、时序的索引及属性
### 1. 索引切片
### 2. 子集索引
### 3. 时间点的属性
#### 采用dt对象可以轻松获得关于时间的信息
#### 利用strftime可重新修改时间格式
#### 对于datetime对象可以直接通过属性获取信息
pd.date_range('2020','2021', freq='W').month

pd.date_range('2020','2021', freq='W').weekday

## 三、重采样
所谓重采样，就是指resample函数，它可以看做时序版本的groupby函数
### 1. resample对象的基本操作
采样频率一般设置为上面提到的offset字符

### 2. 采样聚合

### 3. 采样组的迭代

## 四、窗口函数
下面主要介绍pandas中两类主要的窗口(window)函数:rolling/expanding

### 1. Rolling
#### (a）常用聚合
所谓rolling方法，就是规定一个窗口，它和groupby对象一样，本身不会进行操作，需要配合聚合函数才能计算结果

min_periods参数是指需要的非缺失数据点数量阀值
- s.rolling(window=50,min_periods=3).mean().head()
count/sum/mean/median/min/max/std/var/skew/kurt/quantile/cov/corr都是常用的聚合函数

#### （b）rolling的apply聚合
使用apply聚合时，只需记住传入的是window大小的Series，输出的必须是标量即可，比如如下计算变异系数。 

#### （c）基于时间的rolling

### 2. Expanding
#### （a）expanding函数
普通的expanding函数等价与rolling(window=len(s),min_periods=1)，是对序列的累计计算。 

#### （b）几个特别的Expanding类型函数
- cumsum/cumprod/cummax/cummin都是特殊expanding累计计算方法
- shift/diff/pct_change都是涉及到了元素关系
- ①shift是指序列索引不变，但值向后移动
- ②diff是指前后元素的差，period参数表示间隔，默认为1，并且可以为负
- ③pct_change是指前后元素的变化百分比，period参数与diff类似
