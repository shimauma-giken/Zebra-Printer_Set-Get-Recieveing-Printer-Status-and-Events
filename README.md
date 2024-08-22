# Zebra-Printer_Set-Get-Recieveing Printer Status and Events

# Link-OS プリンタ ステータスとイベント情報を取得する方法（まとめサイト）


![bg](https://www.zebra.com/content/dam/zebra_dam/global/zcom-web-production/web-production-photography/web002/print-dna-photography-website-video-preview-16x9.jpg.imgo.jpg)

</br>
2024年初旬以降、プリンタのステータス取得方法に関する質問が増えている。背景として下記状況があるためと思われる。

- 印刷処理をオートメーション化
- 集中管理によるプリンタ付帯作業の負担軽減
- トラブル発生時の早期解決
- AIによるプリンタ制御

</br>
</br>
</br>

# ステータス・イベント情報の取得方法

Link-OS プリンタのステータスを取得する方法は下記の通り。
ユーザ用途に応じて最適な方法を取得できるように、様々な方式が用意されている。

1. 通知型
   1. ZPL:^SX
   2. SGD:alerts
   3. SNMP Trap
2. クエリ型コマンド
   1. ZPL: ~HS
   2. SGD: device.host_status
   3. SDK: Class PrinterStatus
   4. SDK: Class SGD + "~HS"
   5. SDK: Class SnmpPrinter
   6. SGD: rfid.log.entries
   7. ZPL: ^HV
3. SNMP型
   1. SNMP Query
   2. SNMP Trap
4. ログ型　
   1. Syslog
5. カスタム型
   1. ZBI
6. 統合環境
   1. PPME
   2. ZebraNetBridge
   3. VIQF

</br>
</br>

### I/F 対応早見表

</br>

|                          | USB | Serial | Parallel | E-mail | TCP | UDP | SNMP | Syslog | MQTT | Bluetooth | SDK |
| ------------------------ | :-: | :----: | :------: | :----: | :-: | :-: | :--: | :----: | :--: | :-------: | :-: |
| ZPL:^SX                  |    |   x   |    x    |   x   |  x  |  x  |  x  |        |      |          |  x  |
| SGD:alerts               |  x  |   x   |    x    |   x   |  x  |  x  |  x  |        |  x  |     x     |  x  |
| SNMP Trap                |    |        |          |        |    |    |  x  |        |      |          |    |
| ZPL:~HS                  |  x  |   x   |    x    |        |  x  |    |      |        |      |     x     |  x  |
| SGD:device.host_status   |  x  |   x   |    x    |        |  x  |    |      |        |      |     x     |  x  |
| SDK: Class PrinterStatus |  x  |   x   |    x    |        |  x  |    |      |        |      |     x     |  x  |
| SDK: Class SGD +"~HS"    |  x  |   x   |    x    |        |  x  |    |      |        |      |     x     |  x  |
| SDK: Class SnmpPrinter   |    |        |          |        |    |    |  x  |        |      |          |  x  |
| SGD: rfid.log.entries    |  x  |   x   |    x    |        |  x  |    |      |        |      |     x     |  x  |
| ZPL: ^HV                 |  x  |   x   |    x    |        |  x  |    |      |        |      |     x     |  x  |
| SNMP Query               |    |        |          |        |    |    |  x  |        |      |          |    |
| SNMP Trap                |    |        |          |        |    |    |  x  |        |      |          |    |
| Syslog                   |    |        |          |        |    |    |      |   x   |      |          |    |
| ZBI                      |  x  |   x   |    x    |        |  x  |  x  |      |        |      |     x     |    |
| PPME                     |    |        |          |        |  x  |    |      |        |      |          |    |

---

</br>
</br>
</br>

### 1. 通知型

<img width="500" src="https://images.unsplash.com/photo-1563986768609-322da13575f3?q=80&w=1740&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D">

イベントが発生時に指定したホストへイベント情報を通知する。

用途：

- 多数の機器を集中管理、制御
- 遠隔地の機器をリモート管理、制御
- アプリや組み込みシステムへのアラート通知

</br>
</br>

#### ■ ZPL： ^SX

設定することにより、任意のインターフェイスへイベント通知が可能。</br>
https://github.com/shimauma-giken/Zebra-Printer_Link-OS_Add-Alerts-by--SX-command

#### ■ SGD: alerts.add

^SXの上位互換版。^SXより汎用性が高い。</br>
https://github.com/shimauma-giken/Zebra-Printer_Link-OS_Add-Alerts-by-SGD-Alerts-command

</br>
</br>

## 2. クエリ型コマンド

<img width="500" src="https://images.unsplash.com/photo-1576669801775-ff43c5ab079d?q=80&w=1470&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D">

</br>
</br>

#### ■ ZPL:~HS </br> ■ SGD:device.host_status

プリンタのステータス情報を取得するにあたって、代表的なコマンドが2つある。プリンタの死活監視やオーソドックスなステータス情報収集に用いられることが多い。</br>
https://github.com/shimauma-giken/Zebra-Printer_Link-OS_Get-Host-Status-by-Command

</br>
</br>

#### ■ SDK: Class PrinterStatus</br>■ SDK: Class SGD + "~HS"</br>■ SDK: Class SnmpPrinter

ホストのアプリケーションからステータスを取得する代表的な例を紹介する。
各取得方法に長短があるので、見極めた上で運用に適したものを利用すること。</br>
https://github.com/shimauma-giken/Zebra-Printer_Link-OS_Get-Host-Status-from-Programs

[参考: Zebra プリンタからRFID履歴/プリンタステータスを取得するコード例 C#編](https://github.com/shimauma-giken/Zebra-Printer_C-Sharp_Get-Printer-Status_RFID-Logs)

</br>
</br>

#### ■ SGD: rfid.log.entries</br>■ ZPL:^HV

RFエンコーダの実行結果を取得可能。</br>
https://github.com/shimauma-giken/Zebra-Printer_Recieve-Encode-Results-from-RFID-Printers

[Support Community:Enable and retrieve RFID data log using Zebra Setup Utilities](https://supportcommunity.zebra.com/s/article/000026343)

</br>
</br>

## 3. SNMP型

<img width="500" src="https://user0514.cdnw.net/shared/img/thumb/2unific240629.JPG">

SNMPとはネットワーク上の機器を監視・管理するために開発されたプロトコル。
機器の遠隔マネジメントや多数機器の集中管理の目的で利用されることが多い。
ポーリング方式とトラップ方式が存在する。</br>
https://github.com/shimauma-giken/Zebra-Printer_Link-OS_Get-Host-Status-by-SNMP

</br>
</br>

## 4. ログ型

<img width="500" src="https://user0514.cdnw.net/shared/img/thumb/aig-ai230817HK006-xl_TP_V.jpg">

syslogとは「System Logging Protocol」を略した言葉で、システムログなどをsyslogサーバーと呼ばれる特定のサーバーに送信するために使用されるプロトコルのこと。機器の集中管理や遠隔監視で利用されることが多い。

Link-OSプリンタでもシステム・イベントログをプリンタ内に保存、もしくは、Syslogサーバに通知可能。多拠点・多数のネットワークプリンタの監視・問題発生時の早期解決に役立つ。耐障害性が向上するため、ゼブラ・テクノロジーズは設定を推奨している。
</br>
https://github.com/shimauma-giken/Zebra-Printer_LInk-OS_Syslog-Setup

</br>
</br>

## 5. カスタム型

<img width="500" src="https://media.istockphoto.com/id/1288427343/ja/%E3%82%B9%E3%83%88%E3%83%83%E3%82%AF%E3%83%95%E3%82%A9%E3%83%88/%E3%83%A9%E3%83%99%E3%83%AB%E3%83%97%E3%83%AA%E3%83%B3%E3%82%BF.jpg?s=612x612&w=0&k=20&c=A5FC476OF4jUbcWuXjwnqghHM-BtYPWx1r-epierfjc=">

#### ■ 1. ZBI

ZBIはBasic型のプリンタ開発言語。ZPLやプリンタ機能をだけでは対応できない機能をプリンタに実装することができる。
Link-OS プリンタでは下記のようなイベントが特定のI/Fから通知可能。

- 本体ボタンの押下*
- 各種エラー
- カスタムエラー・メッセージ

https://github.com/shimauma-giken/Zebra-Printer_Link-OS_Send-Event-Notice-to-Host-by-ZBI
</br>
</br>

## 6. 専用オプション型

<img width="500" src="https://hyprotech.com.pl/wp-content/uploads/2019/05/r110xi4-checking-label.jpg">

#### ■ Applicator Port 型

一部のテーブルトップはオートメーションデバイスとして動作できるようにアプリケータI/F オプションをご用意している。このようなオートメーション機器と連動ができるように特定のプリンタ機種にてアプリケータI/Fが実装可能となっている。プリンタのステータスをダイレクトに通知するためにレスポンス性が良い。このI/Fは自動処理システムなど、プリンタに対してシビアな制御が必要な運用環境下で用いられることが多い。  </br>
https://github.com/shimauma-giken/Zebra-Printer-Link-OS_Understanding-Logic-of-Applicator-Port


</br>
</br>

## まとめ

<img width="500" src="https://www.zebra.com/content/dam/zebra_dam/global/zcom-web-production/web-production-photography/hero-photos/photography-web-cat-hero-printers-16x9-3600.jpg.imgo.jpg">

外部運用環境と効率的に連携することを主目的として開発されたLink-OS型プリンタは様々なI/Fや連携方式を持っています。それぞれが長短を持っているので、運用環境に合わせて適切なメソッドを選択いただけますと幸いです。
