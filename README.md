from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import pyautogui
import time

# 로그인 정보
myID = '김동규'
myPW = '8292'

# 1. webdriver 객체 생성
options = Options()
# 브라우저가 자동으로 닫히지 않도록 설정
options.add_experimental_option("detach", True)

service = Service(ChromeDriverManager().install())
driver = webdriver.Chrome(service=service, options=options)

# 2. 페이지 접근
driver.get("http://kosmo.atosoft.org/worknet/Slogin.asp")
driver.maximize_window()
driver.implicitly_wait(2)  # 페이지가 모두 로드될 때까지 대기

# ID 입력
try:
    name_input = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.NAME, 'strSName'))
    )
    name_input.click()
    name_input.send_keys(myID)
    print("이름 입력 완료:", myID)

    # 좌표에서 마우스 클릭 (Point(x=863, y=372) 예시)
    time.sleep(1)  # 자동완성 목록이 나타날 시간을 주기 위해 잠시 대기
    pyautogui.click(863, 400)
    print("자동완성 항목 클릭 완료")

except Exception as e:
    print("ID 입력 실패:", e)

# Password 입력
try:
    pw_element = WebDriverWait(driver, 10).until(
        EC.presence_of_element_located((By.ID, 'strLoginPwd'))
    )
    pw_element.click()
    pw_element.send_keys(myPW)
    print("비밀번호 입력 완료:", myPW)
except Exception as e:
    print("비밀번호 입력 실패:", e)

# 로그인 버튼 클릭
try:
    login_button = WebDriverWait(driver, 10).until(
        EC.element_to_be_clickable((By.CSS_SELECTOR, "input.btn.btn-info.block.full-width"))
    )
    login_button.click()
    print("로그인 버튼 클릭 완료")
except Exception as e:
    print("로그인 버튼 클릭 실패:", e)

# 코드가 종료되지 않도록 잠시 대기
time.sleep(10)
