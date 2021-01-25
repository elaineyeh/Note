# Difference between image and container
**在硬碟裡的就是 image，跑在記憶體裡的就是 container**
## Image
* 唯讀檔案
* 僅安裝必要的應用程序和支援程式

## Container
* 在所有的image上增加一個可寫存儲層，應用程式就是在這個存儲層跑
* 如果Docker 重啟，container就會關掉，資料不會不見，但是不好找，因此要掛上 Volume

## Run the PostgreSQL container
```
docker run --rm   --name pg-docker -e POSTGRES_PASSWORD=postgres -d -p 5432:5432 -v $HOME/docker/volumes/postgres:/var/lib/postgresql/data  postgres
```
```
—rm 跑完後就刪除
—name 你為 image 取的名字
-e 環境變數
-d 在背景執行
-p 5432:5432   port 號  本機 port 號:虛擬機 port 號
-v volume 本機儲存位置:虛擬機儲存位置
```
### Reference:
[Docker Volume](https://phoenixnap.com/kb/docker-image-vs-container)

[Docker image vs container](https://mailtojacklai.medium.com/cs-docker%E7%9A%84%E4%B8%89%E5%80%8B%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5-image-container-%E5%92%8Cregistry-89844595a7a6)
