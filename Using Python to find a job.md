Using Python to find a job


After I got the dreamed visa, I had to plan a new life. The biggest difficulty would be to find a nice employment. Although I had ten years experience of coding, I still felt uncertain about the job market in the new country. And I'd like to put my energy into the field that I'm really curious about. Then I came up an idea, which is using Python to find a job. Hope good ending is waiting for me in the near future.

The first thing of finding a job is to figure out which kind of companies that I'd like to apply. So I open the official website and I saw many links of accredited employers. There are more than one thousand companies in the websites. So my goal is using python to get the company list and open these companies' website automatically. Therefore I can choose companies I liked and find vacacies.

First, install Python. The popular intergrated IDE for Python in China is Anaconda. You can download Anaconda from the website below: 
https://www.anaconda.com/distribution/ 

Second, using Jupyter to create the first code. Open Anaconda Prompt by clicking Windows Start Menu. Then we can see a terminal like this:
![image](https://github.com/waitfuing/pythonDoc/blob/master/640.webp)

We can change the working directory. I've change it to pythonWorkspace. Then type jupyter notebook to launch Jupyter.
![image](https://github.com/waitfuing/pythonDoc/blob/master/641.webp)

Then we can see the jupyter working environment like this:
![image](https://github.com/waitfuing/pythonDoc/blob/master/642.webp)

Jupyter is an interactive IDE for python and it's really friendly for beginners. We can run our codes immediately to see the results and we can even change the running order.

Third, using python to get the source code of a website:
Click new on the top right corner of Jupyter. We need to import requests lib first, then just type requests.get(url) and we can get the source code of the address in most cases.

import requests
url = 'https://www.**/accredited-employers-list'
res = requests.get(url).text
print(res)
Then click run on the top menu, we can see the results we want after a few seconds like below:
![image](https://github.com/waitfuing/pythonDoc/blob/master/643.webp)


It's really simple. is it? Sometimes we can't get the result since some websites refuse unallowed browsers. So we can use headers to fake requests.

import requests
headers = {'User-Agent':'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36'}
url = 'https://www.***/accredited-employers-list'
res = requests.get(url, headers = headers).text
print(res)


After received text content from the website, we need to filter unuseful information and extract websites address and type of companies from the result.



How to get addresses? We have to analyse the result first. We can see what we want, including websites address and company type are both exist in the "secondary_data" para. So we can use another lib to extract "secondary_data" called re.



Forth, extract useful information, like website, company type:

We can use re.findall function to get all websites from the result.

import re
import pandas as pd
text = '"secondary_data": .*?"url": "(.*?)"'
info = re.findall(text, res, re.S)
![image](https://github.com/waitfuing/pythonDoc/blob/master/644.webp)

Then we can use info list to open these websites or store the websites to excel by iterating info variable. We need to import webbrowser lib to finish this job. But be careful! There are more than one thousand websites in info. So we'd better not open too many websites at once.

import webbrowser
for i in range(10):
    webbrowser.open(info[i], new=2, autoraise=True) //new=2 means open a new tab.


As you known, there are so many addresses, so we need not only the address but also the company type to help up filter companies that we don't need. So we can use another function in re.lib which is finditer. 

We can see in every "secondary_data" field, there are two "text" fields  which contains company type and website, so we can use finditer to create an interator and put contents of two "text" field into two lists at same time.

import re
text = '"secondary_data": .*?"url": "(.*?)"'
info = re.finditer(text, res, re.S)

address = []
industry = []
for i in info:
    desc = re.findall('"text": "(.*?)"', i.group(0), re.S)
    address.append(desc[1])
    industry.append(desc[0])
print(address)
print(industry)
Then we can see exactly what we want in pair.
![image](https://github.com/waitfuing/pythonDoc/blob/master/645.webp)




Fifth, how to store these results to excel then we can use them anytime when we need. It's really simple by using Pandas.DataFrame.

import pandas as pd
import os

structure = pd.DataFrame()
structure['Address'] = address
structure['Industry'] = industry
if not os.path.exists('companies.xlsx'):
    structure.to_excel('companies.xlsx')
Then we can see companies.xlsx in our working directory like this:
![image](https://github.com/waitfuing/pythonDoc/blob/master/646.webp)


Open it, we can see exactly what we want:
![image](https://github.com/waitfuing/pythonDoc/blob/master/647.webp)




Sixth, then we can delete some companies that we're not interested in  and save the resultes in another sheet. And we can open websites of companies that we are really interested and search vacancies. Then we can use xwings lib to help us finish reading excel files.

import xlwings as xw
import webbrowser
app = xw.App(visible=False)
wb = app.books.open('companies.xlsx')
sht = wb.sheets['Sheet2']

for cell in sht.range('B2:B3'):
    webbrowser.open(cell.value, new=2, autoraise=True)
wb.close()
app.quit()
Finally we can get what we want now. Although we can also code more to help us find vacancies pages, for instance, using keywords like career/vacacy/job, there are too many issues that we need to consider first. Therefore, we can leave this question in next lesson:)



Hope this page can help you~
