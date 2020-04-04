# 本レポジトリについて
本レポジトリは Docker コンテナ上で mailhog を起動する手順等について、自分用に整理したものです。
あくまで自己学習用のため、参考程度にしてください。

# 起動方法
```
docker-compose up
```

# 設定ファイル
`.env` に設定を記述する
本レポジトリでは HTTP サーバーおよび SMTP サーバのポートを以下のように指定している

HTTP Port: 50025
SMTP Port: 60025

また、Web URL は `http://[IPアドレス or ドメイン名]/mailhog` となるように指定している。

# Basic 認証について
`auth_file` で指定している。
bcrypt で暗号化した文字列を以下の形式で指定している。
```
[ユーザ名]:[bcryptで暗号化したパスワード]
```

bcrypt で暗号化したパスワードは docker コンテナ起動後、以下コマンドを実行することで得られる。
```
docker exec mailhog /usr/local/bin/MailHog bcrypt [平文のパスワード]
```

詳細については https://github.com/mailhog/MailHog/blob/master/docs/Auth.md を確認すること。

# sendmail コマンドによるメール送信例
以下のコマンドでメール送信をテストすることが可能
```
$ {
	echo "From: sender@example.com"
	echo "To: reciever@mailhog.example.com"
	echo "Subject: Test mail"
	echo
	echo "Hello world!"
} |sendmail -i -t -f sender@example.com reciever@maihog.example.com -S 0.0.0.0:60025
```
