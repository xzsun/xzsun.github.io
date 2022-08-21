# Dataframe Manipulate

#### 按照行名称取行
```
select_row <- c("row1","row2","row5")
df[select_row ,]
```

#### 时间选择
```
rm(list=ls())
timestart <- Sys.time()
date <- format(Sys.time(),format="%Y%m%d_%H%M%S")
setwd('C:\\Users\\zhan\\Desktop')

gr <- read.xlsx("ta_select.xlsx", sheet=1,
colNames=T, rowNames=F, check.names=FALSE)
```

#### 宽数据变长数据：key为变长数据的指标列的列名，value为这些指标数值列的列名，：为要合并的指标列
```
dat <- gather(da,key="OTUID",value="va",OTU3062:OTU1523) #@#@#@#@#@

pab_long <- gather(species_top10, key ='tr',value='va',-taxon)
```

#### 使用melt 函数将宽数据gd1转换为长数据gd1_long1
```
gd1_long1<-melt(gd1,
id.vars = c('地区'),#需要保留不参与聚合的变量,
measure.vars = c('X2015年','X2006年')#用于聚合的变量,
variable.name='year',
value.name='gdp')
#ps: id_vars和 measure.vars只需要制定一个即可;另外一个默认是除指定的变量外的所有变量
```

#### 使用spread函数将gd1_long长数据转换为宽数据gd1_wide
```
#year为需要分解的变量，gdp为分解后的列的取值
gd1_wide <- spread(gd1_long,year,gdp) 
```

#### 使用dcast函数将gd1_long长数据转换为宽数据gd1_wide1

gd1_wide1<-dcast(gd1_long1,地区~gd1_long1$year,value.var = 'gdp')

#### 选取数据框da的SampleID列的第一个字符组成crop列
```
da$crop <- substr(da$SampleID,1,1)
```

#### 筛选Host.Descripton列的数据，并筛选为Host.Descripton_select的数据
```
Host.Descripton_select <- c(na.omit(group$Host.Descripton_select))
library(dplyr)
dat1 <- dat %>% group_by(Host.Descripton) %>% filter(Host.Descripton %in% Host.Descripton_select)

group <- group[which(group$compart==co),]
```

#### 直接删除特定列名的列，df为数据框，z,u为列名
```
subset(df,select=-c(z,u))
df[, !colnames(df) %in% c("z","u")]
```

#### 直接删除特定行名的行

```R
#删除n行的处理方式
df[!rownames(df) %in% c("n"), ]
```


#### 统计df中，compart，crop为主的pattern,fertilizer的情况
```
library(dplyr)
datachange <- df %>%
group_by(compart, crop) %>%
summarize(处理=paste(pattern,fertilizer,collapse=','))
data.frame(datachange)
```

#### 交互式操作
```
install.packages("Rcmdr", dependencies=TRUE)
library("Rcmdr")
```

#### 数据正态性转换
```
shapiro.test(log(datN$va))$p.value %>% round(4) #对数转换
shapiro.test(sqrt(datN$va))$p.value %>% round(4) #平方根变换
shapiro.test((datN$va)^(1/3))$p.value %>% round(4) #立方根变换
shapiro.test((datN$va)^(2))$p.value %>% round(4) #平方变换
shapiro.test((datN$va)^(3))$p.value %>% round(4) #立方变换
shapiro.test(-1/(datN$va))$p.value %>% round(4) #负倒数变换
shapiro.test(cos(datN$va))$p.value %>% round(4) #反弦变换
```

#### 修改dataframe中指定列的名称,通用修改方式
```
names(data1)[names(data1) == 'f1'] <- 'Compart'
```

#### devtools::install_github("tomwenseleers/export")
library(export)

#### 导出图形对象可编辑的ppt文件
```
graph2ppt(x=p, file=paste(fname,"相对丰度_",date,".pptx",sep = ""),
aspectr=2,width=8.4,append = TRUE)
#graph2doc(x=p, file=paste(fname,"相对丰度_",date,".docx",sep = ""), aspectr=0.8, append =TRUE)

dataov <- separate(data = dat, col = tr, into = c("f1","f2","f3","f4"), sep = "_")
```

#### Generate ANOVA output
```
fit=aov(va ~ f1*f2*f3*f4, data = dataov) # 'npk' dataset from base 'datasets'
x=summary(fit)[[1]]
```

#### 替换字符串中部分字符
```
rownames(x) <- gsub("f1", "Compart", rownames(x))
rownames(x) <- gsub("f2", "Crop", rownames(x))
rownames(x) <- gsub("f3", "Pattern", rownames(x))
rownames(x) <- gsub("f4", "Fertilization", rownames(x))
```

#### 根据某一列删除数据框中重复值
```
annotation_row <- annotation_row %>% distinct(family, .keep_all = TRUE)
```

#### 选tr列的前两个字符组成inoculum列
```
data1$inoculum <- substr(data1$tr,1,2)
```
#### 合并两个数据框，da与gr合并
```
dat <- merge(da,gr,by.x = "SampleID",by.y = "SampleID",all = FALSE) #@#@#@#@#@
```

#### 合并数据框中的某两列
```
data1 <- unite(data1, "trindi", tr, indicator, sep = "", remove = FALSE)

separate()
```

#### 合并数据框中的某几个连续的列

data <- unite(data,"tr", (Pattern:PlantingHistory), sep="", remove = FALSE)

#### 合并数据框r、P、p.adj的所有行,要求合并的数据框必须有相同的变量

dat <- rbind(r,P,p.adj) #@#@#@#@#@

#### 查看因子水平

levels(factor(data$OTUID))

pacman::p_load(openxlsx,tidyverse)
ot <- read.xlsx("network.xlsx", sheet=2, colNames=T, rowNames=T, check.names=FALSE)
gr <- read.xlsx("network.xlsx", sheet=3, colNames=T, rowNames=T, check.names=FALSE)
ta <- read.xlsx("network.xlsx", sheet=5, colNames=T, rowNames=T, check.names=FALSE)

#### 构造phyloseq对象并加入phyloseq
```
physeq <- phyloseq(otu_table(ot, taxa_are_rows = TRUE),
tax_table(as.matrix(ta)),
sample_data(gr))
physeq
```

#### 使用ape包建立 OTU 系统发育树并导入 phyloseq 对象：
```
library(ape)
random_tree <- rtree(ntaxa(physeq), rooted=TRUE, tip.label=taxa_names(physeq))
plot(random_tree)
```

#### 使用merge_phyloseq函数在之前创建的physeq对象中加入sample_data和phy_tree数据
```
physeq1 <- merge_phyloseq(physeq, random_tree)
physeq <- physeq1
```

#### phyloseq按照一定要求合并数据
```
#数据过滤，保留count大于0的OTU
GP = prune_taxa(taxa_sums(physeq) > 0.01, physeq)
sample_data(GP)$compart <- get_variable(GP, "compart") %in% c("B")
```

#### 按照SampleType合并样品
```
mergedGP = merge_samples(GP, "compart")
SD = merge_samples(sample_data(GP), "compart")
print(SD[, "compart"])
sample_data(mergedGP)
```

### R包之dplyr--处理表格数据的好帮手

#### 用table函数可以查看某一列下不同因子的个数

table(datscat$indicator)

#### 用mutate函数将compart列下的BS和RS都合并为ck。

datscat_combine <- datscat %>%
mutate(compart = gsub(".*S","ck",compart))
view(datscat_combine)
table(datscat_combine$compart)

#### mutate另加一列

datscat_addcol <- datscat %>%
mutate(newcolname = alpha^2)

#### 用mutate函数将module列下的module都合并为Module 。

da <- da %>% mutate(module = gsub("module","Module ",module))

#### 在df1数据框中，如果nodes_id列的值或字符在label_note中，那么为nodes_id列的值，否则为label列的值

df1 <- df1 %>%
mutate(label = if_else(df1$nodes_id %in% label_note, nodes_id, label))

#### relocate改变列的顺序

datscat_relocate <- datscat %>%
relocate(tr,SampleID,crop)

#### filter函数可以对compart列按照某一因子进行筛选，留下我们想要的结果。

datscat_filter <- datscat %>%
filter(compart == "BS")

#### 用select函数可以提取我们想要的列

datscat_col <- datscat %>%
select(SampleID,tr,indicator,alpha)
view(datscat_col)

#### 用select函数提取选取以字母c开头的列

datscat_col <- datscat %>%
select(starts_with("c"))

#### slice函数筛选所需要的行

datscat_slice <- datscat %>%
slice(25:30)

#### rename函数可以修改列名,修改后的列名=修改前的列名

datscat_rename <- datscat %>%
rename(sample = SampleID)

#### arrange函数按照列对数据进行排序

#### arrange(desc(列名1),列名2):以列名1为对象对数据进行降序排列,然后对相同排名的观测值以列名2进行升序排序

datscat_arrange <- datscat %>%
arrange(desc(compart), crop)

#### env数据框按照otu数据框的行名排列env数据框的行

env <- env[match(row.names(otu),row.names(env)),]

#### 按照指定的因子顺序排序，先good，在excellent，最后poor
```
file$Code <- factor(file$Code , levels = c("good", "excellent","poor"))
```

#### 先按照code的指定顺序排序，再按照Score降序
```
View(file[order(file$Code,-file$Score),])
```

#### group_by函数能够将数据进行分组. 这里group_by( )中处理的分组数据类型需为factor,经过group_by( )计算的数据将按照分组进行计算.

datscat_groupby <- datscat %>%
select(crop,indicator,alpha) %>%
group_by(indicator) %>%
summarise(Mean = mean(alpha),SD=sd(alpha))

#### 批量将字符型转化为因子型

df <- da %>% mutate(across(where(is.character), factor))

#### 把所有变量转换为"character"类型

sample <- data.frame(lapply(sample, as.character), stringsAsFactors=FALSE)

#### 选择特定的行列，并表示为百分数
df['row1',"col1"] %>% scales::percent(0.1)

#### p值变*号
```
envi$label <- as.character(symnum(envi$Pr..F.,cutpoints = c(0, 0.001, 0.01, 0.05, 0.1, 1),
symbols = c("******", "****", "**", ".", "")))
```

#### 将df中的NA转化为0

df[is.na(df)] <- 0

#### 筛选df数据框中colname列取值为"CK","MF"的行

df_fil <- df %>% filter(.,colname %in% c("CK","MF"))

#### 检查两个数据框的行名是否一致

df1row <- rownames(al)
df2row <- rownames(altr)
df1row[!(df1row %in% df2row)] %>% view()

#### 将net.p1数据框按照property列排序，其顺序为net.p数据框的property列的顺序
```
net.p1[order(factor(net.p1$property,levels=c(net.p$property))),]
```

#### grep()函数，查找list_name中包含“networkproperty”字符串的值，参考[https://www.jianshu.com/p/af45ddc1bc8a](https://www.jianshu.com/p/af45ddc1bc8a)

list_name1 <- grep(".networkproperty.",list_name,value = T)


