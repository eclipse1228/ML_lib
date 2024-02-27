# DF 기본 함수

  sample.head() : 앞 5개 보기 
	sample.info() : 정보 보기
	sample.describe() : count,mean,std,min.. 자세한 정보 제공

# 데이터 프레임 만들기 

```python
# name , age 는 1개의 열로 추가됨.
sample_dic = {'name': ['john','bsdev','gogo'], 'age' : [10,20,30]};
pd.DataFrame(sample_dic)

# 이거는 1,2    3,4 가 하나의 행으로 추가됨.
# 왜냐하면 key 값이 없으니, default 가 행으로 추가 같음.
pd.DataFrame([[1,2],[3,4],[5,6],[7,8]])

pd.DataFrame([[1,2],[3,4],[5,6],[7,8]], columns= ['var_1','var_2'], index= ['a','b','c','d'])
```
# csv 파일 읽기
```python
sample_df = pd.read_csv('sample.csv')
```
# 인덱싱
특정 행과 열에서 데이터의 일부를 선택하는 것을 인덱싱 이라고 한다.

```python
# 0번째 열을 index 로 하겠다.((제일 왼쪽에 있는) index가 0번째 열로 다 바뀜)
sample_df=pd.read_csv('sample_df.csv', index_col =0)
sample_df
```

### 열 하나만 선택해서 보기
	sample_df['var_1']
	여러개 선택해서 보기 -> sample_df[['var_1','var_2','var_3']]
		주의! [] 로 한 번 더 감싸야야한다! (리스트랑 헷갈리니까)

### 행 기준 인덱싱
	sample_df.loc['a']
	sample_df.loc[['a','b','c']]   sample_df.loc['a':'c']

	sample_df.iloc[0:3] # 이건  0번째에서 2번째 (iloc (index 기준으로))
	sample_df.iloc[0:3 2:4] # 행과 열 동시에(2:4는 2~4 열임)


```python
# 인덱스 초기화하기 (0~ x 로 초기화됨)
sample_df.reset_index()  # 초기화했으나 기존 index가 열로 병합됨
sample_Df.reset_index(drop=True) # 초기화 후 기존 index 날려버리기.

# 인덱스 정하기
sample_df.set_index('var_1')

```
### 칼럼, 행 제거하기

``` python
# 칼럼제거하기 
sample_df.drop('var_1',axis=1) # 제거할 axis 꼭 표기! (default가 행제거임))

# 행 제거
sample_df.drop(['a','b']) 
```

# 내부 함수 

### 계산 함수 
```python
sample_df.sum() # mean.. std 등등  
sample_df.aggregate(['sum','mean']) # 여러개 내부함수 사용 
``` 

## 그룹별 계산

```python
# 그룹을 만들어서 mean 계산
iris.groupby('class').mean()

iris.groupby('class').agg(['count','mean']) # 두 개 계
```


# 고유 값 확인 

```python
iris['class'].unique() # class 열의 고유값 확인

iris['class'].nunique() # 고유값의 갯수

iris['class'].value_counts() # 값 갯수 카운트


```

# 합치기 (merge,join,concat)

## merge()
```python

# Default 는 inner join 이다. (교집합)
# 자동으로 공통으로 존재하는 컬럼명을 찾아서 그것을 key값으로 활용한다.
left.merge(right)

# 만약 특정 변수를 지정해서 key값으로 활용하고 싶다면
left.merge(right, on = {'원하는 변수})

# 전체 조인 (합집합)
left.merge(right, how ='outer')

						
left.merge(right, how= 'left') # 왼쪽에 맞춰서 오른쪽 값을 merge하기 
# 없는 값은 NaN으로 기입됨

```

## join()
**인덱스**를 key로 삼아서 테이블을 합친다. 
(merge는 공통되는게 있으면 자동으로 key로 삼았음.)

**(merge 는 df 안에 있는 컬럼으로 자동으로 테이블을 합쳤다.**
**하지만 join은 인덱스를 지정해서 테이블을 합친다.)**
! 중복되는 컬럼명이 있으면 오류가 발생함! 제거하고 join 해야한다.
left.drop('key',axis=1).join(right.drop('key',axis=1))

1. 먼저 index를 동일하게 만들어준다.
2. join()
```python
left = left.set_index('key')
right = right.set_index('key')

left.join(right)

# how , on 가능하다. 
left.join(right, how='inner')
left.join(right, on ='key'); 
```

## concat() 
행을 기준으로 결합한다. 
단순히 마지막 행아래에 결합함.
```python

pd.concat([left,right]) 
# 열기준은..
pd.concat([left,right], axis =1)
```


## 
merge: 특정 컬럼을 기준으로 데이터 결합
join() : 인덱스를 기준으로 데이터를 결합
concat() : 행을 기준으로 합친다. (왼/오른쪽 지원 x)
