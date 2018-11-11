# Zabbix で Nginx + PHP を監視するサンプル

# 準備

- [Docker のインストール](http://docs.docker.jp/engine/installation/)

# 使い方

## コンテナ作成

初回はコンテナを作成するためにビルドします。

```
$ docker-compose build
```

## 起動

```
$ docker-compose up -d
```
`http://localhost:8080` で、Zabbix の Web ページにアクセスできます。  

## 終了

```
$ docker-compose stop
```

## up した時にうまく立ち上がらない場合

agent のコンテナに入って、Nginx と FPM を再起動してみてください。

```
$ docker exec -it monitor_php_with_zabbix_zabbix_agent_1 /bin/sh
$ rc-service nginx restart
$ rc-service /etc/init.d/php-fpm7 restart
```

## 設定値を書き変えたい

Nginx や PHP の設定ファイルはビルド時にしか配置していないので、適宜書き変えてください。

```
$ docker exec -it monitor_php_with_zabbix_zabbix_agent_1 /bin/sh
# vim /etc/php7/php-fpm.conf
# rc-service /etc/init.d/php-fpm7 restart
```

# 参考資料

- [Zabbix公式コンテナとdocker-compose使って検証環境を簡単に作成削除](https://yomon.hatenablog.com/entry/2018/03/29/Zabbix%E5%85%AC%E5%BC%8F%E3%82%B3%E3%83%B3%E3%83%86%E3%83%8A%E3%81%A8docker-compose%E4%BD%BF%E3%81%A3%E3%81%A6%E6%A4%9C%E8%A8%BC%E7%92%B0%E5%A2%83%E3%82%92%E7%B0%A1%E5%8D%98%E3%81%AB%E4%BD%9C%E6%88%90)
- [https://github.com/tanrakukairo/zabbix_template_php-fpm](https://github.com/tanrakukairo/zabbix_template_php-fpm)

# さいごに

質問、不具合等があれば、イシュー、プルリクエスト、もしくは [Twitter](https://twitter.com/naoki85_201612) からお願いします。