from selenium import webdriver
import time
import bs4 as bs
from selenium.common.exceptions import NoSuchElementException


PATH = "C:\Program Files (x86)\chromedriver.exe"
driver = webdriver.Chrome(PATH)
url = "https://www.bunnings.com.au/our-range/bathroom-plumbing/plumbing/pipe-fittings/pvc-pressure"
driver.get(url)
time.sleep(10)

try:
    while True:
        time.sleep(6)
        element = driver.find_element_by_xpath("//div[@class='view-more_section']//button[@type='button']")
        element.click()

except NoSuchElementException:

    page = driver.page_source
    soup = bs.BeautifulSoup(page, 'html.parser')
    items = soup.find_all('article', class_=["codified-product-tile--featured",
                                             'codified-product-tile'])

    filename = 'bunnings.csv'
    f = open(filename, 'w')

    label = 'Product Name,'
    label2 = ' Price \n'
    f.write(label + label2)

    for item in items:
        titleitem = item.find('span', style="display: block; width: 100%;").text
        priceitem = item.find('span', class_="codified-product-tile__price--value--dollars").text
        priceitemcents = item.find('span', class_="codified-product-tile__price--value--decimal-cents").text

        f.write(titleitem + ',' + '$' + priceitem + priceitemcents + '\n ')
