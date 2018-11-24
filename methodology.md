'사라진 방화'  어떻게 분석했나



  이 글은 YTN 데이터저널리즘팀이 2018년 9월부터 방송과 인터넷으로 연속 보도한 시리즈  **'사라진 방화 - 화재 조사의 불편한 진실'** 의 자료 분석 과정에 대한 이해를 돕기 위한 글입니다.  관련 콘텐츠는  [사라진 방화](http:dev.heartcount.io/YTN)  특집 사이트에서 별도로 정리되어 공개되어 있습니다. 관련 데이터 역시 깃허브에서 내려받을 수 있습니다.



## 데이터

####          1. **화재** 데이터

소방청에 정보공개청구해 받은 2007년부터 2017년까지의 전국 화재 데이터가 488,912건입니다.

화재가 발생한 뒤 개별 소방서의 담당 소방조사관이 국가화재정보시스템에 입력한 데이터로

199개의 칼럼 중 결측치가 많은 대부분 칼럼을 제외하고 20여 개의 핵심 칼럼을 활용했습니다.

칼럼 이름은 분석 편의를 위해 영어로 개명했습니다.

- number :  소방서에서 부여한 화재 현장 조사서 고유 번호   (예: '200741170001) : character   

- province : 화재 발생지의 광역지자체 (예: 경기도) : character

- station : 담당 소방서 (예: 파주소방서)  : character 

- province_station : 광역지자체 + 담당 소방서 이름 (예: 서울특별시 용산소방서) : character 

- death : 사망자 숫자 (명) (예:0,1,2,3...) : integer

- injury : 부상자 숫자 (명) (예:0,1,2,3...) : integer 

- time_fire : 화재 발생 연월일시 예: 2007-01-01 0:20 : date 

- year : 화재 발생 연도  (년)  :  integer 

- month : 화재 발생 월 (월) : integer 

- date : 화재 발생 일 (일) : integer

- hour : 화재 발생 시각 (시) : integer

- minute : 화재 발생 분 (분) : integer

- day_week : 화재 발생 요일 (요일) : character 

- type : 화재 발생지의 성격 구분 (예:'건축,구조물', '자동차, 철도차량', '임야', '선박,항공기','기타(쓰레기화재등)'  : character  

- propertydamage : 재산손실액 (천 원)  : number   

- source1:발화원 대분류 : character 

- source2: 발화원 소분류  : character 

- cause1: 발화요인 대분류 : character 

- cause2: 발화요인 소분류 : character 

- mat2 : 최초 착화물 재료 : character 

- place2 : 화재 발생 장소 중분류 : character 

- place3: 화재 발생 장소 소분류 : character 

- manpower : 동원 소방관 숫자 (명) : integer 

- temp : 기온 (섭씨) : number 



  #### 2. 소방서 일련 번호 데이터 

province_station : 광역지자체 + 소방서 이름  : character 

id : 'province_station'을 가나다순으로 정렬해 붙인 일련 번호 : integer 



## 분석 방법 개요  

YTN 데이터저널리즘팀은 소방 화재 데이터상에서 전체 화재 중 '방화'나 '방화 의심'으로 판정된 화재의 비율이 10여 년 전에는 10%였다가 지속적으로 낮아져 현재는 2%대에 머무는 사실을 주목했습니다. 관련 당국과 화재 조사 전문가 사이에는 이처럼 낮은 수치는 "바람직하지 못하다" 거나 "상식적이지 않다"는 내부적인 우려가 있었습니다. 방화로 잡혀야할 화재가 제대로 걸러지지 않고 있을 가능성이 있기 때문입니다. 실제로 그럴까?  그 원인과 배경을 정밀하게 조사할 수 있는 데이터는 없었습니다. 개별 화재 사건의 조사 및 수사 결과에 대한 자세한 데이터는 외부에 거의 공개되지 않고 있었습니다. 전국적인 범위에서  그 실태를 파악하는 것은 현실적으로 어려운 상황이었습니다. 기자는 조사 대상을 좁혀 데이터를 모으고, 현행 화재조사체계의 문제점은 없는지 파헤치기로 했습니다. 다만, 실제로 방화가 줄어들었을 가능성도 열어놓고 취재를 진행했습니다.  

 기자는 1차 분석에서 각 연도와 광역지자체별로 변화 추이를 분석해, 방화비율의 감소가 전국적으로 진행된 현상이었음을 확인했습니다.

취재진은 이어 연도별 소방서별 변화 추이를 분석했습니다. 2007년, 2008년과 2016, 2017년을 각각 묶어 10여 년 사이에 방화비율이 크게 떨어진 곳들을 '관심 구역'으로 골라냈습니다. 이어 데이터로 머신러닝 알고리즘인 랜덤 포레스트를 학습시킨 뒤,  '관심 구역'의 화재가 방화인지에 대해 알고리즘이 예측하도록 했습니다. 이 예측 결과를 실제 소방서의 화재 조사 결과와 비교해, 컴퓨터는 방화일 것으로 판단했으나, 소방서는 일반 화재로 분류한 '수상한 화재'들을 추렸습니다.  다음 단계로 이렇게 골라낸 화재에 대해 소방 현장 조사서를 입수해, 전문가와 함께 그 내용을 검토했습니다. 또 경찰은 방화 의혹을 어떻게 수사했는지도 조사했습니다. 이렇게 정량 분석과 정성 분석을 병행하는 방법으로 '수상한 화재 목록'을 도출할 수 있었습니다.  



## 소방서 담당 구역별 방화비율 분석 



앞서 언급한 대로 연도별 소방서별 방화비율의 변화 추이는 2007년과 2008년, 2016년과 2017년을 각각 묶어 방화 비율이 변한 정도를 비교했습니다. 그 결과 인천 계양소방서와 서울특별시 용산소방서 등 방화비율이 20% 가까이에서 1%나 0%대로 떨어진 소방서 구역 등을 찾아낼 수 있었습니다. 다음은 R로 소방서 구역을 분석한 코드입니다.

  

```

library(readr)
library(dplyr)
library(tidyr)

# 데이터 불러오기
fire_s=read.csv("firedata.csv",stringsAsFactors= FALSE)

# 문자열 필드의 안보이는 앞뒤 공백을 제거
str_trim(fire1$province, side="both")
str_trim(fire1$station, side="both")
str_trim(fire1$source2, side="both")
str_trim(fire1$cause2, side="both")
str_trim(fire1$mat2, side="both")
str_trim(fire1$province_station, side="both")

#2007년 2008년을 묶어 20072008년으로 2016년 2017년을 묶어 20162017년으로 표시한 year2 필드 생성
#20072008과 20162017만 필터링
#발화요인 소분류가 방화나 방화의심이면 1, 아니면 0으로 표시한 arson 필드 생성
#지역+소방서이름 필드와 year2 별로 화재집계, 방화집계, 방화비율 계산 

fire1=fire_s %>% 
  mutate(year2= ifelse(year %in% c(2007,2008), 20072008,ifelse(year %in% c(2016,2017),20162017, year))) %>% 
  filter(year2== 20072008 | year2==20162017) %>%
  mutate(arson=ifelse(cause2 %in% c("방화","방화의심"),1,0)) %>% 
  group_by(province_station,year2) %>%
  summarise(count_fire=n(),count_arson=sum(arson,na.rm=TRUE),arsonratio=((count_arson/count_fire)*100))

#20072008과 20162017을 spread 함수로 펼치기
fire2=fire1 %>%
  select(province_station,year2, arsonratio)%>%
  spread(year2, arsonratio)

#20072008 등 칼럼의 숫자이름을 year200782008로 개명해 다음 단계 분석에서 에러 방지
df=data.frame(fire2)
colnames(fire2) = c("province_station","year20072008","year20162017")

#소방서별로 20072008년 방화비율에서 20162017년 방화비율 차이를 소숫점 둘째자리까지 계산해 순위 작성
fire3=fire2 %>%
  filter(!is.na(year20072008|year20162017)) %>%
  mutate(arsonratio_change=round((year20072008-year20162017),2)) %>%
  filter(!is.na(arsonratio_change)) %>% 
  arrange(desc(arsonratio_change))
```



## 머신 러닝을 위한 변수 전처리 



기자는 머신 러닝 분석을 위해서  방화 비율이 가장 많이 떨어졌으면서, 방화 비율이 1%대 이하인 6개 소방서 구역을 골라내 '관심 구역'으로 삼았습니다. 6개 소방서를 고른 이유는,  해당 구역에서 어떤 연유에서 방화 비율이 현저히 떨어졌는지 그 속살을 들여다보기 위함입니다. 또한 데이터 분석을 담당한 기자 1인이 취재로 추가 조사할 수 있는 분량에 한계가 있는 현실도 고려했습니다. 머신 러닝 분석 이후에 개별 조사서를 일일이 입수하고, 문헌 분석과 취재를 통해 내용을 확인할 수 있는 분량을 고려할 때 6개 구역이 적당한 범위로 판단했습니다. 아울러 관심 구역의 범위를 좁힘으로써 나머지 구역의 데이터 즉 컴퓨터를 학습시킬 데이터의 양을 더 충분히 늘릴 수 있다는 이점도 있었습니다. 다만, 6개 소방서 구역의 선정은 방법론상의 선택일 뿐, 이 지역 이외의 구역에서도 '수상한 화재'는 얼마든지 발견될 개연성이 있음을 밝혀둡니다.

기자는 관심구역(6곳)과 일반 구역(관심 구역 이외의 구역 212곳 )의 2016년과 2017년 데이터를 75대 15의 비율로 나눴습니다. 이를 각각 학습 데이터와 테스트 데이터로 삼았습니다.아울러 데이터의 불균형성에서 오는 문제점을 해결하기 위해, 학습 데이터에서 방화와 일반 화재의 비율이 균형을 이루도록 다운샘플링했습니다. 

아울러 범주형 데이터는 더미 변수 (각각의 항목에 해당하면 1, 아니면 0으로 표시) 칼럼을 생성해 처리했습니다. R의 Random forest package는 더미 변수 대신 범주형 데이터도 처리할 수 있지만, 더미 변수를 활용하는 편이 더 안정적으로 예측을 실행할 수 있을 것으로 판단했습니다.  화재 데이터에는 다양한 필드가 있었지만, 여러 번의

시행착오를 거쳐 선별한 '발화원과 '최초 착화물, 재산 손실액 등의 핵심 변수를 중심으로 랜덤 포레스트를 학습시켰습니다.  기자가 랜덤 포레스트를 위한 데이터 전처리에 사용한 R 코드는 다음과 같습니다.

```
library(readr)
library(dplyr)
library(tidyr)
library(caret)
library(randomForest)
library(magrittr)
library(stringr)

set.seed(1000)

fire1=read.csv("firedata.csv",stringsAsFactors= FALSE)

#문자열 필드의 안보이는 앞뒤 공백을 제거
str_trim(fire1$province, side="both")
str_trim(fire1$station1, side="both")
str_trim(fire1$source2, side="both")
str_trim(fire1$cause2, side="both")
str_trim(fire1$mat2, side="both")
str_trim(fire1$place2, side="both")
str_trim(fire1$place3, side="both")
str_trim(fire1$province_station, side="both")

#소방서 일련번호 목록 파일을 불러와 조이닝 
stationid=read.csv("province_station_id.csv",stringsAsFactors = FALSE)
fire2=full_join(fire1,stationid,by="province_station")

##필요없는 필드 제외
fire3=fire2 %>%  select(-date, -minute, -type,-place2,-place3,-province_station)

#2016, 2017년 화재만 필터링
fire20162017=fire3 %>% filter(year==2016|year==2017)

#arson 필드 생성- 방화1, 일반화재0
fire20162017 %<>% mutate(arson=ifelse(cause2 %in% c("방화","방화의심"),1,0))

#one-hot-encoding, 발화요인소분류, 최초착화물소분류 필드 

FactoredVariable = factor(fire20162017$source2)
dumm = as.data.frame(model.matrix(~FactoredVariable)[,-1])
dfWithDummies = cbind(fire20162017, dumm)

FactoredVariable = factor(fire20162017$mat2)
dumm = as.data.frame(model.matrix(~FactoredVariable)[,-1])
dfWithDummies2 = cbind(dfWithDummies, dumm)

#arson의 데이터 타입을 factor 변수로
dfWithDummies2$arson = factor(dfWithDummies2$arson)

#오브젝트 이름 x로 바꿈
x1=dfWithDummies2

#중복된 더미변수 필드명 '기타' '미상'을 다른 이름으로 수정 
colnames(x1)[50] = "FactoredVariable기타재료"
colnames(x1)[69] = "FactoredVariable미상재료"

#필드명 앞의 "factoredvariables" 제거 
names(x1) <- gsub("FactoredVariable", "", names(x1))

#필드명 에러 안나게 수정 (쉼표 등 제거)
names(x1) <- make.names(names(x1))

#10년간 방화비율이 급학한 6개 관심 소방서 구역 화재를 x3에, 그 외 화재를 x2로  
x2=x1%>% filter(!station_id %in% c(150,138,146,106,118,119))
x3=x1%>% filter(station_id %in% c(150,138,146,106,118,119))

#더미 변수로 만든 원래 필드 등 이제 필요없는 변수 제거
x2_1=x2 %>% select(-number,-province,-station,-time_fire,-year,-source2,-cause2,-mat2,
-day_week,-station_id)
x3_1=x3 %>% select(-number,-province,-station,-time_fire,-year,-source2,-cause2,-mat2,
-day_week,-station_id)

#x2_1을 75:25 비율로 분할하고 각각 학습용, 테스팅용으로 지정
inTrain<- createDataPartition(y=x2_1$arson, p=0.75, list=FALSE)
training<-x2_1[inTrain,]
testing<-x2_1[-inTrain,]

#연습 데이터를 다운샘플링  
trainingdown <- downSample(subset(training, select=-arson), training$arson)


```



  

##  랜덤 포레스트로 방화 여부 예측하기 



전 처리된 데이터로 랜덤 포레스트를 학습시킨 뒤, 테스트 데이터에서 예측을 실행해 그 정확도를 계산하니 87%가 나왔습니다. (정확도는 seed를 어떻게 설정하느냐에 따라 다소 변할 수 있습니다.)  이어 6개 관심 구역에서 

일어난 화재가 방화일지 여부에 대해 예측하도록 했습니다. 참고로 변수의 중요도 분석 그래프를 보면 

라이터로 불을  붙였는지가 단연 중요한 변수로  활용됐음을 알 수 있었습니다. 다만 이렇게 예측된 결과에는 

그 또한 오류가 있을 수 있음을 밝혀 둡니다. 학습 데이터의 변수가 제한적이며, 학습 데이터로 삼은 212곳

 소방서의 방화 판정 내역 자체가 오류를 안고 있을 수 있기 때문입니다. 



다음의 코드는 랜럼 포레스트 분석 코드입니다. 앞의 전처리 코드와 죽 붙여 이어서 실행해야 합니다.



```
library(readr)
library(dplyr)
library(tidyr)
library(magrittr)
library(stringr)
library(caret)
library(randomForest)

set.seed(1000)

#랜덤 포레스트 학습
modFit <- randomForest(Class ~. , data=trainingdown,na.action = na.omit)

##predict 명령어가 인식하도록 타겟필드명 arson을 Class로 변경
colnames(testing)[8]="Class"

#정확도 예측
predict(modFit, newdata = testing) %>% confusionMatrix(testing$Class)

#관심 구역 화재도 타겟 필드명을 Class로 변경
colnames(x3_1)[8]="Class"

predict(modFit, newdata = x3_1) %>% confusionMatrix(x3_1$Class)

#각 화재의 방화 확률 계산 
prediction_prob = predict(modFit, newdata = x3_1, type = "prob")

#변수 중요도 시각화
varImpPlot(modFit, pch = 19, main = "Importance of variables", color = "red", cex = 1)

#방화 확률 필드를 6개 구역 화재 데이터에 붙임 
x_prediction = cbind(x3, prediction_prob)

#방화 확률 필드명 변경
colnames(x_prediction)[130] = "prob"
colnames(x_prediction)[131] = "prob_arson"

#일부 필드만 선별
x_prediction1=x_prediction %>% select(number,province,station,year,month,hour,time_fire,source2,cause2,mat2,arson,prob,prob_arson)

#화재조사서 번호 기준으로 화재 로데이터에 방화 확률 필드 등 조이닝 
fire_joined= full_join(fire1, x_prediction1, by = "number")

#최종 결과물 csv 파일로 저장
write.csv(fire_joined,"fire_arson_prob.csv")


```





## 화재 현장 조사서와 내사결과 보고서 등을 활용한 검증

 

앞서 언급한대로  '사라진 방화' 분석 과정은 정량 분석과 함께 관련 문서와 정보 취재를 병행해 이뤄졌습니다.

머신 러닝을 통해 6개 구역에서 150여 건의 화재를 고른 뒤, 여기에 대한 화재현장조사서를

입수하고, 이에 대한 전문가의 의견을 들어 수상한 화재를 찾아갔습니다. 익명을 요청한 오랜 경력의

화재 조사관 2명의 도움도 받았습니다.  경찰은 어떻게 처리했는지도 확인하기 위해 일일이 해당 경찰서에  2번

이상의 확인 과정을 거쳤습니다. 경찰은 화재 사건 데이터를 로데이터로  공개하지 않기 때문에 해당 화재에

대한 내사 결과 보고서를 정보공개청구하거나 전화 취재를 통해 해당 사건의 수사 내용을 파악했습니다. 이상의 내용은  스프레드시트  [우리 동네 수상한 화재 보고서 1] (http://bit.ly/수상한화재_1)을 통해 확인할 수 있습니다.  소방 화재 현장조사서의 사본 링크도 스프레드시트에 함께 공개했습니다.



## 구글 스프레드시트를 통한  관련 자료 공개 



취재진은 국내 화재 조사체계의 양대 축인 소방과 경찰의 방화 의심 사건 처리 내역과 관련해 공개한

스프레드시트는 3가지입니다. 경찰은 로데이터를 공개하지 않고, 소방당국의 화재 로데이터도 

방화 분석에 한계점이 있는 상황에서 ,  수상한 화재를 수집해 공개함으로써 

사회적 경각심을 높이고, 화재조사시스템의 개선을 위한 공론화를 유도하고자 했습니다. 

  

1. [우리 동네 수상한 화재 목록 1](http://bit.ly/수상한화재_1) 

- 소방도, 경찰도, 방화 의혹을 규명하지 못한 화재 24건

- 동시에 컴퓨터 알고리즘은 방화 가능성이 있다고 보았고, 취재진과 전문가의 내용 검토를 

  거친 사건  

2. [우리 동네 수상한 화재 목록 2](http://bit.ly/수상한화재_2)  

-   소방서는 방화나 방화의심 사건으로 분류했지만 경찰은 내사종결하거나 달리 판단한 화재 19건

3. [경찰 처리 방화 사건 목록](http://bit.ly/경찰서방화사건개요) 

- 관심 소방서 구역 6개 구역 중 대전을 제외한 해당 구역 경찰서가 2016년, 2017년에 방화 사건으로 처리한 사건  79건

- 경찰의 방화 사건 처리 내역이 좀처럼 외부에 공개되지 않는 상황에서, 경찰이 방화로 판단하는

  화재 사건은 어떤 유형이며, 얼마나 되는지를 엿볼 수 있음



   ## 대형 화재에 대한 사례 연구

  수상한 화재 목록의 화재들은 우리 주변에서 일어나지만, 언론에는 좀처럼 보도되지 않는 ''작은 불''입니다. 

 이와 별도로 과거에 언론의 큰 관심을 받은 대형 화재이지만 화재 조사 결과에 대해 논쟁점이 남은 

 주요 화재 5건을 별도로 사례 분석했습니다. 화재현장 조사서와 내사 결과 보고서, 국립과학수사연구원 감정서

 등을  정보공개청구하거나 별도의 취재를 통해 사례 연구를 했습니다. 해당 주요 화재의 목록은 다음과 같습니다.



- 1999년 씨랜드 화재  
- 2005년 강호순 방화 사건
- 2008년 용인 고시원 화재
- 2017년 광주 3남매 사망 화재 사건
- 2017년 강릉 석란정 화재 

 

 취재진은 이 대형 화재들에서도 '수상한 화재 목록'에서 확인한 화재 조사 시스템의 허점이 일부 반복적으로

 드러남을 확인할 수 있었습니다.



##  과학 수사와 단락흔 판정에 대한 조사  



취재진은 전국 17개 지방경찰청과 경기도 일선 경찰서, 국립과학수사연구원을 상대로 화재 사건에 대한 과학 수사

실태에 대해  정보공개청구를 통해 문의했습니다. 이 과정에서 단락흔 판정에 대한 국과수 등 관계 당국의

입장을 들었고, 단락흔만으로는 전기 화재인지 여부를 구분할 수 없다는 사실을 확인할 수 있었습니다.





  





















  









 





























  

