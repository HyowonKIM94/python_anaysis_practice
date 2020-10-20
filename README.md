# python_anaysis_practice

import pandas as pd 
# csv,exel 과 같은 파일을 읽을 수 있는 모듈 = pandas 판다스 

CCTV_Seoul = pd.read_csv('data/01. CCTV_in_Seoul.csv',encoding= 'utf-8') 
#판다스에서 csv파일을 읽는 명령은 read_csv ('파일경로',인코딩방식='') 
# csv파일이 data폴더안에 있으므로 경로 앞에 붙여줘야 파일경로를 인식함 
CCTV_Seoul.head()

CCTV_Seoul.columns
CCTV_Seoul.columns[0] # 서울 씨씨티비 파일의 열들중 인덱스가 0번인 값 = 1열 값

CCTV_Seoul.rename(columns={CCTV_Seoul.columns[0]:'구별'},inplace=True)
# 열 이름 바꾸기 rename 
# rename(columns={바꾸고싶은 컬럼 인덱스:'바꾸고 싶은 이름'},inplace-True) 
# inplace=True 는 실제 CCTV_Seoul 변수의 내용을 갱신 
CCTV_Seoul.head()

pop_Seoul=pd.read_excel('data/01. population_in_Seoul.xls') #엑셀파일 불러오기 read_excel
pop_Seoul.head()

# header=2로 하여 3줄을 읽도록 하자. 
#엑셀 컬럼을 나타내는 B, D, G, J, N 열만 읽도록 parse_cols='B,D,G,J,N' 옵션을 넣는다.(pasecols-> usecols로 바뀜)
pop_Seoul = pd.read_excel('data/01. population_in_Seoul.xls', 
                           header=2,
                           usecols = 'B, D, G, J, N')
pop_Seoul.head()

pop_Seoul.rename(columns={pop_Seoul.columns[0]: '구별',
                          pop_Seoul.columns[1]: '인구수',
                          pop_Seoul.columns[2]: '한국인',
                          pop_Seoul.columns[3]: '외국인',
                          pop_Seoul.columns[4]: '고령자',} , inplace=True)
pop_Seoul.head()


#컬럼명 변경하기 
# 변수명.리네임 (컬럼명들 = {변수.컬럼명들[인덱스]: '새 컬럼명',
#변수.컬럼명들[인덱스]:'새컬럼명',
#변수.컬럼명들[인덱스]:'마지막 새컬럼명',}, inplace=True) 마지막 새컬럼명 다음에도 쉼표를 붙임 

pop_Seoul.rename(columns={pop_Seoul.columns[0]:'자치구',
                         pop_Seoul.columns[1]:'인구수',
                         pop_Seoul.columns[4]:'고령자',},inplace=True)
pop_Seoul.head()

import pandas as pd
import numpy as np

#Series - 대괄호로 만드는 파이썬의 list 데이터로 만들 수 있다. 
s=pd.Series([1,3,5,np.nan,6,8])
s
#2013년 01월01일 부터 6일 동안의 데이터를 저장
#변수명 = pd.date_range ('기본날짜', period ='기간')
dates=pd.date_range('20130101',periods=6)
dates

# 6행 4열의 random 변수를 만들고  판다스.데이터프레임 (넘파이.랜덤.randn(행,열))
#columns=['A','B','C','D']로 지정, 
#index는 2013월 01월 01일 부터 6일 동안의 데이터 인덱스(헹)에 코드[39]에서 만든 날짜형 데이터를 입력

df=pd.DataFrame(np.random.randn(6,4),columns=['A','B','C','D'],index=dates)
df
# 첫3행을 보여줌 
df.head(3)

# DataFrame의 index 정보 출력 = df.index
print(df.index)

# DataFrame의 column 정보 출력 = df.column 
print(df.columns)

# DataFrame의 Values정보 출력 = 행/열 내용 값 =df.values
print(df.values)

# DdataFrame 의 통계적 개요를 확인 = df.describe()
print(df.describe())

# DataFrame 컬럼에 정렬 기준 지정 
# 데이터프레임.정렬_값들을('b기준으로',오름차순정렬=false 이게 = 내림차순으로)
df.sort_values(by='B',ascending=False) 
# ascending 오름차순이 false이면 내림차순 이란 의미 
 
pandas 이용해서 CCTV와 인구 현황 데이터 파악하기

CCTV_Seoul.head()
# cctv_seoul 에서 cctv 전체 개수인 소계로 오른차순 정렬 (변수지정없이)
CCTV_Seoul.sort_values(by='소계',ascending=True).head(5)
# cctv_seoul 에서 cctv 전체 개수인 소계로 오른차순 정렬 (변수지정하여)
CCTV_seoul= CCTV_Seoul.sort_values(by='소계',ascending=True)
CCTV_seoul.head(3)

# 2014-2016년 3년간 cctv수를 더하고 2013년 이전 cctv수로 나누어 최근 3년간 cctv증가율 계산 (*100 )

CCTV_Seoul['최근증가율']= (CCTV_Seoul['2014년']+CCTV_Seoul['2015년']+CCTV_Seoul['2016년'])/CCTV_Seoul['2013년도 이전']*100
CCTV_Seoul

# 최근증가율 기준으로 내림차순으로 정렬 

CCTV_Seoul.sort_values(by='최근증가율',ascending=False).head()

# 결론 :2014~2016 3년간 cctv가 2013년 대비 많이 증가한 구는 '종로구','도봉구','마포구 순임 '

pop_Seoul.head()
# 필요없는 행 데이터 삭제 (df)변수명.drop 
pop_Seoul.drop([0],inplace=True)
pop_Seoul.head()
#유니크 조사 - 반복된 데이터는 하나로 나타내서 한 번 이상 나타난 데이터를 확인하기 
# (df)변수명['칼럼명'].unique()
pop_Seoul['자치구'].unique()
pop_Seoul.isnull()
pop_Seoul[pop_Seoul['자치구'].isnull()] #자치구에서 nan인 값을 찾고싶어 
pop_Seoul.drop([26],inplace=True)
pop_Seoul

pop_Seoul['외국인비율'] = pop_Seoul['외국인']/ pop_Seoul['인구수'] *100
pop_Seoul.head()
pop_Seoul['한국인비율'] = pop_Seoul['한국인']/ pop_Seoul['인구수'] *100
pop_Seoul.head()
pop_Seoul['고령자비율'] = pop_Seoul['고령자']/ pop_Seoul['인구수'] *100
pop_Seoul.head()

# 데이터 프레임 열 삭제 (aixs='columns') 라고 주어야 함 
pop_Seoul.drop(['한국인비율'],axis='columns',inplace=True)
pop_Seoul.head()

pop_Seoul['고령자비율'] = pop_Seoul['고령자'] / pop_Seoul['인구수'] *100
pop_Seoul.head()

# 인구수가 높은 순으로 자치구 정렬 
pop_Seoul.sort_values(by='인구수',ascending=False).head()

# 외국인 수가 높은 순으로 자치구 정렬
pop_Seoul.sort_values(by='외국인', ascending=False).head()

# 외국인 비율이 높은 순으로 자치구 정렬
pop_Seoul.sort_values(by='외국인비율',ascending=False).head()

# 고령자 수가 높은 순으로 자치구 정렬
pop_Seoul.sort_values(by='고령자',ascending=False).head()

# 고령자 비율이 높은 순으로 자치구 정렬
pop_Seoul.sort_values(by='고령자비율',ascending=False).head()

Pandas 고급기능- 두 DataFrame병합하기
CCTV_Seoul.head()
pop_Seoul.head()
pop_Seoul.rename(columns={pop_Seoul.columns[0]:'구별'},inplace=True)
# 열 이름 바꾸기 rename 
# rename(columns={바꾸고싶은 컬럼 인덱스:'바꾸고 싶은 이름'},inplace-True) 
# inplace=True 는 실제 CCTV_Seoul 변수의 내용을 갱신 
pop_Seoul.head()

# CCTV 데이터와 인구 현황 데이터 합치기 

data_result= pd.merge(CCTV_Seoul,pop_Seoul,how='inner',on='구별')
data_result.head()

#의미없는 컬럼 지우기 2013년도 이전, 2014년, 2015년, 2016년
# 행 방향 삭제 drop 
# 열 방향 삭제 del

# del data_result['2013년도 이전']
del data_result['2014년']
del data_result['2015년']
del data_result['2016년']
data_result.head()

# 향후 그래프를 편하기 그리기 위해 index = 구 이름 되어야 유리 
# 인덱스 변경 set_index
#변수명.set_index ('새로운 인덱스명', inplace=True)


data_result.set_index('구별',inplace=True)
data_result.head()

#상관관계 계산 numpy.corrcoef
#상관관계 지수 0.1 이하 무시 / 0.3이하면 약한 상관관계 / 0.7이상이면 뚜렷한 상관관계 
#고령자 비율과 cctv개수관의 상관관계 \
np.corrcoef(data_result['고령자비율'],data_result['소계'])#약한 음의 상관관계 
np.corrcoef(data_result['외국인비율'],data_result['소계'])
#상관관계가 0.1대로 큰 의미 없음
np.corrcoef(data_result['인구수'],data_result['소계'])
#상관관계 0.3으로 약한 상관관계 
data_result.sort_values(by='소계',ascending=False).head() //CCTV순으로 정렬
data_result.sort_values(by='인구수',ascending=False).head() //인구수 순으로 정렬

# 파이썬의 대표 시각화 도구 Matplotlib
import matplotlib.pyplot as plt
#맷플롯립.파이썬플롯  
%matplotlib inline
#그래프의 결과를 출력 세션에 나타나게 하는 설정

# CCTV현황 그래프로 분석하기
import platform 

from matplotlib import font_manager, rc
plt.rcParams['axes.unicode_minus']=False

if platform.system()=='Darwin':
    rc('font',family='AppleGothic')
elif platform.system()=='Windows':
       path="c:/Windows/Fonts/malgun.ttf"
       font_name=font_manager.FontProperties(fname=path).get_name()
       rc('font',family=font_name)
else:
       print('Unkown system...sorry~~~')
 
//한국어를 인식하게 하기 위한 코드 
data_result.head()

#데이터에서 소계데이터만을 정렬해서 가져와서 수평선 그래프로 보여줘봐 
data_result['소계'].sort_values().plot(kind='barh',grid=False, figsize=(8,8))
plt.show()
#cctv 개수에서 강남구가 월등히 많다는것을 볼수 있음 

#구별 인구대비 cctv 개수를 나누어 cctv 비율 확인하고 그래프로 시각화 
data_result['cctv비율']=data_result['소계']/data_result['인구수'] *100
data_result.head()
data_result['cctv비율'].sort_values().plot(kind='barh',grid=True, figsize=(10,10))
plt.show()

# 점 분산표 그래프(scatter (x,y),s=점사이즈)
plt.figure(figsize=(10,10))
plt.scatter(data_result['인구수'],data_result['소계'],s=50)
plt.xlabel('인구수')
plt.ylabel('CCTV')
plt.grid()
plt.show()

#기울기와 절편 확인 polyfit => array(기울기, 절편)
fp1=np.polyfit(data_result['인구수'],data_result['소계'],1)
fp1
f1=np.poly1d(fp1) # poly1d (폴리원디) 기울기와 절편의 값을 ax + b 형태로 만들어 주기 위한 함수  
fx=np.linspace(100000,700000,100) #린스페이스
# 점 분산표 그래프 + 기울기 직선 그래프 
plt.figure(figsize=(10,10))
plt.scatter(data_result['인구수'],data_result['소계'],s=50)
plt.plot(fx,f1(fx),ls='dashed',lw=3,color='g') 
# f1 = ax+b 이고 a(기울기),b(절편)값이 이미 들어가 있으므로 f1(x) x값만 넣어주면 됨  
plt.xlabel('인구수')
plt.ylabel('CCTV')
plt.grid()
plt.show()

# 오차 컬럼 형성 후 
data_result['오차'] = np.abs(data_result['소계']-f1(data_result['인구수']))
data_result.head()
df_sort = data_result.sort_values(by='오차',ascending=False) #오차 순으로 정렬 
df_sort.head()
# 오차가 높은 구별 인덱스로 표시하여 시각화 하기 
plt.figure(figsize=(14,10))
plt.scatter(data_result['인구수'],data_result['소계'],c=data_result['오차'],s=50)
plt.plot(fx,f1(fx),ls='dashed',lw=3,color='g')

for n in range(10):
    plt.text(df_sort['인구수'][n]*1.02,df_sort['소계'][n]*0.98,df_sort.index[n],fontsize=15)

    
plt.xlabel('인구수')
plt.ylabel('인구당비율')

plt.colorbar()
plt.grid()
plt.show()
