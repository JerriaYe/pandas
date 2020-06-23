# pandas
let us learn pandas easily and happily~

# 6.缺失数据
## 1.了解缺失信息
### 缺失观测及其类型 
isna和notna方法

### 查看缺失值的所在行
df[df['Physics'].isna()]
### 挑选出所有非缺失值列
df[df.notna().all(1)]

## 2.三种缺失符号
### np.nan
它在numpy中的类型为浮点，由此导致数据集读入时，即使原来是整数的列，只要有缺失值就会变为浮点型

对于布尔类型的列表，如果是np.nan填充，那么它的值会自动变为True而不是False

但当修改一个布尔列表时，会改变列表类型，而不是赋值为True
### None
它的布尔值为False 

修改布尔列表不会改变数据类型
### NaT
NaT是针对时间序列的缺失值，是Pandas的内置类型，可以完全看做时序版本的np.nan，与自己不等，且使用equals是也会被跳过

修改布尔列表会改变数据类型

## 3.Nullable类型与NA符号
### Nullable整型 float：NaN int：<NA>
它与原来标记int上的符号区别在于首字母大写：'Int’

它的好处就在于，其中前面提到的三种缺失值都会被替换为统一的NA符号，且不改变数据类型

### Nullable布尔

### string类型
它本质上也属于Nullable类型，因为并不会因为含有缺失而改变类型

此外，和object类型的一点重要区别就在于，在调用字符方法后，string类型返回的是Nullable类型，object则会根据缺失类型和数据类型而改变


