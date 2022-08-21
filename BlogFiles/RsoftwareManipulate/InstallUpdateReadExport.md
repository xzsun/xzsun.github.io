# Install

#网络在线安装github包

#devtools::install_github("YuLab-SMU/ggtreeExtra")

#从github上下载zip包后离线手动安装

devtools::install_local("C://Users//TD//Desktop//Achilles-master.zip")  #镜像选择all-- 1

#导入依赖包没有则自动安装

pacman::p_load(openxlsx,tidyverse,dplyr,sxzforanova,stringr,ggplot2,ggsci,viridis)

#自定义函数

source('G:/R/source/colStandard202103091124.R')

#### BiocManager

if (!requireNamespace("BiocManager", quietly = TRUE))
install.packages("BiocManager")
BiocManager::install(version = "3.15")

BiocManager::install(c("Biostrings"))
BiocManager::install("phyloseq")

#查看的version

R.version

#查看package的version

packageVersion("mypackage")


# Update

update.packages()

#R软件的更新

install.packages("installr")
library(installr)
updateR()


# Unstall

remove.packages("mypackage")

#删除包

remove.packages("mypackage")


# Read files

df <- read.xlsx("ta_select.xlsx", sheet=1, colNames=T, rowNames=F, check.names=FALSE)

```R
bb<-c("setosa","versicolor","virginica")
kk<-list()
for(i in 1:length(bb)){
  kk[[i]]<-read.csv(paste0("D://data",bb[i],".csv"),header=T)
```




# Export

#### 生成csv文件

write.csv(occor.r, file = paste("occor",".csv", sep = ""), row.names = T)

#### 生成xlsx文件

write.xlsx(dfANOVA,paste("dfANOVA",date,".xlsx"),colNames = TRUE,rowNames = TRUE)

#### 生成png文件

ggsave(paste(fname,"_",date,".png", sep = ""),plot=p,family="Times New Roman", #Arial#Times New Roman
width = 4.0,height = 6,units = 'cm',dpi = 600)#1/2width=8.4cm

####  生成PDF文件

ggsave(paste(fname,"_",date,".pdf",sep = ""), family="ArialMT", #Arial#Times New Roman
p, width=4.0, height=6,units='cm',device=cairo_pdf)

#### 生成PPT文件

export::graph2ppt(x=p,file=paste(fname,"_",date,".pptx",sep = ""),width=4.4, height=6,append = F)

#### 保存pptx文件

eoffice::topptx(p,filename = "R图.pptx")

#### Save ANOVA table as an Excel

table2excel(x,file=paste(fname,"相对丰度_",date,".xlsx",sep = ""),
sheetName="aov_noformatting", digits = 1, digitspvals = 3, add.rownames=TRUE, fontName="Times New Roman", fontSize = 12, fontColour = "black",  border=c("top", "bottom"), fgFill = rgb(0.9,0.9,0.9), halign = "center", valign = "center", textDecoration="")

#### Save ANOVA table as a PPT

### Option 1: pass output as object

x=summary(fit)
table2ppt(x=x,file=paste(fname,"相对丰度_",date,".ppt",sep = ""),
add.rownames =TRUE,  width=5, font="Times New Roman",
pointsize=10, digits=4, digitspvals=1, append=TRUE)

#### Save ANOVA table as a DOC file

table2doc(x=x,file=paste(fname,"相对丰度_",date,".doc",sep = ""),
add.rownames =TRUE, width=3.5, font="Times New Roman",
pointsize=10, digits=4, digitspvals=1, append=TRUE)#, add.rownames =TRUE

library(table2office)






