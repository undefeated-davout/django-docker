# 使い方
## Dockerコマンド
### 新規にプロジェクト[mysite]を作る場合
#### 以下、django-dockerディレクトリ直下にてコマンド実行

```$xslt
# Ansibleをwebディレクトリ直下へcloneしてくる
$ git clone https://github.com/teruzoufox/ansible-django.git web/ansible-django
$ docker-compose build
$ docker-compose run web django-admin.py startproject mysite .
```

#### src/mysite/mysite/setting.pyを編集

```$xslt
# 下記内容を追加（[import os]の下等適当に）
import pymysql
pymysql.install_as_MySQLdb()

# DATABASESの内容を下記の通り修正
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'mysite',
        'USER': 'mysiteuser',
        'PASSWORD': 'mysitepass',
        'HOST': 'db',
        'PORT': '3306',
        'OPTIONS': {
            'charset': 'utf8mb4',
        },
    }
}
```

#### 下記コマンドでマイグレーション

```$xslt
$ docker-compose run web ./manage.py makemigrations
$ docker-compose run web ./manage.py migrate
```

#### 管理者設定

```$xslt
$ docker-compose run web ./manage.py createsuperuser
```

## 実行コマンド

```$xslt
$ docker-compose up -d
```

## ブラウザで確認するには
ブラウザで下記アドレスに接続する

- http://localhost:8000

## コンテナ内にログインするとき

```$xslt
$ docker exec -it mysite.web /bin/bash
```

## 停止コマンド

```$xslt
$ docker-compose down
```

## MySQLへ接続するには
下記コマンドで接続

```$xslt
$ mysql -uroot -p -h0.0.0.0
```

もしくは下記接続情報でWorkbench等で接続
- ホスト：0.0.0.0
- ポート：3306
- ユーザ：root
- パスワード：mysitepass

