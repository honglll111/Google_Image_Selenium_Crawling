# 구글 이미지 크롤링
- https://g3lu.tistory.com/28 참고
- 구글에서 유재석 사진 크롤링
  
![image](https://github.com/user-attachments/assets/aeaf3e39-17db-46f6-9cb7-0c707bc5a150)

```
from selenium import webdriver
from selenium.webdriver import ChromeOptions
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
import time
```
```
# 크롬 웹드라이버 실행, 브라우저의 창을 최대화로 설정
options = ChromeOptions()
options.add_argument("--start-maximized")
driver = webdriver.Chrome(options=options)
```
```
# 구글 이미지로 이동
url = 'https://images.google.com/'  
driver.get(url)
```
```
# 원하는 이미지 검색
input_element = driver.find_element(By.CLASS_NAME, "gLFyf")
input_element.send_keys("유재석" + Keys.ENTER)  # 유재석 대신 입력하고 싶은 것을 입력
```
```
# 스크롤 다운
elem = driver.find_element(By.TAG_NAME, 'body')
for i in range(60):
    elem.send_keys(Keys.PAGE_DOWN)
    time.sleep(1)

# 더보기 처리
try:
    view_more_button = WebDriverWait(driver, 10).utill(EC.element_to_be_clickable((By.CLASS_NAME, 'mye4qd')))
    view_more_button.click()
    for i in range(80):
        elem.send_keys(Keys.PAGE_DOWN)
        time.sleep(3)
except:
    pass
```
## 이미지 추출
- class="YQ4gaf"
![image](https://github.com/user-attachments/assets/e31cc553-9cb3-44af-8445-ad757d3a96e3)

- 신문사 로고의 경우 class="YQ4gaf zr758c"라 images = driver.find_elements(By.CSS_SELECTOR, ".YQ4gaf")
- 이를 사용할 경우 위에 유산슬, 유 퀴즈, 압구정 현대 아파트 등의 추천 검색어의 이미지 + 이미지 + 신문사 이미지가 크롤링 되는 문제 발생 -> 해결함
![image](https://github.com/user-attachments/assets/472a7d58-0642-4015-9d4f-5a40cc7dad50)


```
# 이미지 다운로드하기
# 신문사 로고의 경우 YQ4gaf zr758c라 이것도 수집되어, 사진만 추출할 수 있도록 코드 추가함
images = driver.find_elements(By.CSS_SELECTOR, ".YQ4gaf")
images = [img for img in images if img.get_attribute("class") == "YQ4gaf"]  

links = []
for image in images:
    src = image.get_attribute('src')
    if src is None:
        src = image.get_attribute('data-src')  # data-src도 확인
    if src:
        links.append(src)
        
print('찾은 이미지의 개수: ', len(links))
```
```
# 경로 설정하기
import urllib.request
import os

save_path = "D:\\jaesok"  # D 드라이브에 폴더 생성
os.makedirs(save_path, exist_ok=True)  # 폴더가 없으면 생성

for k, i in enumerate(links):
    url = i
    file_path = os.path.join(save_path, f"jaesok{k}.jpg")  # D:\jaesok\jaesok0.jpg 형태로 저장
    urllib.request.urlretrieve(url, file_path)

print('다운로드를 완료하였습니다.')
```
![image](https://github.com/user-attachments/assets/5c08d625-c5fd-4f30-b38b-d5149c561d58)

