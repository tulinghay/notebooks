# numpy

```python
import numpy  as np
t=np.array(range(1,4))
t=np.array(range(1,4),dtype="i1")#有符号八位字节类型的数据
t1=t.astype("i2")#转化数据类型为i2
#形状
t1.shape
#该表形状
t2=t1.reshape((3,4))


#读取txt文件中以逗号分隔的数据,delimiter为文件中的分隔符，int为数据类型,unpack是否转置默认为false
t=np.loadtxt(filepath,delimiter=",",dtype="int",unpack=True)
#转置
t.transpose()
#转置
t.T
#输出其中小于10的元素
t[t<10]
#将t中小于10的元素赋值为10
t[t<10]=10
#将t中小于10的置为0，其余的置为10
np.where(t[t<10],0,10)
#将t中小于10的替换为10，大于18的替换为18
t.clip(10,18)


#竖直拼接
np.vstack((t1,t2))
#水平拼接
np.hstack((t1,t2))
#行交换
t[[1,2],:]=t[[2,1],:]
#列交换
t[:,[0,2]]=t[:,[2,0]]
#创建0矩阵
np.zeros((3,4))
#创建对角矩阵n为10
np.eye(10)
#求0轴的最大元素的索引
np.argmax(t,axis=0)
#求1轴的最小元素的索引
np.argmin(t,axis=1)

#复制b到a，两者数据不影响
a=b.copy()

#判断nan的数量
np.count_nonzero(t!=t)
#返回同shape的bool矩阵，nan为True
np.isnan(t2)

#求和
np.sum(t)
#对应行求和，默认axis为空
np.sum(t,axis=0)
t.mean()
np.median(t)
t.max()
t.min()
#最大值和最小值之差
np.ptp(t)
#标准差
t.std()
```

# panda

```python
import pandas as pd
# 一维数据，只有行索引
t=pd.Series([1,2,3,4,5])
#执行特殊索引
pd.Series([1,2,3,4,5],index=list("abcdef"))
#传入字典后，索引会变成字典中的key
pd.Series(tempdict)
#根据索引名取数据
t["huang"]
t[["huang","da"]]

#读取csv文件
df=pd.read_csv(filename)
#从数据库读取数据
pd.read_sql(sql,conn)

#二维数据,有行索引和列索引，axis=0是行，axis=1是列,index是行索引，columns是列索引
#如果传入的为字典，则key为列索引，行索引默认为数字
df=pd.DataFrame(np.arange(12).reshape(3,4),index=list("abc"),columns=list("WXYZ"))
df.index
df.columns
df.values
df.shape
df.dtypes
df.ndim

pd.head(3)#显示前3行,不传参数默认5行
df.tail(3)#显示后3行,不传参数默认5行
df.info()#显示行数，列数，列索引，等数据
df.describe()#均值方差等信息

df[:20]#取前20行
df["Row_Lables"]#取列名为Row_Lables这一列
df[:20]["Row_Lables"]#取前20行列名为Row_Lables这一个数据
df.loc[["a","b"]] #通过标签索引行数据,行索引为a和z的
df.loc[["a","b"],:]#通过标签索引行数据,行索引为a的数据

df.iloc[1,:]#通过位置获取列数据
df.iloc[:,2]#获取第二列数据
df.iloc[:,[2,1]]#获取获取第1，2列数据
df.iloc[[0,2],[2,1]]#获取第0,2行和1,2列交叠的数据
df.iloc[1:,:2]#第一行到最后一行，前两列
#输出索引行大于800小于1000的数据
print(df[(800<df["索引"])&(df["索引"]<1000)])
#清除nan
pd.isnull()
pd.notnull()
#w列不为null的行
t[pd.notnull(t["w"])]
#删除有nan的行
t.dropna(axis=0,how="any")
#删除全为nan的行
t.dropna(axis=0,how="all")
#加了inplace=True,则直接对t进行了修改，不然就需要将结果赋值给一个新的变量
t.dropna(axis=0,how="all",inplace=True)
#填充nan为0
t.filina(0)
#想要将非0的数据不参与mean均值计算，可以将0替换为nan，nan不参与mean计算
t[t==0]=np.nan


pandas之字符串操作：https://www.cnblogs.com/eryuehong/p/13022378.html
```





































































































































































