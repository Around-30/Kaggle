# Predict Future Sales

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

5. 시간정보 (numeric feature)
  5.1. date_block_num = Jan 2013부터 Oct 2015까지 0부터 33으로 numbering
  5.2. year —> date_block_num//12 (numeric)
  5.3. month —> date_block_num%12 (numeric)
  5.x. seasonality? (넣는다면 같은 해의 대한 정보와 다른 해의 같은 달에 대한 정보)

6. 가격정보 (mixed-type feature)
  6.1. item_price값 자체 (numeric)
  6.2. price importance(상대적으로)높은 가격 / 중간 가격 / 낮은 가격 등으로 분위를 나누어서 중요도 측정 (categorical)
  6.3. price increasing rate를 이용하여 해당 상점인근에서 얼마나 가격이 올랐는지에 대한 정보 (numeric)

7. city (categorical feature)
- 해당 지역에 대한 정보.
- EDA notebook에서 해당 정보 추출


item_cnt

item_id별로 전기간에 몇개 팔렸냐로 분위

item이 steady seller냐 아니냐
33개 개월의 normalized itemcnt

#전년도 같은달 item_cnt?
$바로 전 달 item_cnt?
