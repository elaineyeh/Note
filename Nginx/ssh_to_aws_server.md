# ssh to AWS server
專案 a, b 用同一個 docker-compose-template.yml，當我們要新增專案時只要將 docker-compose-template.yml 複製到該專案即可
專案 c 因為要 掛上 a, b 的 volumes 及對外連接，所以他自己一個 docker-compose.yml
## Step 1. Open aws EC2
開啟一個伺服器，並下載 .pem 檔

## Step 2. Write docker-compose-template.yml
```
version: '3'
services:
  prj_a:
    image: nginx
    volumes:
      - ${PWD}:/usr/share/nginx/html
```
`${PWD}` 使用環境變數自動帶入

## Step 3. Write circleci yml
```
|__nginx
     |__a
     |  |__.circleci
     |          |__config.yml
     |  |__index.html
     |  |__docker-compose-a.yml
     |
     |__b
     |  |__index.html
     |  |__docker-compose-b.yml
     |
     |__middle
        |__docker-compose.yml
```
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
      - run: echo $CIRCLE_PROJECT_REPONAME >> sshenv # set the project name to the ssh environment variable
      - run: scp -o StrictHostKeyChecking=no sshenv $USER@$HOST:~/.ssh/environment # bring the ssh environment variable and set the continue connect is yes
      # - run: envsubst '${CIRCLE_PROJECT_REPONAME}' < deploy.sh > real_deploy.sh # copy the deploy.sh to real_deploy.sh and bring the environment variable
      - run: ssh -o StrictHostKeyChecking=no $USER@$HOST < deploy.sh # ssh to AWS server

```

## Step 4. Write deploy.sh
deploy.sh 執行 git pull, docker compose up 等動作，讓網頁跑起來
### 先順過流程再寫才不用一直改來改去
### 因為我們檔案是下 git pull ，所以第一次要手動連線到伺服器 git clone
```
echo $CIRCLE_PROJECT_REPONAME
cd $CIRCLE_PROJECT_REPONAME
pwd
git pull
mkdir -p ../docker-compose

envsubst < docker-compose-template.yml > docker-compose-$CIRCLE_PROJECT_REPONAME.yml
cp docker-compose-$CIRCLE_PROJECT_REPONAME.yml ../docker-compose

cd ../docker-compose
pwd
FILENAME=`ls ./*.yml` # list all .yml file in the folder
FILE="docker-compose"
for EACHFILE in $FILENAME
do
   FILE+=" -f $EACHFILE" # put the -f docker-compose.yml in to string
done
FILE+=" up -d"
echo $FILE
echo $FILE > run.sh # put it into run.sh
sh run.sh
```
```
run.sh 要多下一行執行而前面 deploy.sh 不用是因為 ssh 本身就接受指令，只是我們把要執行的指令放到黨案裡
ssh 就是遠端的 sh
ssh=secure shell
```

## Step 5. git init
## Step 6. Circleci follow project
## Step 7. Set the environment variable
為了跑 config.yml 可時候可以代數環境變數
## Step 8. Set the Github ssh key
#### 設定 Github ssh key 讓 circle 可以不用輸入密碼就將專案拉下來
#### 是將公鑰上傳
[Reference](https://ithelp.ithome.com.tw/articles/10205988)
## Step 9. Try and error
