# WebCrawler/Selenium_jobsearch_project

#### Object: Automatically scrape all the job titles and links in different categories, and organize into an excel file.
##### Tech: Python/Selenium/Openpyxl
##### Web: https://www.adecco.com.tw/advancedsearch.aspx?search=1
##### Note: There are many categories in the job list, so the point is to be able to scrape jobs by each different category.
   * First, scrape all the pages in the same category.
    
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
    
   * Second, scrape the next category.
   * Third, sequentially write all the results in an excel file. 
