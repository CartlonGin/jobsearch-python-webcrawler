# WebScraper/Selenium_jobsearch_project

Imagine that you are searching for all the job opportunities. However, there are more than 20 categories of jobs and so many web pages that you have to spend a lot of time on browsing all the opportunities. It can be more efficient by scraping all the information with sort and format. In this case, there are lots of pages and buttons should be controlled, that is the reason why I would apply Selenium to scrape the web. Let's get started!

#### What is the purpose?
Automatically scrape all the job titles and links by different categories, and organize into an excel file.
#### Tech I use:
Python/Selenium/Openpyxl
#### Web:
https://www.adecco.com.tw/advancedsearch.aspx?search=1

   * Based on the web browser you used, apply the corresponding driver to control the behavior of the web browser. 
   ```js
   from selenium import webdriver
   from selenium.webdriver.chrome.service import Service
   from webdriver_manager.chrome import ChromeDriverManager
   from selenium.webdriver.support.ui import WebDriverWait
   from selenium.webdriver.support import expected_conditions as EC
   from selenium.webdriver.common.by import By
   
   s = Service(ChromeDriverManager().install())
   driver = webdriver.Chrome(service=s)
   driver.get("https://www.adecco.com.tw/advancedsearch.aspx?search=1")
   ```
   
   * First, scrape all the pages in the same category. Using ```def``` function to automatically turn to next page. When checking DevTools of the website, you can find the difference of code: ```ctl00_ContentPlaceHolder1_ucSearchResults1_rptPaging_ctl```. Create the next_page function to turn to the next page in the same category.
   ```js
   def next_page(driver, page_num):
    try:
        page_id_name = f"ctl00_ContentPlaceHolder1_ucSearchResults1_rptPaging_ctl{next_id(page_num)}_lnkButtonPaging"
        next_page_button = WebDriverWait(driver, 5).until(
            EC.element_to_be_clickable((By.ID, page_id_name)))
        next_page_tf = True
    except:
        next_page_button = None
        next_page_tf = False
    page_list = [next_page_tf, next_page_button]
    return  
   ```
   
   * Second, scrape the next category. Using ```try``` function to ensure all the category will be scraped. Once we get all job titles and links under a category, it will print 'Done' and move on to the next category.
   ```js
   while category != None:
    try:
        category_id = f"ctl00_ContentPlaceHolderLeftNav_ucJobSearchFilter1_rptClassification_ctl{next_id(n)}_lbLink"
        category_name = driver.find_element(By.ID, f"ctl00_ContentPlaceHolderLeftNav_ucJobSearchFilter1_rptClassification_ctl{next_id(n)}_lbLink")\
            .get_attribute('textContent')
        category_button = WebDriverWait(driver, 5).until(
            EC.element_to_be_clickable((By.ID, category_id)))
        category_button.click()
        time.sleep(5)

        t = 1
        page_end = False

    except:
        category = None
        driver.refresh()
        print('抓取結束')
   ```
   * Third, sequentially write all the results in an excel file by using ```openpyxl```. Create an excel file with column 'job' and 'job_link'.
   ```js
   wb = openpyxl.Workbook()
   ws = wb.active 
   ws.title = 'jobsearch'
   ws['A1'] = 'Job'
   ws['B1'] = 'job_link'
   ```
