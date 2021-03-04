# ssh to AWS server
專案 a, b 用同一個 docker-compose-template.yml，當我們要新增專案時只要將 docker-compose-template.yml 複製到該專案即可
專案 c 因為要掛上 a, b 的 volumes 及對外連接，所以他自己一個 docker-compose.yml
## Step 1. Open aws EC2
開啟一個伺服器，並下載 .pem 檔

## Step 2. Write docker-compose-template.yml
### 專案 a, b
```
version: '3'
services:
  prj_a:
    image: nginx
    volumes:
      - ${PWD}:/usr/share/nginx/html
```
`${PWD}` 使用環境變數自動帶入

### 專案 c
```
version: '3'
services:
  nginx:
    container_name: aws_middle
    image: nginx
    volumes:
      - /home/ubuntu/conf.d:/etc/nginx/conf.d
    ports:
      - "80:80"
```

## Step 3. Write circleci.yml
```
|__nginx
     |__a
     |  |__.circleci
     |          |__config.yml
     |  |__index.html
     |  |__docker-compose-template.yml
     |  |__docker-compose-a.yml
     |
     |__b
     |  |__.circleci
     |          |__config.yml
     |  |__index.html
     |  |__docker-compose-template.yml
     |  |__docker-compose-b.yml
     |
     |__middle
     |  |__.circleci
     |          |__config.yml
     |  |__docker-compose.yml
```
### 專案 a, b, c
```
version: 2.1
jobs:
  build:
    docker:
      - image: cimg/node:14.10.1 # the primary container, where your job's commands are run
        auth:
          username: mydockerhub-user
          password: $DOCKERHUB_PASSWORD  # context / project UI env-var reference
    steps:
      - checkout # check out the code in the project directory
      - add_ssh_keys:
          fingerprints:
            - "c7:ce:e6:59:d6:34:62:26:8d:ec:11:63:f5:d7:1e:b0" #AWS key

      - run: echo "hello world" # run the `echo` command
      - run: echo $CIRCLE_PROJECT_REPONAME # get the github project name
      - run: envsubst '${CIRCLE_PROJECT_REPONAME}' < deploy.sh > real_deploy.sh # copy the deploy.sh to real_deploy.sh and bring the environment variable
      - run: ssh -o StrictHostKeyChecking=no $USER@$HOST < real_deploy.sh # ssh to AWS server

```

## Step 4. Write deploy.sh
deploy.sh 執行 git pull, docker compose up 等動作，讓網頁跑起來
### 先順過流程再寫才不用一直改來改去
### 因為我們檔案是下 git pull ，所以第一次要手動連線到伺服器 git clone
### 專案 a, b
```
echo $CIRCLE_PROJECT_REPONAME
cd $CIRCLE_PROJECT_REPONAME
export CIRCLE_PROJECT_REPONAME=$CIRCLE_PROJECT_REPONAME
pwd
git pull

mkdir -p ../conf.d
cd ../$CIRCLE_PROJECT_REPONAME
pwd
CONF_FILE=`ls ./*.conf`
echo $CONF_FILE
for CONF in $CONF_FILE
do
   mv $CONF ~/conf.d
done

mkdir -p ../docker-compose
envsubst < docker-compose-template.yml > docker-compose-$CIRCLE_PROJECT_REPONAME.yml
cp docker-compose-$CIRCLE_PROJECT_REPONAME.yml ../docker-compose

cd ../docker-compose
pwd
FILENAME=`ls ./*.yml`
FILE="docker-compose"
for EACHFILE in $FILENAME
do
   FILE+=" -f $EACHFILE"
done
FILE+=" up -d"
echo $FILE
echo $FILE > run.sh
sh run.sh

```
### 專案 c
```
echo $CIRCLE_PROJECT_REPONAME
cd $CIRCLE_PROJECT_REPONAME
pwd
git pull

mkdir -p ../docker-compose
cp docker-compose.yml ../docker-compose

cd ../docker-compose
pwd
FILENAME=`ls ./*.yml`
FILE="docker-compose"
for EACHFILE in $FILENAME
do
   FILE+=" -f $EACHFILE"
done
FILE+=" up -d"
echo $FILE
echo $FILE > run.sh
sh run.sh
```

## Step 5. Write prj.conf
現在我們不要讓專案 c 去連接專案 a, b，要記住一個重點就是`專案 a, b, c 是互相不知道的`，所以我們現在要把專案 a, b 的 aws_prj_a.conf, aws_prj_b.conf 掛到伺服器上，專案 c 的 conf.d 再跟伺服器掛一起
```
|__nginx
     |__a
     |  |__.circleci
     |          |__config.yml
     |  |__index.html
     |  |__docker-compose-template.yml
     |  |__docker-compose-a.yml
     |  |__aws_prj_a.conf
     |
     |__b
     |  |__.circleci
     |          |__config.yml
     |  |__index.html
     |  |__docker-compose-template.yml
     |  |__docker-compose-b.yml
     |  |__aws_prj_b.conf
     |
     |__middle
     |  |__.circleci
     |          |__config.yml
     |  |__docker-compose.yml
```
### prj.conf
```
map $http_upgrade $connection_upgrade {
    default upgrade;
    '' close;
}

server {
	listen       80;
	server_name  one.prj.com; # domain

    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    location / {
        proxy_pass http://project_a; # project_a is docker container name you set in docker-compose.yml
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_set_header Host $host;
	}
}

```

## Step 5. git init
## Step 6. Circleci follow project
## Step 7. Set the environment variable
為了跑 config.yml 可時候可以代數環境變數
## Step 8. Set the Github ssh key
#### 設定 Github ssh key 讓 circle 可以不用輸入密碼就將專案拉下來
#### 將公鑰上傳
[Reference](https://ithelp.ithome.com.tw/articles/10205988)
## Step 9. Try and error

### Note
copy file to ssh
scp -i "key.pem" FILE_YOU_WANT_TO_BRING SSH_IP:/home/ubuntu/FILE_PATH_YOU_WANT_TO_PUT
