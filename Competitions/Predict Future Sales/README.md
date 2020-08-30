# Predict Future Sales

## 2020-08-30 요약

### Feature Engineering

1. shop_id는 그냥 60차원으로 넣어보기

2. shop importance는 전체 판매량과 판매액을 normalize해서 넣기

3. item_category는 category별 판매량 normalize해서 넣기

4. item importance도 전체 판매량 normailze해서 넣기

5. date_block_num, year, month는 그대로 넣기

6.
- item_price는 globally average해서 넣기<br>
- item 가격을 정규화해서 해당 item이 해당 상점에서 얼마나 차이가 있는지를 (-1, 1)의 값으로 나타냄. (item price locality)

/* 7. city는 필요없을듯. (앞에서 다룬 다른 feature들이 city 관련 정보를 포함하고 있음) */
 
8. Oct. 2015랑 Nov. 2013/2014의 item_cnt 3개 넣기.

9. item_name에 대한 빈도분석을 시행한 후, 몇 개의 top feature를 사용할지 선택해서 CountVectorizer를 사용.

/* 10. skip. */

## 2020-08-22 요약

### Feature Engineering

#### Q. 어떤 feature를 뽑아서 input data를 만들것인가?
1. shop_id (categorical feature)
  - one-hot encoding을 사용하면 60차원으로 표시가 됨.
  - 그냥 숫자로 넣으면 regression task에서 Loss 계산시 절대값 자체가 의미를 가지게 됨.

2. shop importance (numeric feature)
  - 어떤 매장이 판매량에 영향을 주는지에 대한 중요도
  - 판매량이 높은순

3. item_category (categorical feature)
  - item_id를 넣을 수는 없으니, 좀 더 적은 종류의 item_category를 넣는다.
  - one-hot encoding을 사용하면 84차원으로 표시가 됨.
  - 그냥 숫자로 넣으면 regression task에서 Loss 계산시 절대값 자체가 의미를 가지게 됨.

4. item importance (numeric feature)
  - shop importance와 비슷하게 어떤 item이 판매량이 높았는지에 대한 정보
  - 판매량이 높은 item이라면 Nov 2015에도 어느정도 판매량이 높을 것이다.

5. 시간정보 (numeric feature)<br>
  5.1. date_block_num = Jan 2013부터 Oct 2015까지 0부터 33으로 numbering<br>
  5.2. year —> date_block_num//12 (numeric)<br>
  5.3. month —> date_block_num%12 (numeric)<br>
  5.x. seasonality? (넣는다면 같은 해의 대한 정보와 다른 해의 같은 달에 대한 정보)<br>

6. 가격정보 (mixed-type feature)<br>
  6.1. item_price값 자체 (numeric)<br>
  6.2. price importance(상대적으로)높은 가격 / 중간 가격 / 낮은 가격 등으로 분위를 나누어서 중요도 측정 (categorical)<br>
  6.3. price increasing rate를 이용하여 해당 상점인근에서 얼마나 가격이 올랐는지에 대한 정보 (numeric)<br>

7. city (categorical feature)
  - 해당 지역에 대한 정보.
  - EDA notebook에서 해당 정보 추출

8. Lag Feature (Previous Value Prediction)
  - 직전달의 item_cnt (numeric)
  - item_id별로 전체기간동안 몇개나 팔렸는지의 정보를 통한 중요도 (categorical)
  - Oct 2015의 item_cnt
  - Nov 2014, Nov 2013의 item_cnt
  
9. Text Features (numeric vector)
  - BOW vectorization을 통해 해당 item의 거래가 얼마나 많은 날에 발생했는지 확인
  - TF-IDF는 왜했을까? (모든 거래에서 item_name의 TF는 1이고, IDF 정보만 사용. 하지만 전체빈도 top25만 feature로 사용)

10. Item selling features (numeric vector)
  - 전체 34개의 기간(monthly)동안 각 아이템이 전체 판매량에서 차지하는 비율을 통해, best seller인지 steady seller인지 poor seller인지 확인.

