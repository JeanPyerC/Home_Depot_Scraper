# Home Depot Scrapper

### Project

Recently, a client contacted me to request a website scraping service aimed at extracting specific information that could be used to analyze new trends and prices. After a thorough discussion regarding project requirements, we created a list of items to extract. This included details such as who was selling the product, product descriptions, pricing information, customer rating percentages, and promotional details. With this data at our disposal, we could analyze trends, pricing, and reviews to better plan for material sourcing.


### Step 1 - Know On What To Extract

To extract the desired details, we should begin by visiting the website and examining the available data. I recommend starting with small extractions of each line of code before moving on to adding loops or additional formatting. It's important to know exactly what we're targeting, such as each individual pod within a particular field (as seen in the photo below). If we were to target the entire page, we could inadvertently extract extraneous information such as "Related Products," "Other Customer Searches," and "Customers Also Bought These Items." These details are not relevant to our needs and could potentially obscure the data we're trying to extract.


### Step 2 - Extracting The Details

Once we have an idea on what to extract, we can start by extracting each pods using BeatifulSoup find_all method. This will scrape everything within that class allowing us to better extract our targets, we will call this field 'company_detail'. 

For the next example, lets target and extract the description of the product. To this we need to target the following element within **company_detail**, and lets add a 'if' statment just in case there's a product missing. If there is, we will leave it blank. We will call this, product_list. 

To better undetstand: Your First Line is extracting every pod detail information. Your second line is going into each pod and extracting the product details, if there is a pod with an empty field, it will replace it with 'None' so the code won't break or over lap. 


```
company_detail = soup.find_all("div", class_="browse-search__pod col__12-12 col__6-12--xs col__4-12--sm col__3-12--md col__3-12--lg")

product_list = [comp.find("span",class_="product-header__title-product--4y7oa").text if comp.find("span",class_="product-header__title-product--4y7oa") is not None else "" for comp in company_detail]
```

![](https://github.com/JeanPyerC/Home_Depot_Scraper/blob/main/HomeDepot_Scrape%20-%20Complete/Photos/Photo01.png)

### Step 3 - Save the data, and loop it.

Once we know the code is working, the next step is to append the data into an empty list, and to repeat the process for each page. Please keep in mind, every website is different. In this case, if you are in the end page and if you click next, it's going to bring you back to page one causing a forever loop. To avoid this; I created an IF statment where if the total Page Number is not the same as the current Page Number, to continue as is. 

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

### Step 4 - Graph the data, and show case it.

In the final step, I created a visually appealing summary of the extracted details. The goal was to create a key that not only provides essential information but also engages the audience by telling a compelling story. To achieve this, I utilized Power BI to present the data in an interactive manner. Users can now easily filter the information based on Color, Size, and Tags, allowing them to gain valuable insights into competitor pricing and customer interests. Additionally, they can observe the website's performance. I thoroughly enjoyed working on this project, and I am delighted that the client is satisfied with the outcome.


![](https://github.com/JeanPyerC/Home_Depot_Scraper/blob/main/HomeDepot_Scrape%20-%20Complete/Photos/Photo04.png)

![](https://github.com/JeanPyerC/Home_Depot_Scraper/blob/main/HomeDepot_Scrape%20-%20Complete/Photos/Photo02.png)



