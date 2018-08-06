---
layout: post
title: 사이킷런을 활용한 머신러닝 - 선형 회귀 분석
category: Python
tag: Scikit-learn
---

#사이킷런을 활용한 선형 회귀 분석
<hr/>
<div class="message">
데이터 셋
  * [OECD 웹사이트, Better Life Index]: (https://gwangjonghan.github.io/datasets/180807/lifesat/oecd_bli_2015.csv)
  * [IMF 웹사이트, 1인당 GDP 통계]: (https://gwangjonghan.github.io/datasets/180807/lifesat/gdp_per_capita.csv)
</div>

##데이터 정제
```python
import pandas as  pd

oecd_bli = pd.read_csv("https://gwangjonghan.github.io/datasets/180807/lifesat/oecd_bli_2015.csv",thousands=",")
gdp_per_capita =pd.read_csv("https://gwangjonghan.github.io/datasets/180807/lifesat/gdp_per_capita.csv",
                            thousands=',',delimiter='\t',encoding='latin1', na_values="n/a")
```

```python
def prepare_country_stats(oecd_bli, gdp_per_capita):
  oecd_bli= oecd_bli[oecd_bli["INEQUALITY"]=="TOT"]
  oecd_bli= oecd_bli.pivot(index="Country", columns="Indicator", values="Value")
  gdp_per_capita.rename(columns={"2015": "GDP per capita"}, inplace=True)
  gdp_per_capita.set_index("Country", inplace=True)
  full_country_stats= pd.merge(left=oecd_bli, right=gdp_per_capita,
  left_index=True, right_index=True)
  full_country_stats.sort_values(by="GDP per capita", inplace=True)
  remove_indices= [0, 1, 6, 8, 33, 34, 35]
  keep_indices= list(set(range(36)) -set(remove_indices))
  return full_country_stats[["GDP per capita", 'Life satisfaction']].iloc[keep_indices]
```