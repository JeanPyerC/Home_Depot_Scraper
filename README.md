# Home Depot Scrapper

I created a visual appealing summary of the extracted details. The goal was to provide essential information and engage the audience by telling a compelling story. To achieve this, I utilized Power BI to present the data. Users can now easily filter the information based on Color, Size, and Tags, allowing them to gain valuable insights into competitor pricing and customer interests. Additionally, they can observe the website's performance. I thoroughly enjoyed working on this project and am delighted that the client is satisfied with the outcome. 


![](https://github.com/JeanPyerC/Home_Depot_Scraper/blob/main/HomeDepot_Scrape%20-%20Complete/Photos/Photo05.png)

![](https://github.com/JeanPyerC/Home_Depot_Scraper/blob/main/HomeDepot_Scrape%20-%20Complete/Photos/Photo02.png)

### Step 1 - Extracting The Details

Once we have an idea of what to extract, we can start by extracting each pods using BeatifulSoup find_all method. This will scrape everything within that class, allowing us to extract our targets better; we will call this field 'company_detail'. 

Let's target and extract the product's description for the following example. To this, we need to target the following element within **company_detail**, and lets add an 'if' statement just in case there's a product missing. If there is, we will leave it blank. We will call this product_list. 

To better understand: Your First Line is extracting every pod detail information. Your second line is going into each pod and extracting the product details; if there is a pod with an empty field, it will replace it with 'None' so the code won't break or overlap. 


```
company_detail = soup.find_all("div", class_="browse-search__pod col__12-12 col__6-12--xs col__4-12--sm col__3-12--md col__3-12--lg")

product_list = [comp.find("span",class_="product-header__title-product--4y7oa").text if comp.find("span",class_="product-header__title-product--4y7oa") is not None else "" for comp in company_detail]
```

![](https://github.com/JeanPyerC/Home_Depot_Scraper/blob/main/HomeDepot_Scrape%20-%20Complete/Photos/Photo01.png)

### Step 2 - Save the data, and loop it.

Once we know the code is working, the next step is to append the data into an empty list and repeat the process for each page. Please keep in mind every website is different. In this case, if you are on the end page and click next, it will bring you back to page one, causing a forever loop. To avoid this, I created an IF statement where if the total Page Number is not the same as the current Page Number, to continue as is. 

```
df_list = []

cont=True

while(cont):
    pagination_results = soup.find_all("span",class_="results-pagination__counts--number")
    page_count = int(pagination_results[0].text.split("-")[1].strip())

   
    if page_count!=total_count:
        company_detail = soup.find_all("div", class_="browse-search__pod col__12-12 col__6-12--xs col__4-12--sm col__3-12--md col__3-12--lg")
        product_list = [comp.find("span",class_="product-header__title-product--4y7oa").text if comp.find("span",class_="product-header__title-product--4y7oa") is not None else
    
    else:
        company_detail = soup.find_all("div", class_="browse-search__pod col__12-12 col__6-12--xs col__4-12--sm col__3-12--md col__3-12--lg")
        product_list = [comp.find("span",class_="product-header__title-product--4y7oa").text if comp.find("span",class_="product-header__title-product--4y7oa") is not None else "" for comp in company_detail]


    df_list.append(pd.DataFrame(list(zip(product_list)), columns = ['Description']))

cont=False
```

### Step 3 - Graph the data, and showcase it.

I created a visually appealing summary of the extracted details in the final step. The goal was to create a key that provides essential information and engages the audience by telling a compelling story. To achieve this, I utilized Power BI to present the data interactively. Users can now easily filter the information based on Color, Size, and Tags, allowing them to gain valuable insights into competitor pricing and customer interests. Additionally, they can observe the website's performance. I thoroughly enjoyed working on this project and am delighted that the client is satisfied with the outcome.


![](https://github.com/JeanPyerC/Home_Depot_Scraper/blob/main/HomeDepot_Scrape%20-%20Complete/Photos/Photo05.png)

![](https://github.com/JeanPyerC/Home_Depot_Scraper/blob/main/HomeDepot_Scrape%20-%20Complete/Photos/Photo02.png)



