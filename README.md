# price_tracker_python_project.py
web_scrapping_using_bs4
import requests
from bs4 import BeautifulSoup

products_to_track = [
    {
        "product_url": "https://www.amazon.in/Samsung-Galaxy-Prime-Light-Blue/dp/B0BD3T6Z1D/ref=dp_prsubs_3?pd_rd_w=BRf3F&content-id=amzn1.sym.993b0821-8ecd-4fad-8eef-d1599368ca9c&pf_rd_p=993b0821-8ecd-4fad-8eef-d1599368ca9c&pf_rd_r=5EZ096B2W4AWE6GRBFJ4&pd_rd_wg=x7Z3R&pd_rd_r=eb882657-2145-4b72-b23b-bee2652a4b6f&pd_rd_i=B0BD3T6Z1D&psc=1",
        "name": "Samsung M32",
        "target_price": 12000
    },
    {
        "product_url": "https://www.amazon.in/Samsung-Galaxy-Prime-Light-128GB/dp/B0BD3V985M/ref=sr_1_12?crid=5K0RSWTMYA6G&keywords=samsung+galaxy&qid=1676973278&s=electronics&sprefix=samsung+g%2Celectronics%2C971&sr=1-12",
        "name" : "Samsung galaxy M32",
        "target_price": 16000
    },
    {
        "product_url": "https://www.amazon.in/Samsung-Storage-6000mAh-Purchased-Separately/dp/B09TWH8YHM/ref=sr_1_10?crid=5K0RSWTMYA6G&keywords=samsung+galaxy&qid=1676973278&s=electronics&sprefix=samsung+g%2Celectronics%2C971&sr=1-10",
        "name": "Samsung M33",
        "target_price": 17000
    },
    {
        "product_url": "https://www.amazon.in/Samsung-Galaxy-Silver-128GB-Storage/dp/B0BS1673FT/ref=sr_1_9?crid=5K0RSWTMYA6G&keywords=samsung+galaxy&qid=1676973278&s=electronics&sprefix=samsung+g%2Celectronics%2C971&sr=1-9",
        "name": "Samsung galaxy A23",
        "target_price": 23000
    },
    {
        "product_url": "https://www.amazon.in/Samsung-Galaxy-Storage-6000mAh-Battery/dp/B0B4F2XCK3/ref=sr_1_13?crid=5K0RSWTMYA6G&keywords=samsung+galaxy&qid=1676973278&s=electronics&sprefix=samsung+g%2Celectronics%2C971&sr=1-13",
        "name": "Samsung galaxy M13",
        "target_price": 14000
    }
]

def give_product_price(URL):
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.0.0 Safari/537.36 Edg/110.0.1587.46"
    }
    page = requests.get(URL, headers=headers)
    soup = BeautifulSoup(page.content, 'html.parser')

    product_price = soup.find(id="tp_price_block_total_price_ww")
    # if (product_price == None):
    #     product_price = soup.find(id="priceblock_ourprice")



    return product_price.getText()

result_file = open('my_result_file.txt','w')

try:
    for every_product in products_to_track:
        product_price_returned = give_product_price(every_product.get("product_url"))
        print(product_price_returned + " - " + every_product.get("name"))

        my_product_price = product_price_returned[10:]
        my_product_price = my_product_price.replace(',', '')
        my_product_price = int(float(my_product_price))

        print(my_product_price)
        if my_product_price < every_product.get("target_price"):
            print("Available at your required price")
            result_file.write(every_product.get(
                "name") + ' -  \t' + ' Available at Target Price ' + ' Current Price - ' + str(my_product_price) + '\n')

        else:
            print("Still at current price")

finally :
    result_file.close()
