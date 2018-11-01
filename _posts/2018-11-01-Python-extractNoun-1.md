

```python
import pandas as pd
import numpy as np
import re
from konlpy.tag import *
```


```python
data = pd.read_csv('D://r_crawl//dohyeon_utf8.csv')
```


```python
df = data.drop_duplicates(['title'])
```


```python
df = df.drop_duplicates(['body'])
```


```python
df = df.dropna(axis=0)
```


```python
df.index=np.arange(0,len(df),1)
```


```python
df.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>no</th>
      <th>title</th>
      <th>date</th>
      <th>body</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>22698999</td>
      <td>런닝하는 월드컵대표팀</td>
      <td>입력 2018.06.08 19:36</td>
      <td>【레오강(오스트리아)=뉴시스】고범준 기자 = 2018 러시아월드컵에 출전하는 한국...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22699000</td>
      <td>손흥민  가벼운 몸으로</td>
      <td>입력 2018.06.08 19:35</td>
      <td>【레오강(오스트리아)=뉴시스】고범준 기자 = 2018 러시아월드컵에 출전하는 한국...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>22699001</td>
      <td>고요한  내가 간다!</td>
      <td>입력 2018.06.08 19:36</td>
      <td>【레오강(오스트리아)=뉴시스】고범준 기자 = 2018 러시아월드컵에 출전하는 한국...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>22699002</td>
      <td>훈련하는 문선민</td>
      <td>입력 2018.06.08 19:36</td>
      <td>【레오강(오스트리아)=뉴시스】고범준 기자 = 2018 러시아월드컵에 출전하는 한국...</td>
    </tr>
    <tr>
      <th>5</th>
      <td>22699004</td>
      <td>훈련하는 구자철</td>
      <td>입력 2018.06.08 19:36</td>
      <td>【레오강(오스트리아)=뉴시스】고범준 기자 = 2018 러시아월드컵에 출전하는 한국...</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.tail()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>no</th>
      <th>title</th>
      <th>date</th>
      <th>body</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>103586</th>
      <td>22848971</td>
      <td>보수매체 적극 활용 '상고법원 집중 선전' 전방위 전략</td>
      <td>입력 2018.07.31 17:38</td>
      <td>【서울=뉴시스】심동준 기자 = '양승태 행정처'가 숙원 사업인 상고법원 도입을 위...</td>
    </tr>
    <tr>
      <th>103587</th>
      <td>22848983</td>
      <td>말년병장도 지원하세요 ‘드론 방송항공 촬영전문가 과정’</td>
      <td>입력 2018.07.31 17:39</td>
      <td>【서울=뉴시스】 박대로 기자 = 한국방송카메라감독연합회 산하 한국방송영상교육원이 ...</td>
    </tr>
    <tr>
      <th>103588</th>
      <td>22848984</td>
      <td>훈련위해 이동하는 단일팀 선수</td>
      <td>입력 2018.07.31 17:40</td>
      <td>【충주=뉴시스】인진연 기자 = 31일 충북 충주시 탄금호 조정경기장에서 2018 ...</td>
    </tr>
    <tr>
      <th>103589</th>
      <td>22848993</td>
      <td>"경찰 아저씨 구해줘서 고마워요" 캘리포니아 산불서 살아난 새끼사슴</td>
      <td>입력 2018.07.31 17:41</td>
      <td>【캘리포니아고속도로순찰대·AP/뉴시스】 미국 캘리포니아고속도로순찰대 소속 데이비드...</td>
    </tr>
    <tr>
      <th>103590</th>
      <td>22848994</td>
      <td>"우리는 엄지 척"</td>
      <td>입력 2018.07.31 17:41</td>
      <td>【충주=뉴시스】인진연 기자 = 31일 충북 충주시 탄금호 조정경기장에서 2018 ...</td>
    </tr>
  </tbody>
</table>
</div>



### title cleansing


```python
lst_title = list(df['title'])
```


```python
lst_title = list(map(lambda x : re.sub('[-=+,#/\?:^$.@*\"※~&%ㆍ!』\\‘|\(\)\[\]\<\>`\'…》]','', x).strip(),lst_title))
lst_title = list(map(lambda x : re.sub('"','', x),lst_title))
lst_title = list(map(lambda x : re.sub("'",'', x),lst_title))
lst_title = list(map(lambda x : re.sub(" ",'', x),lst_title))
```

### date cleansing


```python
lst_date = list(df['date'])
```


```python
lst_date = list(map(lambda x : re.sub('입력 ','',x).strip(),lst_date))
lst_time = list(map(lambda x : x[11:16],lst_date))
lst_date = list(map(lambda x : x[0:10],lst_date))
lst_date = list(map(lambda x : x.replace('.','-'),lst_date))
```

### body cleansing


```python
lst_body = list(df['body'])
```


```python
def only_hangul(sentence):
    hangul = re.compile('[^ ㄱ-ㅣ가-힣]+') # 한글과 띄어쓰기를 제외한 모든 글자
    # hangul = re.compile('[^ \u3131-\u3163\uac00-\ud7a3]+')  # 위와 동일
    result = hangul.sub('', sentence) # 한글과 띄어쓰기를 제외한 모든 부분을 제거
    return (result)
```


```python
lst_body = list(map(lambda x : only_hangul(x), lst_body ))
```

### extract noun


```python
twit = Twitter()
```

    C:\ProgramData\Anaconda3\lib\site-packages\konlpy\tag\_okt.py:16: UserWarning: "Twitter" has changed to "Okt" since KoNLPy v0.4.5.
      warn('"Twitter" has changed to "Okt" since KoNLPy v0.4.5.')
    


```python
title_noun = list(map(lambda x: twit.nouns(x),lst_title))
```


```python
body_noun = list(map(lambda x: twit.nouns(x), lst_body))
```

### export noun_df


```python
lst_no = list(df['no'])
```


```python
noun_df = pd.DataFrame(data={'no':lst_no,
                             'date': lst_date,
                             'time' : lst_time,
                             'title': title_noun,
                             'body' : body_noun})
```


```python
pd.DataFrame.to_csv(noun_df,'d://r_crawl/dohyeon_noun_df.csv',encoding='utf8')
```
