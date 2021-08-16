# 批次下載google search 圖片

## 前言:

因為需要一些公開的資料，補足train model data 不足，所以要在 google 上進行搜尋以及下載

## 找到了兩個套件:

### google_images_download ( 好像不支援了)

![image](https://user-images.githubusercontent.com/88547312/129569975-f51f298a-4425-407f-a95f-7c049ecc0ffd.png)

不支援 : https://yanwei-liu.medium.com/%E4%BD%BF%E7%94%A8python%E6%89%B9%E6%AC%A1%E4%B8%8B%E8%BC%89google%E5%9C%96%E7%89%87-5c0a98935e30

實際使用沒有下載成功:
![image](https://user-images.githubusercontent.com/88547312/129570493-04dec0a6-d15e-48a0-81ac-ec1937f3cd7d.png)


原始網站的 doc :
- https://google-images-download.readthedocs.io/en/latest/arguments.html
- https://google-images-download.readthedocs.io/en/latest/installation.html
- https://google-images-download.readthedocs.io/en/latest/index.html

### flickr_scraper (用這個下載成功)

- 原始 github: https://github.com/ultralytics/flickr_scraper

- 安裝流程: 

```
$ git clone https://github.com/ultralytics/flickr_scraper
$ cd flickr_scraper
$ pip install -U -r requirements.txt
```
- 安裝好去註冊 Flickr 
- 註冊完成去 製作一個 API https://www.flickr.com/services/apps/create/apply => 會得到一個key 跟密碼
- 去 flickr_scraper.py 輸入剛剛 API 做好的 key
- 執行下載動作 : python3 flickr_scraper.py --search 'honeybees on flowers' --n 10 --download

![image](https://user-images.githubusercontent.com/88547312/129573076-5877d2ce-95ec-401f-a1b7-18eb80198dfc.png)









