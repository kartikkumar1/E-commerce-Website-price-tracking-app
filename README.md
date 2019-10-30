# Amazon-Price-Tracking-App
#Tracks price of a particular product on Amazon and send an email alert when price falls below a desired amount.

import requests
from bs4 import BeautifulSoup

from re import sub
from decimal import Decimal

import smtplib


URL='https://www.amazon.in/Redmi-6A-Gold-32GB-Storage/dp/B07DJD1H1C/ref=sr_1_3?keywords=mi+6a&qid=1562494025&s=gateway&sr=8-3'

headers = {"User Agent":'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/75.0.3770.100 Safari/537.36'}


def check_price():
    page = requests.get(URL,headers=headers)
    soup= BeautifulSoup(page.content,"html.parser")

    title = soup.find(id = "productTitle").get_text()
    price = soup.find(id="priceblock_dealprice").get_text()

    money= price
    converted_price = Decimal(sub(r'[^\d.]', '', money))

    print(title.strip())
    print(converted_price)
    if(converted_price<6500.00):
        send_email()

def send_email():
    server=smtplib.SMTP('smtp.gmail.com',587)
    server.ehlo()
    server.starttls()
    server.ehlo()

    server.login('UserEmail1','UserEmail1_password')

    subject = 'Price fell down!'

    body =  'check amazon website /"https://www.amazon.in/Redmi-6A-Gold-32GB-Storage/dp/B07DJD1H1C/ref=sr_1_3?keywords=mi+6a&qid=1562494025&s=gateway&sr=8-3"'  #website of desired Product

    msg = f"subject: {subject}\n\n{body}"

    server.sendmail('UserEmail1','UserEmail2',msg)
    print('Email has been sent')

    server.quit()

check_price()


##***** google less secure app access
