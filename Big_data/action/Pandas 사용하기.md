# Pandas 사용하기



### `iterrows()`

```python
for i, store in stores_most_scored.iterrows():
```

- 순회하는 것
- 첫 번재에 idx 를 받는다.



## dataframes

> Pandas 의 기본 자료구조, 2차원 배열, 리스트 Data Table 전체를 포함하는 object

- 특징
  - Row 와 Column index 가 존재한다.
  - 각 column은 서로 다른 데이터 타입을 가질 수 있다.
  - 기본적으로 csv 포맷을 지원한다.
- 참고
  - https://dsbook.tistory.com/12



### `pd.merge()`

> 두 데이터 프레임을 각 데이터에 존재하는 고유값(key) 를 기준으로 병합할 때, 사용한다.
>
> 디폴트 값 : `pd.merge(df_left, df_right, how='inner', on=None)`

```python
stores_reviews =
	pd.merge(
        dataframes["stores"], dataframes["reviews"], left_on="id", 	right_on="store"
    )
```

- 데이터 프레임을 합치는데 각각 어떤 값을 기준으로 할지 `left_on`, `right_on` 에서 정해준다.



### `pd.groupby()`

> 같은 값을 하나로 묶어서 통계 또는 집계 결과를 얻기 위해서 사용한다.



|      | city | fruits | price | quantity |
| ---- | ---- | ------ | ----- | -------- |
| 0    | 부산 | apple  | 100   | 1        |
| 1    | 부산 | orange | 200   | 2        |
| 2    | 부산 | banana | 250   | 3        |
| 3    | 부산 | banana | 300   | 4        |
| 4    | 서울 | apple  | 150   | 5        |
| 5    | 서울 | apple  | 200   | 6        |
| 6    | 서울 | banana | 400   | 7        |



- ```python
  df.groupby('city').mean()
  ```

  - |      | price | quantity |
    | ---- | ----- | -------- |
    | city |       |          |
    | 부산 | 212.5 | 2.5      |
    | 서울 | 250.0 | 6.0      |

    - 도시별로 가격 평균을 구하고 싶을 때 묶는 용도로 사용한다.

  - `mean()` : 평균값을 구해주는 메서드

- ```python
  df.groupby(['city', 'fruits']).means()
  ```

  - |      |        | price | quantity |
    | ---- | ------ | ----- | -------- |
    | city | fruits |       |          |
    | 부산 | apple  | 100.0 | 1.0      |
    |      | banana | 275.0 | 3.5      |
    |      | orange | 200.0 | 2.0      |
    | 서울 | apple  | 175.0 | 5.5      |
    |      | banana | 400.0 | 7.0      |

    - 도시별로 그룹화하고 다시 과일 종류별로 그룹이 되어서 평균값을 낸다.



```python 
scores_group = stores_reviews.groupby(["store", "store_name"])

scores = scores_group.mean()
```

- 가게와 가게 이름으로 분류를해서 그것에 대한 평균값을 내겠다는 것



### `df.head()`

> `df.head(self, n=5)`
>
> 데이터프레임 내의 처음 n 줄의 데이터를 출력한다.
>
> 객체안에 제대로 된 데이터 타입이 입력되어 있는지 빠르게 확인할 경우에 사용하면 매우 유용하다.



### `df.reset_index()`

> `df.reset_index(drop=False, inplace=False)`
>
> 인덱스를 리셋 시키는데 사용한다.
>
> 새로운 단순한 정수 인덱스를 세팅한다.



- `df.reset_index(drop=False, inplace=False)`
  - `drop`
    - 인덱스로 세팅한 열을 데이터프레임 내에서 삭제할지 여부를 결정한다.
  - `inplace`
    - 원본 객체를 변경할지 여부를 결정한다.



- 예시

  - df

    | c0   | c1   | c2   | c4   |
    | ---- | ---- | ---- | ---- |
    | r0   | 0    | 1    | 2    |
    | r1   | 3    | 4    | 5    |
    | r2   | 6    | 7    | 8    |

  - df.reset_index()

    |      | c0   | c1   | c2   | c4   |
    | ---- | ---- | ---- | ---- | ---- |
    | 0    | r0   | 0    | 1    | 2    |
    | 1    | r1   | 3    | 4    | 5    |
    | 2    | r2   | 6    | 7    | 8    |

  - df

    | c0   | c1   | c2   | c4   |
    | ---- | ---- | ---- | ---- |
    | r0   | 0    | 1    | 2    |
    | r1   | 3    | 4    | 5    |
    | r2   | 6    | 7    | 8    |



### `df.to_pickle()`

> 데이터를 pickle 형식으로 저장하는 것

```python
def dump_dataframes(dataframes):
    # 데이터를 pickle 형식으로 저장하는 것
    pd.to_pickle(dataframes, DUMP_FILE)
```





### `df.value_counts()`

> 1차원의 히스토그램을 계산한다.
>
> 동일한 요소들이 몇 개나 있는지 출력한다.
>
> 각 요소별로 개수를 출력한다면 히스토그램을 작성할 수 있다.

```python
df.value_counts(subset=None, normalize=False, sort=True, ascending=False)
```

- 설명
  - normalize
    - default = False
    - True 일 경우에는 요소별 개수가 아닌 비율로 변환해서 출력한다.
  - sort
    - default = True
    - 결과에 대한 정렬 수행
  - ascending
    - default = False
    - True, 오름차순 정렬
  - subset
    - 선택적 입력이 가능하며, 고유한 열을 계산할 때 사용한다.

```python
def get_most_review_user(dataframes, n=20):
    reviews = dataframes['reviews']
    reviews = reviews.value_counts(['user'])
    # 여기까지만 하면 정렬이 되어서 출력된다.
    
    print(type(reviews))
    # <class 'pandas.core.series.Series'>
```

- **주의할 점!!!!!**

  - value_counts() 의 결과는 series 로 나온다.
    이 친구의 결과는 인덱스로 나오게 된다.

    ```shell
    user  
    469245    461
    243883    389
    328775    380
    391794    362
    74999     352
    ```

    - 저기에 있는 값들은 인덱스로 되어있다.

    - 따라서 `review[0]` 으로 접근하면 된다.

    ```python
    for i, review in user_most_reviews.iterrows():
            print(
                "{rank}위 : {user}({review_cnt}개)"
                .format(rank = i + 1, user = review.user, review_cnt = review[0])
            )
    ```

    ```shell
    [최다 리뷰어]
    1위 : 469245(461개)
    2위 : 243883(389개)
    3위 : 328775(380개)
    4위 : 391794(362개)
    5위 : 74999(352개)
    ...
    ```

    

