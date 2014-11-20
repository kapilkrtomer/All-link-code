All-link-code
=============

import requests
from bs4 import BeautifulSoup
from lxml import html
import phan_proxy
import time
import req_proxy
import scrolldriver

import os

def main1(link,directory,doc_name):
    page = scrolldriver.supermain(link)
    soup = BeautifulSoup(page,"html.parser")

    filename ="%s/%s.doc"%(directory,doc_name)
    f = open(filename,"a+")

    all_link = soup.find("div",attrs={"id":"e_matrix"}).find_all("li")    
    for l1 in all_link:
        try:
            l2 = l1.find("div",attrs={"class":"qoutter"}).a.get("href")
            f.write(str(l2) + "\n")

        except:
            pass

    f.close()

def main(link):
    page = req_proxy.main(link)
    #driver = phan_proxy.main(link)
    #page = driver.page_source

    #driver.delete_all_cookies()
    #driver.quit()
   
    soup =BeautifulSoup(page,"html.parser")

    directory = "zivame%s" %(time.strftime("%d%m%Y"))

    try:
        os.makedirs(directory)

    except:
        pass


    all_link = soup.find("a",text="Brands").find_parent("li").find_all("li")
    for l1 in all_link:
        try:
            doc_name =l1.a.get_text().replace(" ","_").replace(".","_").replace("-","_").strip()
            link ="http://www.zivame.com%s" %(l1.a.get("href"))
            main1(link,directory,doc_name)

        except:
            pass




def supermain():
    main("http://www.zivame.com")



if __name__ == "__main__":
    supermain()


