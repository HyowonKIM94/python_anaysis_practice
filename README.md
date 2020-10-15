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


