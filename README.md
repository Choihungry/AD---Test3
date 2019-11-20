# AD-Test3 지도시각화 코드 추가제출
### 지도시각화
```{r}
# 설치

#install.packages("ggmap")
#install.packages("ggplot2")
#install.packages("raster")
#install.packages("rgeos")
#install.packages("maptools")
#install.packages("rgdal")


library(ggmap)
library(ggplot2)
library(raster)
library(rgeos)
library(maptools)
library(rgdal)


P <- read.csv(file.choose(), header = T) 

# sample.csv 열기. 여기서 지금 A와 B가 다른 수로 되어있기 때문에 sample.csv파일 엑셀 열어서 내가 원하는 데이터 집어넣기. 중요 포인트: ,있으면 factor로 읽혀서 엑셀에서 통화 -> 기준없음인가? 그걸로 바꾸기 그럼 , 사라짐 int로 안전하게 읽힘 

map <- shapefile(file.choose())

# shp 파일 불러오기

map <- spTransform(map, CRSobj = CRS('+proj=longlat +eIIps=WGS84 +datum=WGS84 +no_defs'))
new_map <- fortify(map, region='SIG_CD')
new_map$id <- as.numeric(new_map$id)

seoul_map <- new_map[new_map$id <=11740,]
P_merge <- merge(seoul_map, P, by='id') 

str(P_merge)

# 데이터 구조확인. 앞서 이야기 했듯이 학생수 데이터 같은 거 int 인지 확인.

#fill옵션에서 계속 바꾸면 됨, + 타이틀, 색상코드
A<-ggplot()+ geom_polygon(data = P_merge, aes(x = long, y = lat, group = group, fill=고등학교.1개당.평균학생수, colour = 시군구명)) + geom_polygon(fill = "#FFFFFF") + scale_fill_gradient(low="#ffe5e5", high="#ff3232", space = "Lab", guide = "colourbar")+ theme_bw() + labs(title = "고등학교") + theme(panel.grid.major.x = element_blank(), panel.grid.minor.x =element_blank(),panel.grid.major.y =element_blank(),panel.grid.minor.y = element_blank(), plot.title = element_text(size = 18, hjust = 0.5))
A
```
