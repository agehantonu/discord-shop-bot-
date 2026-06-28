
<p align="center">
  <img src="https://github.com/user-attachments/assets/0dc3517b-9f92-44ff-a12d-6f6a18ccbecb" alt="loji-the-fire-rises">
</p>



<h1 align="center">多機能自販機 機能集</h1>

<p align="center">
  <b>権限制御</b>: すべての管理コマンドは<b>サーバー管理者権限</b>が必要です。<br>
  一般ユーザーはパネルのボタン操作のみ利用可能です。
</p>

\---

## 💰 PayPay 連携

|コマンド|説明|パラメータ|
|-|-|-|
|`/paypay login`|PayPayアカウントを登録します|`phone`: 電話番号, `password`: パスワード|

**フロー**:

1. 電話番号・パスワード入力 → OTP（SMS認証コード）送信
2. 4桁のOTPコード入力 → アカウント登録完了

\---

## 📈 MEXC 連携

|コマンド|説明|パラメータ|
|-|-|-|
|`/mexc login`|MEXCアカウントにログインします|`api\_key`: アクセスキー, `secret`: シークレットキー|

**フロー**:

1. MEXCのAPIキー・シークレットキー入力
2. 認証テスト → 残高取得確認 → 登録完了

\---

## 💎 残高確認

|コマンド|説明|
|-|-|
|`/balance`|MEXCのLTC残高をJPY換算で表示します|

**表示内容**:

* LTC保有量
* LTC/JPY換算額（MEXCのレート準拠）

\---

## 🔄 自動交換パネル

|コマンド|説明|
|-|-|
|`/panel`|PayPay→LTCの自動交換パネルを設置します|

### パネル内ボタン（全ユーザー利用可能）

|ボタン|説明|
|-|-|
|**Exchange**|PayPayリンクとLTCアドレスを入力して自動交換|
|**Check Status**|取引IDを入力して送金状況を確認|
|**Balance**|現在のLTC残高をJPY換算で表示|

### 交換フロー

```
\[初回利用者]
  ↓ Exchangeボタン
  ↓ ランダム金額（1〜100円）のPayPayリンク送信 → 本人確認
  ↓ 以降は通常フロー

\[通常フロー]
  ↓ PayPayリンク送信（最低金額以上）
  ↓ 自動受取 → MEXCからLTC自動出金
  ↓ DMにて完了通知 + 取引ID
```

### レート設定（`/settings`で設定）

|設定項目|説明|
|-|-|
|最低交換金額|交換を受け付ける最低金額（JPY）|
|PayPay Moneyレート|実際残高に対する交換率（%）|
|PayPay Money Lightレート|ライト残高に対する交換率（%）|

\---

## 🛒 自動販売機（Vending Machine）

|コマンド|説明|パラメータ|
|-|-|-|
|`/vm\_create`|自動販売機を作成します|`name`: 自動販売機の名前|
|`/vm\_set\_log`|公開売上ログチャンネルを設定します|`vending\_machine\_id`, `channel`: ログチャンネル|
|`/vm\_set\_private\_log`|非公開売上ログチャンネルを設定します|`vending\_machine\_id`, `channel`: ログチャンネル|
|`/vm\_add\_product`|自動販売機に新しい商品を追加します|`vending\_machine\_id`, `name`, `price`, `description`（任意）, `emoji`（任意）|
|`/vm\_add\_stock`|商品に在庫を追加します|`vending\_machine\_id`, `stock\_type`: 有限/無限, `stock\_file`: .txtファイル（有限時）|
|`/vm\_setup`|自動販売機パネルを設置します（複数商品対応）|`vending\_machine\_id`, `title`, `color`, `image\_url`|
|`/vm\_withdraw\_stock`|商品の在庫を取り出します|`vending\_machine\_id`, `quantity`|
|`/vm\_check\_stock`|商品の在庫内容を確認します|`vending\_machine\_id`|
|`/vm\_delete\_product`|自動販売機から商品を削除します|`vending\_machine\_id`|
|`/vm\_edit\_product`|商品情報を編集します|`vending\_machine\_id`|
|`/vm\_delete`|自動販売機を削除します|`vending\_machine\_id`|
|`/vm\_update`|自動販売機パネルを更新します|`vending\_machine\_id`, `message\_link`, `panel\_title`, `panel\_description`, `panel\_image`|

### 購入処理

```
\[パネル表示]
  ↓ 商品選択（ドロップダウン）
  ↓ 数量・クーポンコード入力
  ↓ 購入確認
  ↓ PayPayリンク送信（支払い）
  ↓ 自動受取確認 → 在庫から商品をDM送信
  ↓ ログチャンネルに売上記録
```

### 在庫

|タイプ|説明|
|-|-|
|**有限**|.txtファイルの1行1商品。購入で自動減少|
|**無限**|固定テキストを無限に販売可能|

\---

## チケット

|コマンド|説明|パラメータ|
|-|-|-|
|`/ticket\_panel`|チケット作成パネルを設置します|`panel\_title`（任意）, `panel\_description`（任意）|

### チケット処理

```
\[パネル表示]
  ↓ 「チケット作成」ボタン押下
  ↓ 専用テキストチャンネル作成（閲覧権限: 作成者のみ）
  ↓ サポート対応
  ↓ 「チケットを閉じる」→ 確認 → チャンネル削除
```

\---

## 設定コマンド

|コマンド|説明|パラメータ|
|-|-|-|
|`/settings`|交換設定を構成します|モーダル入力: 最低金額, Moneyレート%, Money Lightレート%|
|`/allowed\_users add`|許可ユーザーを追加します|`user`: 対象ユーザー|
|`/allowed\_users remove`|許可ユーザーを削除します|`user`: 対象ユーザー|
|`/log\_channel public`|公開ログチャンネルを設定します|`channel`: 対象チャンネル|
|`/log\_channel private`|非公開ログチャンネルを設定します|`channel`: 対象チャンネル|

> \*\*注意\*\*: `allowed\_users`は旧仕様の互換機能です。現在は\*\*管理者権限\*\*があれば誰でも実行可能です。

\---

## 🤖 自動処理（裏側機能）

|機能|説明|
|-|-|
|**PayPay自動受取**|送られてきたPayPayリンクを自動で受け取り|
|**MEXC自動出金**|LTCを指定アドレスに自動送金|
|**レート自動計算**|JPY/USDT, LTC/USDTの相場をリアルタイム取得|
|**アカウント紐付け**|DiscordIDとPayPay externalIdを紐付けて不正防止|
|**初回認証**|ランダム金額のPayPayリンクで本人確認|
|**在庫自動管理**|テキストファイルベースの在庫自動減算|
|**ログ出力**|公開ログ・非公開ログ・DMへの自動通知|

\---

## 🔐 権限まとめ

|権限|実行可能なコマンド|
|-|-|
|**サーバー管理者**|すべてのスラッシュコマンド|
|**一般ユーザー**|パネルボタン操作（Exchange, Check Status, Balance, 商品購入, チケット作成）|



<h2>招待リンク</h2>

https://discord.com/oauth2/authorize?client\_id=1516814724811980820\&permissions=8\&integration\_type=0\&scope=bot 

<h2>サポートサーバー</h2>

https://discord.gg/ghgpx4EQFC

<h2>製作者のdiscordアカウント</h2>

https://discord.com/users/1512652537407340674

