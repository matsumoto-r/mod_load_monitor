# mod_load_monitor

# How to use
こんな感じで設定すると、LoadMonitorOverの場合は、リクエスト処理時に指定したロードアベレージの値20を超えていると、指定したコマンドが実行されます。また、LoadMonitorUnderの場合は、指定したロードアベレージ5より低い場合に、指定したコマンドが実行されます。ちなみに、.htaccessにも記述することができます。

```
LoadModule load_monitor_module modules/mod_load_monitor.so
LoadMonitorOver 20.0 "echo '20 yori takai' >> /tmp/over"
LoadMonitorUnder 5.0 "echo '5 yori hikui' >> /tmp/under"
```

こんな感じで設定すると使えます。一応簡単なログ出力もしておきました。

```
[Thu Jul 26 19:07:54 2012] [debug] mod_load_monitor DEBUG load_monitor_access_checker: cmd: echo '20 yori takai' >> /tmp/over code: 0 (current load: 34.160000 over threshold load: 20.000000)
```

Apache上でやってもあまり意味がないかもしれませんが、細かく設定することでApache自体が自分自身で能動的な動きをするような設定ができるかもしれませんね。その他、Apache自身が何か新しい監視システムになって、監視したりしだすと面白いかもしれません。

環境変数で取得できるようにしました。現在使える環境変数は以下の通りです。

```
環境変数            値
LOAD_MON_HOSTNAME   サーバのホスト名
LOAD_MON_FILENAME   リクエストファイル
LOAD_MON_URI        リクエストURL
LOAD_MON_METHOD     メソッド（GETとかPUTとか）
LOAD_MON_CLIENT_IP  クライアントのIP
LOAD_MON_SERVER_IP  サーバのIP
```

他にも取得できた方が良いような値があれば、言っていただくもしくはpull requestしていただければ、随時追加しようと思います。例えば以下のように使うことができます。

```
LoadMonitorUnder 1.00 "echo $LOAD_MON_HOSTNAME $LOAD_MON_FILENAME >> /tmp/under"
```

じょじょに機能が増えてきてしまいました。悪い癖です。
