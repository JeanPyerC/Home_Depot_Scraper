# Home Depot Scrapper

### Project

A client contacted me requesting a website scraping service to extract specific information for examining new trends and prices. We had a discussion on the requirements for this project and came up with a list of items to extract, including who is selling the product, product descriptions, pricing, customer rating percentages, and promo details. By collecting this data, we can analyze trends, prices, reviews, and plan for better material sourcing. Additionally, we added a feature to search for specific text before running the report, which in this case, was "Floor Tiles."


### Step 1 - Know On What To Extract

We need to visit the website and see what data we have to use to extract the following details. I always suggest to start small by extracting each line of code before adding in loops or addtional formatting. We also need to know what we are targeting, for example, in the photo below, we need to target each pods within this field. If we target the entire page, we will be extracting "Related Porducts", "Other Customers Search", and "Customers Also Bought These Items". Those information will not be needed, and if extracted, if could obscure our data.  


### Step 2 - Extracting The Details

Once we have an idea on what to extract, we can start by extracting each pods using BeatifulSoup find_all method. This will scrape everything within that class allowing us to better extract our targets, we will call this field 'company_detail'. 

For the next example, lets target and extract the description of the product. To this we need to target the following element within **company_detail**, and lets add a 'if' statment just in case there's a product missing. If there is, we will leave it blank. We will call this, product_list. 

To better undetstand: Your First Line is extracting every pod detail information. Your second line is going into each pod and extracting the product details, if there is a pod with an empty field, it will replace it with 'None' so the code won't break or over lap. 


```
company_detail = soup.find_all("div", class_="browse-search__pod col__12-12 col__6-12--xs col__4-12--sm col__3-12--md col__3-12--lg")

product_list = [comp.find("span",class_="product-header__title-product--4y7oa").text if comp.find("span",class_="product-header__title-product--4y7oa") is not None else "" for comp in company_detail]
```

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

### Step 4 - Graph the data, and show case it




