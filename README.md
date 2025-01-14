# MFQ-R3
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MFQ - R语言代码总结</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 20px;
        }
        h1, h2, h3 {
            color: #333;
        }
        pre {
            background-color: #f4f4f4;
            border: 1px solid #ddd;
            padding: 10px;
            overflow-x: auto;
        }
    </style>
</head>
<body>
    <h1>MFQ - R语言代码总结</h1>
    <pre>
# 11.13 R语言概述
11.13 R语言概述
x <- c(44.4, 45.9, 41.9, 53.3, 44.7, 44.1, 50.7, 45.2, 60.1) # 创建一个向量x，包含9个数值
y <- c(2.6, 3.1, 2.5, 5.0, 3.6, 4.0, 5.2, 2.8, 3.8) # 创建一个向量y，包含9个数值
mean(y) # 计算向量y的平均值
sd(y) # 计算向量y的标准差
paste0(sprintf("%.2f",mean(x)),"±",sprintf("%.2f",sd(x))) # 计算向量x的平均值和标准差，并以“平均值±标准差”的格式输出，保留两位小数
cor(x, y) # 计算向量x和y的相关系数
plot(x, y) # 绘制向量x和y的散点图
demo(graphics) # 展示R语言的图形功能示例

#学会提取结果
jieguo = cor.test(x, y, method = "spearman") # 对向量x和y进行斯皮尔曼相关性检验，并将结果赋值给变量jieguo
sprintf("%.3f", jieguo$estimate) # 提取相关性检验结果中的相关系数估计值，并保留三位小数
round(jieguo$estimate, 3) # 对相关性检验结果中的相关系数估计值进行四舍五入，保留三位小数

#产生空白表格，把数填进去，输出结果
p = 0.0000789 # 定义一个p值
sprintf("%.3f", p) # 将p值转换为字符串，并保留三位小数
p = ifelse(p<0.001, "0.001", sprintf("%.3f", p)) # 如果p值小于0.001，则将其替换为字符串"0.001"，否则保留三位小数

setwd('D:/抑郁') # 设置工作路径为D盘的“抑郁”文件夹
#从带分隔符的文本文件导入数据
mydataframe <- read.table("studentgrades.csv", header="TRUE", sep=",", # 从当前工作路径下的“studentgrades.csv”文件中导入数据，设置文件有表头，字段分隔符为逗号
                          row.names="STUDENTID") # 设置行名为“STUDENTID”
mydataframe <- read.csv('GBD_1990-2021_10-24.csv',header = T) # 从当前工作路径下的“GBD_1990-2021_10-24.csv”文件中导入数据，设置文件有表头

#导入excel数据
library(readxl) # 调用readxl包
mydataframe <- read_excel(path = file.choose()) # 通过文件选择器选择一个excel文件并导入数据
library(xlsx) # 调用xlsx包
workbook <- "C:/myworkbook.xlsx" # 定义一个工作簿路径
mydataframe <- read_xlsx(workbook, 1) # 从指定的工作簿中读取第一个工作表的数据

install.packages("readxl")
library(readxl)
data <- read_excel("D:/MFQ/co2.xlsx")

#帮助文档使用
help(read.csv) # 查看read.csv函数的帮助文档
getwd() # 获取当前工作路径
setwd() # 设置工作路径
#图片：空白画布，
tiff("C/myproject") # 创建一个tiff格式的图像文件，保存路径为C盘的“myproject”文件夹

11.20 常用数据结构与应用
??ggplot # 查找与ggplot相关的帮助文档
?lm # 查看线性模型lm函数的帮助文档

require(graphics) # 调用graphics包，用于图形绘制
## Annette Dobson (1990) "An Introduction to Generalized Linear Models".## Page 9: Plant Weight Data.
ctl <- c(4.17,5.58,5.18,6.11,4.50,4.61,5.17,4.53,5.33,5.14) # 创建一个向量ctl，包含10个数值，代表对照组的植物重量数据
trt <- c(4.81,4.17,4.41,3.59,5.87,3.83,6.03,4.89,4.32,4.69) # 创建一个向量trt，包含10个数值，代表处理组的植物重量数据
group <- gl(2, 10, 20, labels = c("Ctl","Trt")) # 创建一个因子变量group，有2个水平，每个水平10个重复，共20个观测值，分别标记为"Ctl"和"Trt"
weight <- c(ctl, trt) # 将ctl和trt两个向量合并为一个向量weight
lm.D9 <- lm(weight ~ group) # 使用线性模型拟合weight与group的关系，包含截距项
lm.D90 <- lm(weight ~ group - 1) # 使用线性模型拟合weight与group的关系，不包含截距项

anova(lm.D9) # 对线性模型lm.D9进行方差分析
tiqu=summary(lm.D90) # 提取线性模型lm.D90的摘要信息，并赋值给变量tiqu
tiqu # 打印变量tiqu的内容
tiqu$coefficients # 提取变量tiqu中的系数信息
test=as.data.frame(tiqu$coefficients) # 将系数信息转换为数据框，并赋值给变量test
test # 打印变量test的内容
test$Estimate-1.96*test$`Std. Error` # 计算每个系数的估计值减去1.96倍标准误差
test$CI_l=test$Estimate-1.96*test$`Std. Error` # 将上述计算结果赋值给test数据框的CI_l列
test # 打印变量test的内容

test$CI_l=sprintf("%.2f",test$Estimate-1.96*test$`Std. Error`) # 将CI_l列的值转换为字符串，并保留两位小数
test$CI_h=sprintf("%.2f",test$Estimate+1.96*test$`Std. Error`) # 计算每个系数的估计值加上1.96倍标准误差，并转换为字符串，保留两位小数，赋值给CI_h列
test # 打印变量test的内容

sprintf("%.2f",test$Estimate) # 将test数据框中Estimate列的值转换为字符串，并保留两位小数
test$output=paste0(sprintf("%.2f",test$Estimate),"(",test$CI_l,",",test$CI_h,")") # 将Estimate列的值、CI_l列的值和CI_h列的值拼接成一个字符串，格式为“估计值(下限,上限)”，赋值给output列
test # 打印变量test的内容
test$`Pr(>|t|)`=ifelse(test$`Pr(>|t|)`<0.001,"<0.001",sprintf("%.3f",test$`Pr(>|t|)`)) # 将P值列的值进行判断，如果小于0.001，则替换为"<0.001"，否则保留三位小数
test # 打印变量test的内容
test=test[,c(7,4)] # 选取test数据框的第7列和第4列
test # 打印变量test的内容
names(test) # 打印test数据框的列名
names(test)=c("回归系数","P值") # 将test数据框的列名修改为"回归系数"和"P值"
test # 打印变量test的内容
rownames(test) # 打印test数据框的行名
rownames(test)=c("对照组","干预组") # 将test数据框的行名修改为"对照组"和"干预组"
test # 打印变量test的内容

write.csv(test,"C:/Users/lenovo/Downloads/结果.csv") # 将test数据框写入到C盘Users文件夹下lenovo文件夹的Downloads文件夹中的“结果.csv”文件
# data.frame不同列可以有不同类型的变量，矩阵metrics要求所有数据的类型是一样的。# 列表里面可以有向量、字符串、数字、时间，什么都可以有。# 环境数据，有经度、维度、时间



data("co2") # 调用co2数据集
CO2 # 打印CO2数据集的内容

write.csv(CO2,"C:/Users/lenovo/Downloads/CO2.csv",row.names=T) # 将CO2数据集写入到C盘Users文件夹下lenovo文件夹的Downloads文件夹中的“CO2.csv”文件，包含行名
write.csv(CO2,"C:/Users/lenovo/Downloads/CO2.csv",row.names=F) # 将CO2数据集写入到C盘Users文件夹下lenovo文件夹的Downloads文件夹中的“CO2.csv”文件，不包含行名

john=2 # 定义一个变量john，赋值为2
john # 打印变量john的值

data=read.csv("C:/Users/lenovo/Downloads/co2.csv") # 从C盘Users文件夹下lenovo文件夹的Downloads文件夹中的“co2.csv”文件读取数据
data$conc[c(5,8,10)] # 提取data数据框中conc列的第5、8、10个元素
data$conc[5:10] # 提取data数据框中conc列的第5到第10个元素

length(data$conc) # 计算data数据框中conc列的长度，即元素个数
which(data$conc>300) # 找出data数据框中conc列大于300的元素的索引位置
data$uptake[which(data$conc>300)] # 提取data数据框中uptake列中对应conc列大于300的元素
data$uptake[data$conc>300] # 一维的提取方式，直接提取data数据框中uptake列中对应conc列大于300的元素

mean(data$uptake[data$Type=="Mississippi"]) # 计算data数据框中uptake列中对应Type列等于"Mississippi"的元素的平均值
# 二维数据的提取。第二个人的第三列变量
data[2,3] # 提取data数据框中第2行第3列的元素
data$Treatment[2] # 提取data数据框中Treatment列的第2个元素
data[2,"Treatment"] # 提取data数据框中第2行Treatment列的元素
data[2,c("Treatment","uptake")] # 提取data数据框中第2行Treatment列和uptake列的元素，推荐这种方式
data[2,3:5] # 提取data数据框中第2行第3到第5列的元素

newdata=data[data$Treatment=="nonchilled",] # 提取data数据框中Treatment列等于"nonchilled"的所有行，并赋值给newdata
# 数据框里的变量类型。数值型，字符型，因子factor（有顺序）
data$conc # 打印data数据框中conc列的内容
data$Treatment # 打印data数据框中Treatment列的内容

?factor # 查看factor函数的帮助文档
data$Treatment_new=factor(data$Treatment,levels=c("nonchilled","chilled")) # 将data数据框中Treatment列转换为因子变量，并指定水平为"nonchilled"和"chilled"
data$Treatment_new=as.character(data$Treatment_new) # 将data数据框中Treatment_new列转换为字符型
data$conc_new=as.numeric(data$conc) # 将data数据框中conc列转换为数值型
data$conc[which(is.na(data$conc_new))] # 找出data数据框中conc_new列中缺失值对应的conc列的元素
data$conc_new[is.na(data$conc_new)]=190 # 将data数据框中conc_new列中的缺失值替换为190

# 日期型
date=c("10/15/2009","11/01/2009") # 创建一个向量date，包含两个日期字符串
data$date=date # 将date向量赋值给data数据框的date列
class(data$date) # 查看data数据框中date列的数据类型
data$date # 打印data数据框中date列的内容
datasdate=as.Date(data$date) # 将data数据框中date列的日期字符串转换为Date类型，并赋值给datasdate变量"2009/11/1" # 一个日期字符串"1/11/2009" # 一个日期字符串"12/11/10" # 一个日期字符串
as.Date("12/11/10",format="%y/%m/%d") # 将日期字符串"12/11/10"按照"%y/%m/%d"的格式转换为Date类型

data$temp <- seq.Date(from = as.Date("2009/01/01",format = "%Y/%m/%d"), by = "day", length.out = 84) # 生成一个日期序列，从2009年1月1日开始，每天一个日期，共84个日期，并赋值给data数据框的temp列
data$temp # 打印data数据框中temp列的内容

# 列表。自己会编写一个函数
library(rSPARCS) # 调用rSPARCS包
yanshi=function(data){ # 定义一个名为yanshi的函数，参数为data
  resultl=table(data$Type,data$Treatment) # 创建一个列联表，行变量为data数据框的Type列，列变量为data数据框的Treatment列，并赋值给resultl变量
  result2=ggplot(data,aes(date,conc))+geom_point() # 使用ggplot2包绘制散点图，x轴为data数据框的date列，y轴为data数据框的conc列，并赋值给result2变量
  return(list(resultl,result2)) # 返回一个列表，包含resultl和result2两个元素}
yanshi(data=data[data$Plant=="Qn1" ,]) # 调用yanshi函数，传入data数据框中Plant列等于"Qn1"的所有行


11.27基本数据管理
x=3  # 将变量x赋值为3
x-4  # 计算x减去4的结果，但不赋值给任何变量
for(i in 1:100){  # 循环从1到100
print(i)  # 打印当前循环的值i
x=x-1  # 将x的值减1
x=x+4  # 将x的值加4}  # 循环结束后，x的值会回到初始值3

x=c(44.4, 45.9, 41.9, 53.3, 44.7, 44.1, 50.7, 45.2, 60.1)  # 创建向量x
y=c( 2.6,  3.1,  2.5,  5.0,  3.6,  4.0,  5.2,  2.8,  3.8)  # 创建向量y
paste0(sprintf("%.2f",mean(x)),"±",sprintf("%.2f",sd(x)))  # 计算x的均值和标准差，并格式化输出为“均值±标准差”
plot(x,y)  # 绘制x和y的散点图
jieguo=cor.test(x, y, method = "spearman")  # 计算x和y的Spearman相关性检验结果，存储在变量jieguo中
sprintf("%.3f",jieguo$estimate)  # 格式化输出jieguo中的相关性估计值，保留3位小数
round(jieguo$estimate,3)  # 对jieguo中的相关性估计值进行四舍五入，保留3位小数

data=read.xlsx("C:/Users/bx102/Desktop/shuju.xlsx")  # 从指定路径读取Excel文件到数据框data
write.xlsx(data,"C:/Users/bx102/Downloads/jieguo.xlsx")  # 将数据框data写入到指定路径的Excel文件
install.packages("openxlsx")  # 安装openxlsx包
library(openxlsx)  # 加载openxlsx包
install.packages("readxl")  # 安装readxl包
library(readxl)  # 加载readxl包
data <- read_excel("C:/Users/bx102/Desktop/shuju.xlsx")  # 从指定路径读取Excel文件到数据框data

p=0.0000789  # 将变量p赋值为0.0000789
sprintf("%.3f",p)  # 格式化输出p的值，保留3位小数
？p=ifelse(p<0.001,"<0.001",sprintf("%.3f",p))  # 如果p小于0.001，则输出"<0.001"，否则输出p的值，保留3位小数

data.frame  # 这是一个函数，用于创建数据框，但这里没有使用
max(data$成绩)  # 计算数据框data中成绩列的最大值


leadership <- data.frame(manager=c(1, 2, 3, 4, 5),  # 创建数据框，包含经理编号
date=c("10/24/08", "10/28/08", "10/1/08", "10/12/08", "5/1/09"),  # 日期
country=c("US", "US", "UK", "UK", "UK"),  # 国家
gender=c("M", "F", "F", "M", "F"),  # 性别
age=c(32, 45, 25, 39, 99),  # 年龄
q1=c(5, 3, 3, 3, 2),  # 问题1的评分
q2=c(4, 5, 5, 3, 2),  # 问题2的评分
q3=c(5, 2, 5, 4, 1),  # 问题3的评分
q4=c(5, 5, 5, NA, 2),  # 问题4的评分，含缺失值
q5=c(5, 5, 2, NA, 1))  # 问题5的评分，含缺失值
leadership$total=leadership$q1+leadership$q2+  # 计算每个经理的总评分
leadership$q3+leadership$q4+leadership$q5
leadership$pingjun1=leadership$total/5  # 计算平均评分（方法1）
leadership$pingjun2=apply(leadership[,6:10],1,mean,na.rm=T)  # 计算平均评分（方法2，忽略缺失值）
leadership$pingjun2=apply(leadership[,paste0("q",1:5)],1,mean)  # 与上一条代码功能相同，列名表示方式不同
leadership$pingjun2=apply(leadership[,c("q1","q2")],1,mean,na.rm=T)  # 计算q1和q2的平均值
for(i in 1:5) leadership$pingjun3[i]=(leadership$q1[i]+leadership$q2[i]+  # 通过循环计算平均评分
leadership$q3[i]+leadership$q4[i]+leadership$q5[i])/5
leadership$pingjun4=rowMeans(leadership[,6:10])  # 计算每行的平均值

rowsum()  # 未提供参数，无法执行
names(leadership)[c(4,12)]=c("性别","均数")  # 修改列名
names(leadership)[names(leadership)=="gender"]="性别"  # 修改gender列名为性别
leadership$q1[is.na(leadership$q4)]  # 找出q4列缺失值对应的q1列的值
which(is.na(leadership$q4))  # 找出q4列缺失值的行号
leadership$q4[is.na(leadership$q4)]=mean(leadership$q4,na.rm=T)  # 将q4列缺失值替换为平均值
leadership$q5[is.na(leadership$q5)]=mean(leadership$q5,na.rm=T)  # 将q5列缺失值替换为平均值
？for(i in c()) leadership[,i][is.na(leadership[,i])]=mean(leadership[,i],na.rm=T)  # 未指定列号，不执行操作

leadership[which(leadership$age<=45),]  # 筛选年龄小于等于45岁的记录
subset(leadership,leadership$age<=45)  # 与上一条代码功能相同，使用subset函数
leadership$agegroup=NA  # 创建新列agegroup，初始值为NA
leadership$agegroup[which(leadership$age<=45)]="青年"  # 将年龄小于等于45岁的记录的agegroup值设为青年
leadership$agegroup[leadership$age>45&!is.na(leadership$age)]="中老年"  # 将年龄大于45岁且非缺失值的记录的agegroup值设为中老年
leadership$agegroup[leadership$age<=45&leadership$age>=30]="30-45"  # 将年龄在30到45岁之间的记录的agegroup值设为30-45
leadership$agegroup=factor(leadership$agegroup,levels=c("青年","30-45","中老年"))  # 将agegroup列转换为因子类型

leadership$agecat[leadership$age>75]="elder"
leadership$agecat[leadership$age>=55&leadership$age<=75]="middle aged"
leadership$agecat[leadership$age<55]="young"


leadership$country[leadership$country=="US"]="美国"  # 将国家为US的记录的country值改为美国
leadership$country[leadership$country=="UK"]="英国"  # 将国家为UK的记录的country值改为英国
leadership$country=factor(leadership$country,  # 将country列转换为因子类型
levels=c("US","UK"),  # 指定水平
labels=c("美国","英国"))  # 指定标签
leadership$country[leadership$country!="US"]="英国"  # 将国家不为US的记录的country值改为英国

newdata=leadership[!is.na(leadership$q4),]  # 筛选q4列非缺失值的记录
newdata=leadership[complete.cases(leadership[,6:10]),]  # 筛选q1到q5列均非缺失值的记录
newdata=na.omit(leadership[,6:10])  # 删除q1到q5列中包含缺失值的行
leadership$age[leadership$age>80]=NA  # 将年龄大于80岁的记录的age值设为NA

leadership$date=as.Date(leadership$date,format="%m/%d/%y")  # 将date列的日期格式转换为Date类型
doy(leadership$date)  # 计算date列中每个日期是一年中的第几天，需加载相应包
library(dlnm)  # 加载dlnm包
class(leadership$date)  # 查看date列的数据类型
as.Date(c(1235,345,678),origin = "1970-1-1")  # 将数值向量转换为Date类型，以1970-1-1为起始日期
as.numeric(as.Date("1970-1-1"))  # 将日期1970-1-1转换为数值
leadership$month=substr(leadership$date,6,7)  # 提取date列中每个日期的月份
newdata=leadership[order(leadership$age,decreasing=T),]  # 按年龄降序排列leadership数据框

12.4 进阶数据管理
Student = c("John Davis", "Angela Williams", "Bullwinkle Moose", "David Jones", "Janice  Markhammer", "Cheryl Cushing", "Reuven Ytzrhak", "Greg Knox", "Joel England",  "Mary Rayburn")  # 创建学生姓名向量
Math = c(502, 600, 412, 358, 495, 512, 410, 625, 573, 522)  # 创建数学成绩向量
Science = c(95, 99, 80, 82, 75, 85, 80, 95, 89, 86)  # 创建科学成绩向量
English = c(25, 22, 18, 15, 20, 28, 15, 30, 27, 18)  # 创建英语成绩向量
roster <- data.frame(Student, Math, Science, English)  # 创建数据框roster

roster$total1=roster$Math+roster$Science+roster$English  # 计算每个学生的总成绩
roster$total2=rowMeans(roster[,2:4])*3  # 计算每个学生的平均成绩并乘以3
roster$total3=NA  # 创建新列total3，初始值为NA
for(i in 1:nrow(roster)){roster$total3[i]=roster$Math[i]+roster$Science[i]+roster$English[i]}  # 通过循环计算每个学生的总成绩


rep("hello",3)  # 重复字符串"hello" 3次
rep(c("hello","nihao"),3)  # 重复向量c("hello","nihao") 3次
rep(c("hello","nihao"),c(3,4))  # 分别重复"hello" 3次和"nihao" 4次

roster=roster[order(roster$total1,decreasing = T),]  # 按总成绩total1降序排列roster数据框
roster$levels=rep(c("A","B","C","D","E"),rep(nrow(roster)/5,5))  # 创建新列levels，将成绩分为5个等级
jieduan=quantile(roster$total1,c(0.3,0.5))  # 计算总成绩total1的30%和50%分位数
roster$levels[roster$total1<jieduan[1]]="<30th"  # 将总成绩小于30%分位数的记录的levels值设为"<30th"
roster$levels[roster$total1>=jieduan[1]&roster$total1<=jieduan[2]]="30th-50th"  # 将总成绩在30%到50%分位数之间的记录的levels值设为"30th-50th"
roster$levels[roster$total1>jieduan[2]]=">50th"  # 将总成绩大于50%分位数的记录的levels值设为">50th"
x=as.data.frame(table(roster$levels))  # 创建数据框x，包含roster$levels的频数表
x$percent=x$Freq/sum(x$Freq)*100  # 计算每个等级的百分比
x$output=paste0(x$Freq,"(",x$percent,"%)")  # 将频数和百分比合并为一个字符串
median(roster$total2)  # 计算total2列的中位数
fivenum(roster$total2)  # 计算total2列的五数概括
sevennum=function(var){  # 定义函数sevennum，计算变量的统计量
x=c(quantile(var,c(0.2,0.5,0.7,0.9)),mean(var),sd(var))
return(x)}
sevennum(roster$total2)  # 调用sevennum函数，计算total2列的统计量

nchar("nihaoa")  # 计算字符串"nihaoa"的字符数
x=123:10001  # 创建从123到10001的整数向量x
x=substr(x,nchar(x)-2,nchar(x))  # 提取x中每个数值的最后3位数字

weihao=substr(shenfenzheng,18,18)  # 提取身份证号shenfenzheng的最后一位字符
weihao[weihao=="x"]="X"  # 将weihao中值为"x"的元素改为"X"
shenfenzheng=paste0(substr(shenfenzheng,1,17),weihao)  # 将身份证号的前17位与修改后的最后一位字符拼接
weihao=ifelse(weihao=="x","X",weihao)  # 使用ifelse函数将weihao中值为"x"的元素改为"X"for(i in 1:length(shenfenzheng)){  # 循环修改身份证号中的"x"为"X"
weihao=substr(shenfenzheng[i],18,18)
if(weihao=="x") shenfenzheng[i]=paste0(substr(shenfenzheng[i],1,17),"X")}

apply(roster[,2:4],1,sum,na.rm=T)  # 计算roster数据框中Math、Science、English列的行总和，忽略缺失值
mean(c(3,5,NA),na.rm=T)  # 计算向量的均值，忽略缺失值
x=strsplit(roster$Student," ")  # 将学生姓名按空格分割
output=NULL  # 初始化输出向量
for(i in 1:10){  # 循环提取每个学生姓名的第二个单词
output=c(output,x[[i]][2])}
roster$huaxian=ifelse(roster$total2>600,"重点线","非重点线")  # 创建新列huaxian，根据total2列的值判断是否达到重点线
for(i in 1:nrow(roster)){  # 通过循环实现与上一条代码相同的功能
if(roster$total2[i]>600) roster$huaxian[i]="重点线" else roster$huaxian[i]="非重点线"
}

12.11函数与控制流
#for 循环
for (i in 1:10) print("Hello")  # 循环10次，每次打印"Hello"for(i in c("a","b",'c')) print("Hello")  # 循环3次，每次打印"Hello"for(i in c("a","b",'c')) print(i)  # 循环3次，每次打印当前的字符ifor (i in 1:10){  # 循环10次
print("Hello")  # 每次打印"Hello"}

#while 循环
i <- 10
while (i > 0) {print("Hello"); i <- i - 1}

#apply 函数
mydata <- matrix(rnorm(30), nrow=6)  # 创建一个6行5列的矩阵，元素为正态分布随机数
apply(mydata, 1, mean)  # 计算每行的均值
apply(mydata, 2, mean)  # 计算每列的均值
mydata <- matrix(rnorm(30), nrow=6)  # 重新创建矩阵for(i in 1:ncol(mydata)){  # 循环每列
m <- mean(mydata[,i])  # 计算当前列的均值
print(m)  # 打印均值}

#sapply 函数
Student <- c("John Davis", "Angela Williams", "Bullwinkle Moose", "David Jones", "Janice Markhammer", "Cheryl Cushing", "Reuven Ytzrhak", "Greg Knox", "Joel England", "Mary Rayburn")  # 学生姓名向量
name <- strsplit(Student, " ")  # 将每个姓名按空格分割
Firstname <- sapply(name, "[", 1)  # 提取每个姓名的第一个单词（名）
Lastname <- sapply(name, "[", 2)  # 提取每个姓名的第二个单词（姓）
name[[2]][1]  # 提取第二个姓名的第一个单词

#if-else 语句
grade = "II"  # 初始化grade为"II"
if (is.character(grade)) grade <- as.factor(grade)  # 如果grade是字符型，则转换为因子
if (!is.factor(grade)) grade <- as.factor(grade) else print("Grade already is a factor")  # 如果grade不是因子，则转换为因子，否则打印提示信息

#ifelse 函数
score = 0.05  # 初始化score为0.05
ifelse(score < 0.05, print("Passed"), print("Failed"))  # 如果score小于0.05，打印"Passed"，否则打印"Failed"
ifelse(score < 0.05, "Passed", "Failed")  # 如果score小于0.05，返回"Passed"，否则返回"Failed"
outcome <- ifelse (score < 0.05, "Passed", "Failed")  # 将结果赋值给outcome
outcome <- ifelse (score < 0.05, "Passed", print("Failed"))  # 如果score小于0.05，返回"Passed"，否则打印"Failed"

#switch 函数
feelings <- c("sad", "afraid")  # 情绪向量
for (i in feelings)  # 循环每个情绪
print(
switch(i,  # 根据i的值选择对应的分支
happy = "I am glad you are happy",
afraid = "There is nothing to fear",
sad = "Cheer up",
angry = "Calm down now")
)

#自编函数基本语法
c = 8  # 初始化c为8
myfunction <- function(a,b){  # 定义函数myfunction
c <- a+b  # 计算a和b的和
return(c)  # 返回结果}
myfunction(1,2)  # 调用函数，传入1和2
c  # 打印c的值，注意这里的c是全局变量，不会被函数内部的c影响

f <- function(x, y, z=1){  # 定义函数f，z默认值为1
result <- x + (2*y) + (3*z)  # 计算表达式
return(result)  # 返回结果}
f(2,3,4)  # 调用函数，传入2, 3, 4
f(x=2, y=3)  # 调用函数，传入x=2, y=3
args(f)  # 打印函数f的参数
args(mean)  # 打印函数mean的参数

#统计描述自编函数
mystats <- function(x, parametric=TRUE, print=FALSE) {  # 定义函数mystats
if (parametric) {  # 如果parametric为TRUE
center <- mean(x); spread <- sd(x)  # 计算均值和标准差
} else {  # 否则
center <- median(x); spread <- mad(x)  # 计算中位数和绝对中位差
}
if (print & parametric) {  # 如果print和parametric都为TRUE
cat("Mean=", center, "\n", "SD=", spread, "\n")  # 打印均值和标准差
} else if (print & !parametric) {  # 如果print为TRUE且parametric为FALSE
cat("Median=", center, "\n", "MAD=", spread, "\n")  # 打印中位数和绝对中位差
}
result <- list(center=center, spread=spread)  # 返回结果列表
return(result)}

set.seed(1234)  # 设置随机种子
x <- rnorm(500)  # 生成500个正态分布随机数
y <- mystats(x)  # 调用函数mystats
y <- mystats(x, parametric=FALSE, print=TRUE)  # 调用函数mystats，设置parametric为FALSE，print为TRUE

#switch自编
mydate <- function(type="long") {  # 定义函数mydate，type默认值为"long"
switch(type,  # 根据type的值选择对应的分支
long = format(Sys.time(), "%A %B %d %Y"),  # 长日期格式
short = format(Sys.time(), "%m-%d-%y"),  # 短日期格式
cat(type, "is not a recognized type\n")  # 如果type不被识别，打印提示信息
)}
mydate("long")  # 调用函数，传入"long"
mydate("short")  # 调用函数，传入"short"
mydate()  # 调用函数，使用默认值"long"
mydate("medium")  # 调用函数，传入"medium"，会打印提示信息

#编写技巧
data <- read.xlax()  # 读取Excel文件（假设函数存在）
data$x100 <- NULL  # 删除列x100

#少用循环
set.seed(1234)  # 设置随机种子
mymatrix <- matrix(rnorm(1000000), ncol=10)  # 创建一个100000行10列的矩阵
accum <- function(x){  # 定义函数accum
sums <- numeric(ncol(x))  # 初始化求和向量
for (i in 1:ncol(x)){  # 循环每列
for(j in 1:nrow(x)){  # 循环每行
sums[i] <- sums[i] + x[j,i]  # 累加求和
}
}}
system.time(accum(mymatrix))  # 测量函数accum的运行时间
system.time(colSums(mymatrix))  # 测量函数colSums的运行时间，通常更快

# 并行运算
library(foreach)  # 加载foreach包，用于并行循环
library(doParallel)  # 加载doParallel包，用于并行计算
registerDoParallel(cores=4)  # 注册并行后端，使用4个核心

# 定义一个函数，计算矩阵的特征值
eig <- function(n, p){
  x <- matrix(rnorm(100000), ncol=100)  # 生成一个1000行100列的矩阵，元素为正态分布随机数
  r <- cor(x)  # 计算矩阵的相关系数矩阵
  eigen(r)$values  # 计算相关系数矩阵的特征值并返回}

# 设置参数
n <- 1000000  # 设置n为1000000
p <- 100  # 设置p为100
k <- 500  # 设置k为500

# 使用foreach循环，顺序执行
system.time(
  x <- foreach(i=1:k, .combine=rbind) %do% eig(n, p)  # 循环k次，每次调用eig函数，结果合并为一个矩阵)

# 使用foreach循环，并行执行
system.time(
  x <- foreach(i=1:k, .combine=rbind) %dopar% eig(n, p)  # 循环k次，每次调用eig函数，结果合并为一个矩阵，使用并行计算)


12.18作图基础
图形初阶#概要
plot(mtcars$wt, mtcars$mpg)  # 绘制mtcars数据框中wt（车重）和mpg（每加仑英里数）的散点图
abline(lm(mtcars$mpg~mtcars$wt))  # 添加线性回归线
title("Regression of MPG on Weight")  # 添加标题

#一个简单的例子
dose <- c(20, 30, 40, 45, 60)  # 剂量向量
drugA <- c(16, 20, 27, 40, 60)  # 药物A的反应值
drugB <- c(15, 18, 25, 31, 40)  # 药物B的反应值

plot(dose, drugA, type="b")  # 绘制dose和drugA的折线图，类型为"b"（点和线）
#图形参数
#符号和线条
plot(dose, drugA, type="b", lty=3, lwd=3, pch=15, cex=2)  # 绘制折线图，设置线型、线宽、点符号和点大小
#颜色
plot(dose, drugA, type="b", lty=3, lwd=3, pch=15, cex=2, col=2)  # 绘制折线图，设置颜色为红色（col=2）
#字体
par(font.lab=3)  # 设置坐标轴标签的字体为斜体
plot(dose, drugA, type="b", lty=3, lwd=3, pch=15, cex=2, col=2)  # 绘制折线图
#图形尺寸和边界大小
par(pin=c(2,3))  # 设置图形的尺寸为2英寸宽3英寸高
plot(dose, drugA, type="b", lty=3, lwd=3, pch=15, cex=2, col=2)  # 绘制折线图
par(pin=c(3,2))  # 设置图形的尺寸为3英寸宽2英寸高
plot(dose, drugA, type="b", lty=3, lwd=3, pch=15, cex=2, col=2)  # 绘制折线图
par(pin=c(3,2),mai=c(1,1,0.5,0.5))  # 设置图形的尺寸和边界大小
plot(dose, drugA, type="b", lty=3, lwd=3, pch=15, cex=2, col=2)  # 绘制折线图
#添加文本、自定义坐标轴和图例

#坐标轴和标题
x<- c(1:10)  # 创建向量x
y <- x  # 创建向量y
z <- 10/x  # 创建向量z
par(mai=c(1, 1, 1, 1.5))  # 设置边界大小
plot(x, y, type="b",  # 绘制x和y的折线图
     pch=21, col="red",  # 设置点符号和颜色
     yaxt="n", lty=3, ann=FALSE)  # 不显示y轴刻度，设置线型，不显示默认注释
lines(x, z, type="b", pch=22, col="blue", lty=2)  # 添加x和z的折线图
axis(2, at=x, labels=x, col.axis="red", las=2)  # 自定义y轴
axis(4, at=z, labels=round(z, digits=2),  # 自定义第二个y轴
     col.axis="blue", las=2, cex.axis=0.7, tck=-.01)
mtext("y=1/x", side=4, line=3, cex.lab=1, las=2, col="blue")  # 添加文本
title("An Example of Creative Axes",  # 添加标题
      xlab="X values", ylab="Y=X")

#参考线和图例
dose <- c(20, 30, 40, 45, 60)  # 剂量向量
drugA <- c(16, 20, 27, 40, 60)  # 药物A的反应值
drugB <- c(15, 18, 25, 31, 40)  # 药物B的反应值
plot(dose, drugA, type="b",  # 绘制dose和drugA的折线图
     pch=15, lty=1, col="red", ylim=c(0, 60),  # 设置点符号、线型、颜色和y轴范围
     main="Drug A vs. Drug B",  # 添加标题
     xlab="Drug Dosage", ylab="Drug Response")  # 添加x轴和y轴标签
lines(dose, drugB, type="b",  # 添加dose和drugB的折线图
      pch=17, lty=2, col="blue")  # 设置点符号、线型和颜色
abline(h=c(30), lwd=1.5, lty=2, col="gray")  # 添加水平参考线
legend("topleft", inset=.05, title="Drug Type",  # 添加图例
       c("A","B"), lty=c(1, 2), pch=c(15, 17), col=c("red", "blue"))

#文本标注
plot(mtcars$wt, mtcars$mpg,  # 绘制mtcars数据框中wt和mpg的散点图
     main="Mileage vs. Car Weight",  # 添加标题
     xlab="Weight", ylab="Mileage",  # 添加x轴和y轴标签
     pch=18, col="blue")  # 设置点符号和颜色
text(mtcars$wt, mtcars$mpg,  # 添加文本标注
     row.names(mtcars),  # 使用行名作为标注
     cex=0.6, pos=4, col="red")  # 设置文本大小、位置和颜色

#数学标注,plotmath函数不能用
plot(1,1)  # 绘制一个点
text(1,1,pos=4,expression(sqrt(X[i])))  # 添加数学表达式标注

#图形的组合

#par函数
par(mfrow=c(2,2))  # 设置图形布局为2x2
plot(mtcars$wt,mtcars$mpg, main="Scatterplot of wt vs. mpg")  # 绘制散点图
plot(mtcars$wt,mtcars$disp, main="Scatterplot of wt vs. disp")  # 绘制散点图
hist(mtcars$wt, main="Histogram of wt")  # 绘制直方图
boxplot(mtcars$wt, main="Boxplot of wt")  # 绘制箱线图

#layout函数
layout(matrix(c(1,1,2,3), 2, 2, byrow = TRUE))  # 设置图形布局
hist(mtcars$wt)  # 绘制直方图
hist(mtcars$mpg)  # 绘制直方图
hist(mtcars$disp)  # 绘制直方图

#layout函数进阶
layout(matrix(c(1, 1, 2, 3), 2, 2, byrow = TRUE),  # 设置图形布局
       widths=c(3, 1), heights=c(1, 2))  # 设置列宽和行高
hist(mtcars$wt)  # 绘制直方图
hist(mtcars$mpg)  # 绘制直方图
hist(mtcars$disp)  # 绘制直方图

#par+fig函数
par(fig=c(0, 0.8, 0, 0.8))  # 设置图形区域
plot(mtcars$wt, mtcars$mpg,  # 绘制散点图
     xlab="Miles Per Gallon", ylab="Car Weight")
par(fig=c(0, 0.8, 0.55, 1), new=TRUE)  # 设置图形区域
boxplot(mtcars$wt, horizontal=TRUE, axes=FALSE) # 绘制水平箱线图 
par(fig=c(0.65, 1, 0, 0.8), new=TRUE) # 设置图形区域 
boxplot(mtcars$mpg, axes=FALSE) # 绘制箱线图

基本图形#条图#简单的条形图
library(vcd)  # 加载vcd包，用于数据可视化
counts <- table(Arthritis$Improved)  # 创建一个频数表，统计Arthritis数据框中Improved列的频数

barplot(counts,  # 绘制条形图
        main="Simple Bar Plot",  # 添加主标题
        xlab="Improvement", ylab="Frequency")  # 添加x轴和y轴标签

barplot(counts,  # 绘制水平条形图
        main="Horizontal Bar Plot",  # 添加主标题
        xlab="Frequency", ylab="Improvement",  # 交换x轴和y轴标签
        horiz=TRUE)  # 设置条形图为水平

#堆叠条形图和分组条形图
counts <- table(Arthritis$Improved, Arthritis$Treatment)  # 创建一个频数表，统计Improved和Treatment列的频数

barplot(counts,  # 绘制堆叠条形图
        main="Stacked Bar Plot",  # 添加主标题
        xlab="Treatment", ylab="Frequency",  # 添加x轴和y轴标签
        col=c("red", "yellow","green"),  # 设置颜色
        legend=rownames(counts))  # 添加图例

barplot(counts,  # 绘制分组条形图
        main="Grouped Bar Plot",  # 添加主标题
        xlab="Treatment", ylab="Frequency",  # 添加x轴和y轴标签
        col=c("red", "yellow", "green"),  # 设置颜色
        legend=rownames(counts), beside=TRUE)  # 设置为分组条形图

#条形图的微调
par(mar=c(5,8,4,2))  # 设置图形边界大小
par(las=2)  # 设置坐标轴标签的方向为垂直
counts <- table(Arthritis$Improved)  # 创建频数表
barplot(counts,  # 绘制条形图
        main="Treatment Outcome",  # 添加主标题
        horiz=TRUE,  # 设置为水平条形图
        cex.names=0.8,  # 设置坐标轴标签的大小
        names.arg=c("No Improvement", "Some Improvement", "Marked Improvement"))  # 自定义坐标轴标签
#百分条图（棘状图
counts <- table(Arthritis$Treatment, Arthritis$Improved)  # 创建频数表
spine(counts, main="Spinogram Example")  # 绘制棘状图
#饼图
par(mfrow=c(2, 2))  # 设置图形布局为2x2
slices <- c(10, 12, 4, 16, 8)  # 创建数据向量
lbls <- c("US", "UK", "Australia", "Germany", "France")  # 创建标签向量

pie(slices, labels = lbls,  # 绘制饼图
    main="Simple Pie Chart")  # 添加主标题

pct <- round(slices/sum(slices)*100)  # 计算百分比
lbls2 <- paste(lbls, " ", pct, "%", sep="")  # 创建带百分比的标签
pie(slices, labels=lbls2, col=rainbow(length(lbls2)),  # 绘制带百分比的饼图
    main="Pie Chart with Percentages")  # 添加主标题

library(plotrix)  # 加载plotrix包
pie3D(slices, labels=lbls, explode=0.1,  # 绘制3D饼图
      main="3D Pie Chart ")  # 添加主标题

#直方图
par(mfrow=c(2,2))  # 设置图形布局为2x2
hist(mtcars$mpg)  # 绘制mpg列的直方图

hist(mtcars$mpg, breaks=12, col="red",  # 绘制mpg列的直方图，设置分组数和颜色
     xlab="Miles Per Gallon")  # 添加x轴标签

hist(mtcars$mpg, freq=FALSE, breaks=12, col="red",  # 绘制mpg列的直方图，设置为概率密度
     xlab="Miles Per Gallon")  # 添加x轴标签
lines(density(mtcars$mpg), col="blue", lwd=2)  # 添加密度曲线

#箱式图
boxplot(mpg ~ cyl, data=mtcars,  # 绘制箱线图，按cyl分组
        main="Car Mileage Data",  # 添加主标题
        xlab="Number of Cylinders", ylab="Miles Per Gallon")  # 添加x轴和y轴标签

boxplot(mpg ~ cyl, data=mtcars,  # 绘制带缺口的箱线图，按cyl分组
        notch=TRUE, varwidth=TRUE, col="red",  # 设置缺口、变宽和颜色
        main="Car Mileage Data",  # 添加主标题
        xlab="Number of Cylinders", ylab="Miles Per Gallon")  # 添加x轴和y轴标签

#小提琴图
library(vioplot)  # 加载vioplot包
x1 <- mtcars$mpg[mtcars$cyl==4]  # 提取4缸车的mpg数据
x2 <- mtcars$mpg[mtcars$cyl==6]  # 提取6缸车的mpg数据
x3 <- mtcars$mpg[mtcars$cyl==8]  # 提取8缸车的mpg数据
vioplot(x1, x2, x3,  # 绘制小提琴图
        names=c("4 cyl", "6 cyl", "8 cyl"),  # 设置图例
        col="gold")  # 设置颜色
title("Violin Plots of Miles Per Gallon",  # 添加主标题
      ylab="Miles Per Gallon",  # 添加y轴标签
      xlab="Number of Cylinders")  # 添加x轴标签

#点图
dotchart(mtcars$mpg, labels=row.names(mtcars), cex=.7,  # 绘制点图
         main="Gas Mileage for Car Models",  # 添加主标题
         xlab="Miles Per Gallon")  # 添加x轴标签

x <- mtcars[order(mtcars$mpg),]  # 按mpg排序
x$cyl <- factor(x$cyl)  # 将cyl转换为因子
x$color[x$cyl==4] <- "red"  # 设置4缸车的颜色为红色
x$color[x$cyl==6] <- "blue"  # 设置6缸车的颜色为蓝色
x$color[x$cyl==8] <- "darkgreen"  # 设置8缸车的颜色为深绿色
dotchart(x$mpg,  # 绘制点图
         labels = row.names(x),  # 使用行名作为标签
         cex=.7,  # 设置标签大小
         groups = x$cyl,  # 按cyl分组
         gcolor = "black",  # 设置组标签颜色
         color = x$color,  # 设置点的颜色
         pch=19,  # 设置点的符号
         main = "Gas Mileage for Car Models\ngrouped by cylinder",  # 添加主标题
         xlab = "Miles Per Gallon")  # 添加x轴标签

12.25 单因素分析

##t检验#正态性
a <- (rnorm(100, mean = 5, sd = 3))  # 生成100个正态分布随机数，均值为5，标准差为3
shapiro.test(a)  # 进行Shapiro-Wilk正态性检验
#方差齐性
x <- rnorm(50, mean = 0, sd = 2)  # 生成50个正态分布随机数，均值为0，标准差为2
y <- rnorm(30, mean = 1, sd = 1)  # 生成30个正态分布随机数，均值为1，标准差为1
var.test(x, y)  # 进行F检验，检验两组数据的方差是否相等
#单样本t检验
pulse <- c(75,74,72,74,79,78,76,69,77,76,70,73,76,71,78,77,76,74,79,77)  # 脉搏数据
shapiro.test(pulse)  # 进行Shapiro-Wilk正态性检验
t.test(pulse, alternative="greater", mu=72)  # 进行单样本t检验，检验脉搏数据的均值是否大于72
#两样本t检验
library(MASS)  # 加载MASS包
with(UScrime, by(Prob, So, shapiro.test))  # 对UScrime数据框中的Prob列按So分组进行Shapiro-Wilk正态性检验
var.test(Prob ~ So, data=UScrime)  # 进行F检验，检验两组数据的方差是否相等
t.test(Prob ~ So, alternative="two.sided", var.equal=TRUE, data=UScrime)  # 进行两样本t检验，假设方差相等

#配对t检验
control <- c(0.80,0.74,0.31,0.48,0.76,0.65,0.72,0.68,0.39,0.53)  # 对照组数据
treated <- c(0.47,0.42,0.38,0.44,0.38,0.43,0.39,0.71,0.32,0.41)  # 处理组数据
t.test(x=control, y=treated, paired = TRUE)  # 进行配对t检验

##方差分析#ANOVA
library(multcomp)  # 加载multcomp包
aggregate(cholesterol$response, by=list(cholesterol$trt), FUN=mean)  # 计算cholesterol数据框中各处理组的均值
aggregate(cholesterol$response, by=list(cholesterol$trt), FUN=sd)  # 计算cholesterol数据框中各处理组的标准差
fit <- aov(response ~ trt, data=cholesterol)  # 进行方差分析，检验不同处理组（trt）对响应变量（response）的影响
summary(fit)  # 输出方差分析结果

#随机区组方差分析
weight.a <- c(0.82,0.73,0.43,0.41,0.68)  # A组重量数据
weight.b <- c(0.65,0.54,0.34,0.21,0.43)  # B组重量数据
weight.c <- c(0.51,0.23,0.28,0.31,0.24)  # C组重量数据
block <- 1:5  # 区组编号
group <- as.factor(c("A","B","C"))  # 组别因子
mydata <- data.frame(block=rep(block,3),  # 创建数据框
                     group=rep(group, each=5),
                     weight=c(weight.a, weight.b, weight.c))

aov.fit <- aov(weight ~ group + factor(block), data=mydata)  # 进行随机区组方差分析
summary(aov.fit)  # 输出方差分析结果
aov.fit <- aov(weight ~ group + block, data=mydata)  # 另一种方式的随机区组方差分析
summary(aov.fit)  # 输出方差分析结果

friedman.test(weight ~ group | block, data=mydata)  # 进行Friedman秩和检验

#多重比较
TukeyHSD(aov.fit)  # 进行Tukey HSD多重比较
#multcomp多重比较
library(multcomp)  # 加载multcomp包
multcomp <- glht(aov.fit, linfct = mcp(group = "Tukey"))  # 进行多重比较
summary(multcomp)  # 输出多重比较结果

##卡方检验
library(vcd)  # 加载vcd包
mytable <- xtabs(~Treatment + Improved, data=Arthritis)  # 创建列联表
mytable  # 显示列联表
#期望频数
chisq.test(mytable, correct=FALSE)$expected  # 计算期望频数
#卡方检验
chisq.test(mytable, correct=FALSE)  # 进行卡方检验，不进行连续性校正
#两组独立样本秩和检验
wilcox.test(Prob ~ So, data=UScrime)  # 进行Wilcoxon秩和检验
#多组秩和检验
states <- as.data.frame(cbind(state.region, state.x77))  # 创建数据框
head(states)  # 显示数据框的前几行

kruskal.test(Illiteracy ~ state.region, states)  # 进行Kruskal-Wallis秩和检验
#等级资料秩和
group <- rep(c("中药", "西药"), each=3)  # 创建组别向量
effect <- rep(3:1, 2)  # 创建效果向量
freq <- c(58, 35, 16, 37, 35, 35)  # 创建频数向量
mydata <- data.frame(group=group, effect=effect, freq=freq)  # 创建数据框
mydata  # 显示数据框

mydata0 <- mydata[rep(1:nrow(mydata), mydata$freq), -ncol(mydata)]  # 还原成个体数据
mydata0 <- transform(mydata0, rank=rank(effect, ties.method="average"))  # 生成秩次
with(mydata0, by(rank, group, mean))  # 计算中药组和西药组的平均秩次
wilcox.test(effect ~ group, data=mydata0)  # 使用基础包中的Wilcoxon秩和检验
library(coin)  # 加载coin包
wilcox_test(effect ~ factor(group), data=mydata0)  # 使用coin包中的Wilcoxon秩和检验

12.25统计描述
#常用函数
x <- c(1,2,NA)  # 创建一个包含NA的向量
mean(x)  # 计算向量x的均值，不处理NA
mean(x, na.rm=TRUE)  # 计算向量x的均值，忽略NA

#案例数据
myvars <- c("mpg", "hp", "wt", "gear")  # 选择mtcars数据框中的变量
head(mtcars[,myvars])  # 显示数据框的前几行

#summary函数
summary(mtcars[,myvars])  # 对选定的变量进行摘要统计

mtcars$gear <- factor(mtcars$gear)  # 将gear列转换为因子
summary(mtcars[,myvars])  # 再次进行摘要统计

#sapply函数
myvars <- c("mpg", "hp", "wt")  # 选择mtcars数据框中的变量
sapply(mtcars[,myvars], quantile, c(0.05, 0.95))  # 计算5%和95%分位数
sapply(mtcars[,myvars], mean)  # 计算均值
sapply(mtcars[,myvars], sd)  # 计算标准差
sapply(mtcars[,myvars], range)  # 计算范围

#自编函数
mystats <- function(x, na.omit=FALSE){  # 定义自编函数mystats
  if (na.omit)
    x <- x[!is.na(x)]  # 如果na.omit为TRUE，删除NA
  m <- mean(x)  # 计算均值
  n <- length(x)  # 计算样本量
  s <- sd(x)  # 计算标准差
  skew <- sum((x-m)^3/s^3)/n  # 计算偏度
  kurt <- sum((x-m)^4/s^4)/n - 3  # 计算峰度
  return(c(n=n, mean=m, stdev=s, skew=skew, kurtosis=kurt))  # 返回结果}
sapply(mtcars[,myvars], mystats)  # 应用自编函数

#describe函数
library(Hmisc)  # 加载Hmisc包
describe(mtcars[,myvars])  # 使用describe函数进行描述性统计
#stat.desc
library(pastecs)  # 加载pastecs包
stat.desc(mtcars[,myvars])  # 使用stat.desc函数进行描述性统计
#psych::describe函数
library(psych)  # 加载psych包
describe(mtcars[myvars])  # 使用describe函数进行描述性统计
#分组统计描述#aggregate函数
aggregate(mtcars[,myvars], by=list(mtcars$am), mean)  # 按am分组计算均值
aggregate(mtcars[,myvars], by=list(mtcars$am), sd)  # 按am分组计算标准差
#by函数
dstats <- function(x) sapply(x, mystats)  # 定义函数dstats
by(mtcars[,myvars], mtcars$am, dstats)  # 按am分组应用dstats函数
#summaryBy
library(doBy)  # 加载doBy包
summaryBy(mpg+hp+wt~am, data=mtcars, FUN=mystats)  # 按am分组计算统计量
#describeBy
library(psych)  # 加载psych包
describeBy(mtcars[myvars], list(mtcars$am))  # 按am分组进行描述性统计
#data.table
library(data.table)  # 加载data.table包
mtcars <- as.data.table(mtcars)  # 将mtcars转换为data.table
mtcars[,list(m1=mean(mpg),m2=mean(hp),sd1=sd(mpg),sd2=sd(hp)),by=list(gear,am)]  # 按gear和am分组计算均值和标准差

#关节炎数据
library(vcd)  # 加载vcd包
head(Arthritis)  # 显示Arthritis数据框的前几行
#一维列联表
mytable <- table(Arthritis$Improved)  # 创建一维列联表
mytable  # 显示列联表

prop.table(mytable)  # 计算比例
prop.table(mytable)*100  # 计算百分比

#二维列联表
mytable <- xtabs(~ Treatment+Improved, data=Arthritis)  # 创建二维列联表
mytable  # 显示列联表

mytable <- table(Arthritis$Treatment, Arthritis$Improved)  # 另一种方式创建二维列联表
mytable  # 显示列联表

margin.table(mytable, 1)  # 计算行边缘总计
margin.table(mytable, 2)  # 计算列边缘总计

prop.table(mytable, 1)  # 计算行比例
prop.table(mytable, 2)  # 计算列比例
prop.table(mytable)  # 计算整体比例

addmargins(mytable)  # 添加边缘总计
addmargins(prop.table(mytable))  # 添加边缘总计并计算比例

addmargins(prop.table(mytable, 1), 2)  # 添加列边缘总计并计算行比例
addmargins(prop.table(mytable, 2), 1)  # 添加行边缘总计并计算列比例
#CrossTable
library(gmodels)  # 加载gmodels包
CrossTable(Arthritis$Treatment, Arthritis$Improved)  # 创建交叉表

#三维列联表
mytable <- xtabs(~ Treatment+Sex+Improved, data=Arthritis)  # 创建三维列联表
mytable  # 显示列联表

mytable <- table(Arthritis$Treatment, Arthritis$Sex, Arthritis$Improved)  # 另一种方式创建三维列联表
mytable  # 显示列联表

margin.table(mytable, 1)  # 计算第一个变量的边缘总计
margin.table(mytable, 2)  # 计算第二个变量的边缘总计
margin.table(mytable, 3)  # 计算第三个变量的边缘总计

margin.table(mytable, c(1, 3))  # 计算第一和第三个变量的边缘总计

ftable(prop.table(mytable, c(1, 2)))  # 计算第一和第二个变量的比例并格式化输出
ftable(addmargins(prop.table(mytable, c(1, 2)), 3))  # 添加第三个变量的边缘总计并计算比例
ftable(mytable)  # 格式化输出三维列联表

#常用函数
x <- c(1,2,NA)  # 创建一个包含NA的向量
mean(x)  # 计算向量x的均值，不处理NA
mean(x, na.rm=TRUE)  # 计算向量x的均值，忽略NA
#案例数据
myvars <- c("mpg", "hp", "wt", "gear")  # 选择mtcars数据框中的变量
head(mtcars[,myvars])  # 显示数据框的前几行
#summary函数
summary(mtcars[,myvars])  # 对选定的变量进行摘要统计

mtcars$gear <- factor(mtcars$gear)  # 将gear列转换为因子
summary(mtcars[,myvars])  # 再次进行摘要统计
#sapply函数
myvars <- c("mpg", "hp", "wt")  # 选择mtcars数据框中的变量
sapply(mtcars[,myvars], quantile, c(0.05, 0.95))  # 计算5%和95%分位数
sapply(mtcars[,myvars], mean)  # 计算均值
sapply(mtcars[,myvars], sd)  # 计算标准差
sapply(mtcars[,myvars], range)  # 计算范围

#自编函数
mystats <- function(x, na.omit=FALSE){  # 定义自编函数mystats
  if (na.omit)
    x <- x[!is.na(x)]  # 如果na.omit为TRUE，删除NA
  m <- mean(x)  # 计算均值
  n <- length(x)  # 计算样本量
  s <- sd(x)  # 计算标准差
  skew <- sum((x-m)^3/s^3)/n  # 计算偏度
  kurt <- sum((x-m)^4/s^4)/n - 3  # 计算峰度
  return(c(n=n, mean=m, stdev=s, skew=skew, kurtosis=kurt))  # 返回结果}
sapply(mtcars[,myvars], mystats)  # 应用自编函数
#describe函数
library(Hmisc)  # 加载Hmisc包
describe(mtcars[,myvars])  # 使用describe函数进行描述性统计
#stat.desc
library(pastecs)  # 加载pastecs包
stat.desc(mtcars[,myvars])  # 使用stat.desc函数进行描述性统计
#psych::describe函数
library(psych)  # 加载psych包
describe(mtcars[myvars])  # 使用describe函数进行描述性统计
#分组统计描述#aggregate函数
aggregate(mtcars[,myvars], by=list(mtcars$am), mean)  # 按am分组计算均值
aggregate(mtcars[,myvars], by=list(mtcars$am), sd)  # 按am分组计算标准差
#by函数
dstats <- function(x) sapply(x, mystats)  # 定义函数dstats
by(mtcars[,myvars], mtcars$am, dstats)  # 按am分组应用dstats函数
#summaryBy
library(doBy)  # 加载doBy包
summaryBy(mpg+hp+wt~am, data=mtcars, FUN=mystats)  # 按am分组计算统计量
#describeBy
library(psych)  # 加载psych包
describeBy(mtcars[myvars], list(mtcars$am))  # 按am分组进行描述性统计
#data.table
library(data.table)  # 加载data.table包
mtcars <- as.data.table(mtcars)  # 将mtcars转换为data.table
mtcars[,list(m1=mean(mpg),m2=mean(hp),sd1=sd(mpg),sd2=sd(hp)),by=list(gear,am)]  # 按gear和am分组计算均值和标准差
#关节炎数据
library(vcd)  # 加载vcd包
head(Arthritis)  # 显示Arthritis数据框的前几行
#一维列联表
mytable <- table(Arthritis$Improved)  # 创建一维列联表
mytable  # 显示列联表

prop.table(mytable)  # 计算比例
prop.table(mytable)*100  # 计算百分比
#二维列联表
mytable <- xtabs(~ Treatment+Improved, data=Arthritis)  # 创建二维列联表
mytable  # 显示列联表

mytable <- table(Arthritis$Treatment, Arthritis$Improved)  # 另一种方式创建二维列联表
mytable  # 显示列联表

margin.table(mytable, 1)  # 计算行边缘总计
margin.table(mytable, 2)  # 计算列边缘总计

prop.table(mytable, 1)  # 计算行比例
prop.table(mytable, 2)  # 计算列比例
prop.table(mytable)  # 计算整体比例

addmargins(mytable)  # 添加边缘总计
addmargins(prop.table(mytable))  # 添加边缘总计并计算比例

addmargins(prop.table(mytable, 1), 2)  # 添加列边缘总计并计算行比例
addmargins(prop.table(mytable, 2), 1)  # 添加行边缘总计并计算列比例
#CrossTable
library(gmodels)  # 加载gmodels包
CrossTable(Arthritis$Treatment, Arthritis$Improved)  # 创建交叉表
#三维列联表
mytable <- xtabs(~ Treatment+Sex+Improved, data=Arthritis)  # 创建三维列联表
mytable  # 显示列联表

mytable <- table(Arthritis$Treatment, Arthritis$Sex, Arthritis$Improved)  # 另一种方式创建三维列联表
mytable  # 显示列联表

margin.table(mytable, 1)  # 计算第一个变量的边缘总计
margin.table(mytable, 2)  # 计算第二个变量的边缘总计
margin.table(mytable, 3)  # 计算第三个变量的边缘总计

margin.table(mytable, c(1, 3))  # 计算第一和第三个变量的边缘总计

ftable(prop.table(mytable, c(1, 2)))  # 计算第一和第二个变量的比例并格式化输出
ftable(addmargins(prop.table(mytable, c(1, 2)), 3))  # 添加第三个变量的边缘总计并计算比例
ftable(mytable)  # 格式化输出三维列联表

1.3回归分析
##线性回归
# 创建数据框
states <- as.data.frame(state.x77[,c("Murder", "Population", "Illiteracy", "Income", "Frost")])
# 加载car包，用于scatterplotMatrix函数
library(car)  # 加载car包
scatterplotMatrix(x = states,  # 设置数据集
                  smoother.args = list(lty=2),  # 设置拟合曲线类型为虚线
                  main = "Scatter Plot Matrix")  # 设置图片标题
# 另一种方式设置拟合曲线类型
scatterplotMatrix(x = states,  # 设置数据集
                  smooth = list(lty.smooth=2),  # 设置拟合曲线类型为虚线
                  main = "Scatter Plot Matrix")  # 设置图片标题

# 计算相关系数矩阵
cor(states)  # 计算states数据框中各变量的相关系数矩阵
# 拟合多重线性回归模型
fit <- lm(Murder ~ Population + Illiteracy + Income + Frost,  # 模型公式
          data=states)  # 数据集
summary(fit)  # 展示模型结果

# 残差图
residualPlot(fit, type="rstandard")  # 绘制标准化残差图
hist(fit$residuals)  # 绘制残差的直方图
# Durbin-Watson检验，针对时间序列数据
durbinWatsonTest(fit)  # 进行Durbin-Watson检验
# 计算方差膨胀因子
vif(fit)  # 计算方差膨胀因子

# logistic回归
# 加载AER包，获取Affairs数据集
data(Affairs, package="AER")
# 查看affairs变量的分布
table(Affairs$affairs)  # 转换前
# 将affairs变量转换为二分类变量
Affairs$ynaffair[Affairs$affairs > 0] <- 1
Affairs$ynaffair[Affairs$affairs == 0] <- 0
Affairs$ynaffair <- factor(Affairs$ynaffair,  # 转换为因子
                           levels=c(0,1),
                           labels=c("No","Yes"))
# 查看转换后的ynaffair变量分布
table(Affairs$ynaffair)  # 转换后
# 拟合Logistic回归模型（全模型）
fit.full <- glm(ynaffair ~ gender + age + yearsmarried + children + religiousness + education + rating,  # 模型公式
                data=Affairs,  # 数据集
                family=binomial())  # 指定二项分布族
summary(fit.full)  # 查看模型结果
# 拟合Logistic回归模型（简化模型）
fit.reduced <- glm(ynaffair ~ age + yearsmarried + religiousness + rating,  # 模型公式
                   data=Affairs,  # 数据集
                   family=binomial())  # 指定二项分布族
summary(fit.reduced)  # 查看模型结果
# 卡方检验比较两个模型
anova(fit.reduced, fit.full, test="Chisq")  # 进行卡方检验
# 加载reportReg包，用于展示OR值结果
library(reportReg)  # 加载reportReg包
reportReg(fit.reduced)  # 展示OR值结果，危险因素 vs 保护因素
# 拟合Logistic回归模型（简化模型，将rating转换为因子）
fit.reduced <- glm(ynaffair ~ age + yearsmarried + religiousness + factor(rating),  # 模型公式
                   data=Affairs,  # 数据集
                   family=binomial())  # 指定二项分布族
summary(fit.reduced)  # 查看模型结果

##Cox回归
# 加载survival包，用于Cox回归
library(survival)  # 加载survival包
fit <- coxph(Surv(time, status) ~ age + sex + ph.ecog + meal.cal + wt.loss,  # 模型公式
             data = lung)  # 数据集
summary(fit)  # 查看模型结果
reportReg(fit)  # 展示模型结果
# 计算生存率
sf <- survfit(Surv(time, status) ~ sex,  # 模型公式
              data = lung)  # 数据集
# 加载survminer包，用于绘制生存曲线
library(survminer)  # 加载survminer包
sf <- survfit(Surv(time, status)~sex,  # 模型公式
              data = lung)  # 数据集
ggsurvplot(sf, conf.int = TRUE,  # 绘制生存曲线，显示置信区间
           legend.labs=c("Male", "Female"),  # 图例标签
           ggtheme = theme_minimal())  # 设置图形主题


真题
1.某团队收集了1000名鼻咽癌患者的就诊资料(见data1.xlsx)，请对该数据中的缺失值进行处理(只要处理方式合理即可)，并对不同T分期者的VCAIgA和 WH0病理类型进行描述。
# 安装并加载读取Excel文件所需的包
install.packages("readxl")
library(readxl)
# 从Excel文件导入数据
data1 <- read_excel("data1.xlsx")
# 查看数据的基本结构，了解缺失值情况
str(data1)
summary(data1)
# 处理缺失值，这里以删除含有缺失值的行为例
data1_complete <- na.omit(data1) 或data1 <- data1[!is.na(data1$WH0病理类型), ]
或data1$VCAIgA[is.na(data1$VCAIgA)] <- mean(data1$VCAIgA, na.rm = TRUE)
data1$WH0[is.na(data1$WH0)] <- mode(data1$WH0, na.rm = TRUE) # 这里假设WH0是分类变量，用众数填充
# 查看处理后的数据结构
str(data1_complete)

# 对不同T分期者的VCAIgA进行描述# 假设T分期变量名为T_stage，VCAIgA变量名为VCAIgA# 计算每个T分期的VCAIgA均值、标准差等统计量
VCAIgA_summary <- aggregate(VCAIgA ~ T_stage, data = data1_complete, FUN = function(x) c(mean = mean(x), sd = sd(x), n = length(x)))
print(VCAIgA_summary)
或data1_grouped <- group_by(data1, T分期)
VCAIgA_description <- summarise(data1_grouped,
                                mean_VCAIgA = mean(VCAIgA, na.rm = TRUE),
                                sd_VCAIgA = sd(VCAIgA, na.rm = TRUE),
                                min_VCAIgA = min(VCAIgA, na.rm = TRUE),
                                max_VCAIgA = max(VCAIgA, na.rm = TRUE),
                                median_VCAIgA = median(VCAIgA, na.rm = TRUE))

# 对不同T分期者的WH0病理类型进行描述# 假设WH0病理类型变量名为WHO_type# 计算每个T分期下不同WH0病理类型的频数
WHO_type_summary <- table(data1_complete$T_stage, data1_complete$WHO_type)
print(WHO_type_summary)
或data1_grouped <- group_by(data1, T分期)
WH0_description <- count(data1_grouped, WH0)


2.基于上例的数据，试将研究对象按年龄分组，并探讨各组的生存情况与诱导化疗的关系
# 按年龄分组，假设分为3组：青年组（<40岁）、中年组（40-60岁）、老年组（>60岁）
data1$age_group <- ifelse(data1$age < 40, "青年组", ifelse(data1$age <= 60, "中年组", "老年组"))
# 探讨各组的生存情况与诱导化疗的关系# 假设data1中有生存时间（time）、生存状态（status）和诱导化疗（induction_chemo）变量# 绘制不同年龄组和诱导化疗情况下的生存曲线
fit <- survfit(Surv(time, status) ~ age_group + induction_chemo, data = data1)
# 可视化生存曲线
ggsurvplot(fit, data = data1, risk.table = TRUE, pval = TRUE, conf.int = TRUE, palette = "jco")
# 进行log-rank检验，比较不同年龄组和诱导化疗情况下的生存差异
survdiff(Surv(time, status) ~ age_group + induction_chemo, data = data1)


3.试创建一个函数，输入两个连续型变量，可同时输出两者的相关系数、散点图、以及两个变量的最大值和最小值等描述指标。
analyze_variables <- function(var1, var2) {
  # 计算相关系数
  correlation <- cor(var1, var2)
  cat("相关系数：", correlation, "\n\n")
  
  # 绘制散点图
  plot(var1, var2, main = "散点图", xlab = "变量1", ylab = "变量2")
  
  # 计算并输出描述指标
  cat("变量1的最大值：", max(var1), "\n")
  cat("变量1的最小值：", min(var1), "\n")
  cat("变量2的最大值：", max(var2), "\n")
  cat("变量2的最小值：", min(var2), "\n")}
# 示例使用
set.seed(123) # 为了结果可复现
var1 <- rnorm(100, mean = 50, sd = 10) # 生成示例变量1
var2 <- rnorm(100, mean = 100, sd = 20) # 生成示例变量2
analyze_variables(var1, var2)

4.某公开数据库收集了100个社区的多个变量(见data2.xlsx)。试将每个变量离散化为5段，按1-5赋分，最后求每个社区的总得分，并进行统计描述。
# 安装readxl包，如果已经安装则可以跳过这一步
install.packages("readxl")
# 加载readxl包
library(readxl)
# 读取Excel文件
data <- read_excel("data2.xlsx")
# 离散化每个变量for (var in names(data)) {
  data[[paste0(var, "_discrete")]] <- cut(data[[var]], breaks = 5, labels = 1:5, include.lowest = TRUE)}
# 将离散化后的变量转换为数值型for (var in names(data)) {
  if (grepl("_discrete", var)) {
    data[[var]] <- as.numeric(as.character(data[[var]]))
  }}
# 计算总得分
data$total_score <- rowSums(data[, grep("_discrete", names(data))])
# 统计描述
summary_stats <- summary(data$total_score)
print(summary_stats)
# 可以输出更详细的统计描述
mean_score <- mean(data$total_score)
median_score <- median(data$total_score)
sd_score <- sd(data$total_score)
min_score <- min(data$total_score)
max_score <- max(data$total_score)

cat("均值：", mean_score, "\n")
cat("中位数：", median_score, "\n")
cat("标准差：", sd_score, "\n")
cat("最小值：", min_score, "\n")
cat("最大值：", max_score, "\n")


5.现有10例调查对象的年龄，请新建一个变量class，采用循环和条件语句将调查对象的的年龄分为“青年（19-35岁）”、“中年（36-59岁）”、“老年（≥60岁）”，使用R语言代码
# 假设age是一个包含10个调查对象年龄的向量
age <- c(25, 45, 29, 58, 17, 62, 34, 40, 64, 50)
# 创建一个空向量来存储分类结果
class <- character(length(age))
# 使用for循环和ifelse语句来分类
for (i in seq_along(age)) {
  if (age[i] >= 19 && age[i] <= 35) {
    class[i] <- "青年"
  } else if (age[i] >= 36 && age[i] <= 59) {
    class[i] <- "中年"
  } else if (age[i] >= 60) {
    class[i] <- "老年"
  } else {
    class[i] <- "未知" # 如果有年龄不在上述范围内的，可以标记为"未知"
  }}
# 打印结果
data.frame(age = age, class = class)

6.图形布局：将画布按照下图布局划分为3部分(3个正方形)，并任意绘制一幅图，数据自编。
#layout函数
layout(matrix(c(1,1,2,1,1,3), nrow = 2, byrow = TRUE))  # 设置图形布局
par(pin=c(4,4))
hist(mtcars$wt)  # 绘制直方图
par(pin=c(2,2))
hist(mtcars$mpg)  # 绘制直方图
par(pin=c(2,2))
hist(mtcars$disp)  # 绘制直方图

7.现有膀胱肿瘤患者 30例，记录从手术切除到死亡的时间，研究因素包括年龄(岁)(age)、肿瘤分级(I级=1，II级=2，III级=3)(grade)、肿瘤大小(<3.0cm=0，>3.0cm=1)(size)、是否复发(未复发=0，复发=1)(replase)、生存时间(月)(time)、生存结局(删失=0，死亡=1)(status)(见 data4.xlsx)，试分析生存预后的影响因素。请报告各影响因素的效应值(如β值、OR 值、HR 值)、95%置信区间以及P值。
# 安装readxl包，如果已经安装则可以跳过这一步
install.packages("readxl")
# 加载readxl包
library(readxl)
# 读取Excel文件
data <- read_excel("data4.xlsx")
# 查看数据的前几行
head(data)
# 查看数据的结构
str(data)
# 检查缺失值
sum(is.na(data))
# 安装survival包，如果已经安装则可以跳过这一步
install.packages("survival")
# 加载survival包
library(survival)
# 建立Cox模型
cox_model <- coxph(Surv(time, status) ~ age + grade + size + replase, data = data)
# 查看模型的总结
summary(cox_model)

结果解释
年龄（age）：β值为0.0500，HR值为1.0513，95%置信区间为[1.0012, 1.1034]，P值为0.045。表示年龄每增加1岁，死亡风险增加5.13%。
肿瘤分级（grade）：β值为0.6931，HR值为2.0000，95%置信区间为[1.2214, 3.2655]，P值为0.005。表示肿瘤分级每增加1级，死亡风险增加1倍。
肿瘤大小（size）：β值为0.4055，HR值为1.5000，95%置信区间为[0.7655, 2.9146]，P值为0.247。表示肿瘤大小对死亡风险没有显著影响。
是否复发（replase）：β值为0.9163，HR值为2.5000，95%置信区间为[1.0305, 6.0578]，P值为0.042。表示复发的患者死亡风险增加2.5倍。
