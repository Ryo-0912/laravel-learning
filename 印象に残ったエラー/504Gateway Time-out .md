Postmanで(ユーザAの情報で)APIを実行したところ、504 Gateway Timeoutのエラーが表示。

504 Gateway Timeoutということは、NginxがPHP-FPMからの応答を待っていたが、応答がなく時間オーバということで発生するもの。

このエラーを解決するにあたり、Nginxのaccess.log、error.logの中身を調査。

access.log

```
-(37.35.39.10) - - [17/Apr/2024:12:31:05 +0900] "GET /api/v1/matching-list HTTP/1.1" 504 160 "-" "PostmanRuntime/7.37.3"
```

error.log

```
2024/04/17 12:15:57 [error] 81817#81817: *7 upstream timed out (110: Connection timed out) while reading response header from upstream, client: 37.35.39.10, server: xxx.xxxxx.jp, request: "GET /api/v1/matching-list HTTP/1.1", upstream: "fastcgi://unix:/var/run/php/php8.2-fpm.sock", host: "xxx.xxxxx.jp"
```

php8.2-fpm.logを確認。

```
[14-Apr-2024 00:00:03] NOTICE: error log file re-opened
[16-Apr-2024 16:30:34] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[16-Apr-2024 18:45:06] NOTICE: Terminating ...
[16-Apr-2024 18:45:06] NOTICE: exiting, bye-bye!
[16-Apr-2024 18:45:06] NOTICE: fpm is running, pid 394360
[16-Apr-2024 18:45:06] NOTICE: ready to handle connections
[16-Apr-2024 18:45:06] NOTICE: systemd monitor interval set to 10000ms
[16-Apr-2024 19:01:00] NOTICE: fpm is running, pid 551
[16-Apr-2024 19:01:00] NOTICE: ready to handle connections
[16-Apr-2024 19:01:00] NOTICE: systemd monitor interval set to 10000ms
[17-Apr-2024 11:36:25] WARNING: [pool www] server reached pm.max_children setting (5), consider raising it
[17-Apr-2024 12:56:39] NOTICE: Terminating ...
[17-Apr-2024 12:56:39] NOTICE: exiting, bye-bye!
[17-Apr-2024 12:56:40] NOTICE: fpm is running, pid 87106
[17-Apr-2024 12:56:40] NOTICE: ready to handle connections
[17-Apr-2024 12:56:40] NOTICE: systemd monitor interval set to 10000ms
[17-Apr-2024 12:59:11] NOTICE: Terminating ...
[17-Apr-2024 12:59:11] NOTICE: exiting, bye-bye!
[17-Apr-2024 12:59:11] NOTICE: fpm is running, pid 87142
[17-Apr-2024 12:59:11] NOTICE: ready to handle connections
[17-Apr-2024 12:59:11] NOTICE: systemd monitor interval set to 10000ms
[17-Apr-2024 13:00:45] NOTICE: Terminating ...
[17-Apr-2024 13:00:45] NOTICE: exiting, bye-bye!
[17-Apr-2024 13:00:45] NOTICE: fpm is running, pid 87176
[17-Apr-2024 13:00:45] NOTICE: ready to handle connections
[17-Apr-2024 13:00:45] NOTICE: systemd monitor interval set to 10000ms
[17-Apr-2024 13:18:33] NOTICE: Terminating ...
[17-Apr-2024 13:18:33] NOTICE: exiting, bye-bye!
[17-Apr-2024 13:18:33] NOTICE: fpm is running, pid 88409
[17-Apr-2024 13:18:33] NOTICE: ready to handle connections
[17-Apr-2024 13:18:33] NOTICE: systemd monitor interval set to 10000ms
[17-Apr-2024 13:34:19] NOTICE: fpm is running, pid 553
[17-Apr-2024 13:34:19] NOTICE: ready to handle connections
[17-Apr-2024 13:34:19] NOTICE: systemd monitor interval set to 10000ms
[17-Apr-2024 13:35:24] NOTICE: Terminating ...
[17-Apr-2024 13:35:24] NOTICE: exiting, bye-bye!
[17-Apr-2024 13:35:46] NOTICE: fpm is running, pid 551
[17-Apr-2024 13:35:46] NOTICE: ready to handle connections
[17-Apr-2024 13:35:46] NOTICE: systemd monitor interval set to 10000ms
[17-Apr-2024 15:33:11] NOTICE: Terminating ...
[17-Apr-2024 15:33:11] NOTICE: exiting, bye-bye!
[17-Apr-2024 15:33:12] NOTICE: fpm is running, pid 9191
[17-Apr-2024 15:33:12] NOTICE: ready to handle connections
[17-Apr-2024 15:33:12] NOTICE: systemd monitor interval set to 10000ms
[17-Apr-2024 15:35:32] NOTICE: Terminating ...
[17-Apr-2024 15:35:32] NOTICE: exiting, bye-bye!
[17-Apr-2024 15:36:42] NOTICE: fpm is running, pid 551
[17-Apr-2024 15:36:43] NOTICE: ready to handle connections
[17-Apr-2024 15:36:43] NOTICE: systemd monitor interval set to 10000ms
[17-Apr-2024 15:50:05] NOTICE: Terminating ...
[17-Apr-2024 15:50:05] NOTICE: exiting, bye-bye!
[17-Apr-2024 15:50:05] NOTICE: fpm is running, pid 3050
[17-Apr-2024 15:50:05] NOTICE: ready to handle connections
[17-Apr-2024 15:50:05] NOTICE: systemd monitor interval set to 10000ms
```

```
server reached pm.max_children setting (5), consider raising it
```

このエラーが目に止まったので、PHP-FPMの設定ファイル(www.conf)を編集。


API実行するも、504エラーは解決せず。

ディスクの空き容量を見てみたが、問題はなさそう。。

```
/var/log$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        20G  9.7G  9.6G  51% /
devtmpfs        2.0G     0  2.0G   0% /dev
tmpfs           2.0G     0  2.0G   0% /dev/shm
tmpfs           391M 1016K  390M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           2.0G     0  2.0G   0% /sys/fs/cgroup
/dev/loop0       46M   46M     0 100% /snap/certbot/3643
/dev/loop1       46M   46M     0 100% /snap/certbot/3700
/dev/loop2       56M   56M     0 100% /snap/core18/2796
/dev/loop3       56M   56M     0 100% /snap/core18/2812
/dev/loop4       64M   64M     0 100% /snap/core20/2182
/dev/loop5       64M   64M     0 100% /snap/core20/2264
/dev/loop6      355M  355M     0 100% /snap/google-cloud-cli/227
/dev/loop7      351M  351M     0 100% /snap/google-cloud-cli/229
/dev/loop8       92M   92M     0 100% /snap/lxd/24061
/dev/loop10      39M   39M     0 100% /snap/snapd/21465
/dev/loop9       40M   40M     0 100% /snap/snapd/21184
/dev/sda15      105M  6.1M   99M   6% /boot/efi
tmpfs           391M     0  391M   0% /run/user/1006
```

504エラーが出ていたが、laravelのログ出力はされていなかった。ただ、APIを実行するにあたって、ユーザAでは504エラーが出たが、ユーザBでは正常に動いていた。
また、ローカル環境でも、同じAPIをユーザC,Dで行ったが、こちらは共に正常であった。
ただ、ユーザCとDで実行結果が出力されるまでに時間差は明らかにあった。

ここで、ユーザAのjwtに問題があるのかと思い、その他のAPIをユーザAで実施したが、正常に実施できた。

そこで、ローカルで実施したAPIでユーザによって、結果出力までに時差があったことから、laravelのログに出力はされていなかったが、該当APIのコードを見直してみることにした。
そこでは、長いEloquentがあり、NOT EXSISTを使っていた。
そこで、LEFT JOINでコードを書き直して、再度Postmanを実行すると、明らかに処理速度が速くなった。
このコードを試しに、STG環境でも記述して、Postmanを実行。すると、ユーザAでも504エラーを返すことなく、正常に機能した。

追記
ログの設定によるが、タイムアウトや504エラーが発生する場合、ログに反映されないこともあるらしい。
したがって、laravelのログに出力がないからといって、laravel側でのエラーではないと考えるのは良くない。


