# docker-redmine


## イメージを指定してコンテナを作成して起動
~~~
~$ docker run -d --name docker-redmine -p3000:3000 redmine
1fe26b3929040d92ed347786f0d179641290a00ee8d52aad2a5200fdc9ba7b10
~~~
docker-redmineがcontainer名になる
次回起動からは
~~~
~$ docker start docker-redmine
~~~

## 初期admin パスワードの変更
- localhost:3000にブラウザでアクセス
- user admin / password adminでログインすると初期パスワードの変更を求められる
- 管理 - デフォルト設定をロードしておくとトラッカー等の初期値が設定される(一から新規に作るなら不要だけどちょっと面倒)

## 起動中のdocker環境へログイン

~~~
~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
1fe26b392904        redmine             "/docker-entrypoint.…"   7 minutes ago       Up 7 minutes        0.0.0.0:3000->3000/tcp   docker-redmine
~$ docker exec -it docker-redmine /bin/bash
root@1fe26b392904:/usr/src/redmine# 
~~~

## docker環境内にvimを追加(やらないほうがいいか? docker cpして編集するほうがよさそう)
- vim入っていないので入れる
~~~
root@1fe26b392904:/usr/src/redmine# apt-get update
root@1fe26b392904:/usr/src/redmine# apt-get install vim
~~~

## mail設定

hostマシン側でconfiguration.ymlを作る
~~~
~$ cat configuration.yml 
default:
  email_delivery:
      delivery_method: :smtp
      smtp_settings:
        enable_starttls_auto: true
        address: "xxx.sakura.ne.jp"
        port: 587
        authentication: :plain
        domain: 'xxx.sakura.ne.jp'
        user_name: 'xxx@sakura.ne.jp'
        password: 'xxx'
~~~

configuration.ymlをdockerにcopy
~~~
~$ docker cp configuration.yml docker-redmine3:/usr/src/redmine/config/configuration.yml
~~~

docker再起動
~~~
~$ docker restart docker-redmine 
~~~

起動確認
~~~
~$ docker ps
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                    NAMES
e812cd9b4d8d        redmine             "/docker-entrypoint.…"   31 seconds ago      Up 4 seconds        0.0.0.0:3000->3000/tcp   docker-redmine
~~~

起動失敗してたらlogを確認
~~~
~$ docker logs docker-redmine
~~~

~~失敗したcontainerを簡単に修正する方法がわからないのでcontaier消して一からやり直し~~
起動していないcontainerにもdocker cpは可能なので再度copyする


## container削除
~~~~
~$ docker container rm docker-redmine
~~~~











