# selenium 버전에 따라서 
- 드라이버 생성

```python
from selenium import webdriver
from bs4 import BeautifulSoup
import pandas as pd

#이거 두개 불러와야함 
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.common.by import By

#driver_path = r'D:\Study\Python\Jupyter\chromedriver.exe' #Old
s = Service(r'D:\Study\Python\Jupyter\chromedriver.exe') #New

#driver = webdriver.Chrome(driver_path) #Old
driver = webdriver.Chrome(service=s) #New
```

- 특정 요소 클릭

```python
# #클릭 가능한지 확인
review_page_btn = '#section_review > div.pagination_pagination__2M9a4 > a:nth-child(1)'

# driver.find_element_by_css_selector(review_page_btn).click() #Old
driver.find_element(By.CSS_SELECTOR,review_page_btn).click()  # New
```

# DataFrame 전처리하면서 

- [컬럼 리스트로 만들기](https://www.notion.so/fed1c01a435e4d978731c431ae1908bf)

```python
list = df['컬럼명'].tolist()
list = list(df['컬럼명'])
```

- [데이터프레임 index만 가져오기](https://www.notion.so/fed1c01a435e4d978731c431ae1908bf)

```python
index = df.index
```

- [df.isnull() / df.isna()](https://www.notion.so/fed1c01a435e4d978731c431ae1908bf)

isnull() 메소드는 관측치가 결측이면 True, 결측이 아니면 False의 boollean 값을 반환합니다.

isnull(DataFrame) 과 DataFrame.isnull() 은 동일한 값을 반환. isna()는 이름만 다를뿐 isnull()과 같다 

```python
df.isnull()
pd.isnull(df)
pd.isnull(df['컬럼명'])

#결측치 개수 구하기
df.isnull().sum()

#데이터 프레임에 nan이 있는지 확인하는 방법
df.isnull().values.any() #nan이 존재하면 True, 아니면 False
```

결측치를 채울때 fillna를 사용했는데 생각처럼 잘 되지 않아서 iloc를 이용해서 직접 결측치를 채워넣음 
```
# 결측치 있는 row 확인
samsung_df[samsung_df['채널'].isnull()]

#440, 462
samsung_df.iloc[440]['채널'] = '컴퓨존'
samsung_df.iloc[462]['채널'] = '컴퓨존'
```
