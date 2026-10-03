# プライバシーポリシー

iOS アプリ「my鉄道マップ」（以下「本アプリ」）のプライバシーポリシーです。

制定日: 2026年7月2日  
改定日: 2026年10月3日（英語版を追加・送ったリンクを iCloud から探す機能について追記）  
English version below.

## 開発者による情報収集

開発者は、ユーザーの個人情報を収集しません。

- 本アプリにアカウント登録機能はありません
- 本アプリが開発者のサーバーへデータを送信することはありません
  （開発者はサーバーを運用していません）

## 位置情報

現在地を地図上に表示する目的でのみ、端末の位置情報を利用します。
位置情報の利用には iOS の許可が必要で、許可しなくても現在地表示以外の機能は
利用できます。位置情報はアプリ内での表示にのみ使用され、端末の外へ
送信・保存されることはありません。

## ユーザーが作成したデータ

本アプリで作成した自作の駅・路線・ダイヤ（電車ノード）等のデータは、
すべて端末内にのみ保存されます。CSV / JSON / 作品ファイルの書き出しによる
データの共有は、ユーザー自身の操作によってのみ行われます。

**例外は「作品のリンク共有」だけです。** ユーザーがこの機能を使ったときに限り、
選んだ作品が下記のとおりサーバーへアップロードされます。
それ以外の場面で、開発者がユーザーの作成データを取得することはありません。

## 作品のリンク共有（Apple iCloud / CloudKit）

バージョン 1.0.6 以降、作った作品（自作の駅・路線・ダイヤ・停車駅案内・路線図）を
リンクで他の人に渡す機能があります。**この機能を使ったときだけ**、次のことが起こります。

- 選んだ作品のデータが、Apple の iCloud（CloudKit）の**公開データベース**に
  1件アップロードされます。保存先は開発者が管理する領域で、開発者は内容を閲覧できます。
- アップロードには端末での iCloud サインインが必要です（Apple の仕組みによるものです）。
  Apple ID やメールアドレスが開発者や受け取った人に渡ることはありません。
  ただし Apple が発行する**匿名の識別子**（このアプリの中だけで有効なID）がデータに付くため、
  同じ人が作った作品どうしを結び付けることは技術的に可能です。
  この識別子から個人を特定することはできず、他のアプリと共通でもありません。
- リンクを受け取った人は、iCloud のサインインなしで内容を取得できます。
  **リンクを知っている人は誰でも取得できます。**
  公開しても差し支えのない内容だけを共有してください。
- アップロードしたデータは**作成から30日で無効**になり、リンクを開いても取得できなくなります。
  送った本人は、期限を**延ばした日から最長1年まで**延ばせます（アプリの「作品管理・共有」→「送ったリンク」→「期限を延ばす」）。
  期限が過ぎたものは延ばせません。
  期限切れのデータは、**送った本人が次にアプリを開いたときに置き場から削除**されます。
- 送った本人は、**期限を待たずにいつでも取り消せます**
  （アプリの「作品管理・共有」→「送ったリンク」）。
  取り消すと置き場から削除され、リンクを渡した相手も取得できなくなります。
  この控えは端末の中にのみ保存されます。アプリを入れ直したあとも、同じ Apple ID なら
  「送ったリンク」の「iCloud から自分が送ったリンクを探す」で探して取り消せます（1.0.8 以降）。
- 制作者名は、ユーザーが自分で入力したときだけ作品に含まれます。
  端末やアカウント由来の情報を自動で入れることはありません。
- **ファイルで送る機能（`.myrail` ファイル）はサーバーを経由しません。**
  リンクを使いたくない場合はこちらをお使いください。
- 作ったリンクを X（旧 Twitter）に投稿する入口があります。
  **X の投稿画面を開くだけ**で、本アプリが X に情報を送ることはありません。
  投稿するかどうか、何を書くかは利用者が決めます。
- 不適切な内容の作品を見つけた場合は、受け取り画面に表示される番号を添えて
  サポート窓口までご連絡ください。確認のうえ削除します。

## 広告（Google AdMob）

本アプリは Google AdMob による広告（バナー広告・動画広告）を
表示します。広告配信のため、Google が広告識別子（IDFA）や端末情報等を
収集する場合があります。

- 初回起動時等に、iOS の App Tracking Transparency（ATT）による
  トラッキング許可の確認を表示します。「許可しない」を選んでも
  本アプリは利用できます（パーソナライズされない広告が表示されます）
- Google による情報の取り扱いについては以下をご確認ください
  - [Google のプライバシーポリシー](https://policies.google.com/privacy)
  - [Google 広告のしくみ](https://policies.google.com/technologies/ads)
  - [AdMob のデータ収集について](https://support.google.com/admob/answer/6128543)

プレミアム（App Store 経由の買い切り課金）を購入すると広告は表示されず、
上記の広告関連のデータ収集も行われません。

## 課金情報

プレミアムの購入は App Store（Apple）を通じて処理されます。
開発者がクレジットカード情報等の決済情報を取得することはありません。

## 改定について

本ポリシーは、機能追加や法令の変更等に応じて改定することがあります。
重要な変更がある場合は、本ページ（GitHub リポジトリ）で告知します。
最新版は常に本ページに掲載されます。

## お問い合わせ

本ポリシーに関するお問い合わせは、本リポジトリの
[Issue](../../issues) からお願いします。

---

# Privacy Policy (English)

This is the privacy policy for the iOS app "MyRailwayMap" (my鉄道マップ, "the App"). If this English version and the Japanese version above differ, the Japanese version prevails.

Established: July 2, 2026  
Revised: October 3, 2026 (added this English version and finding sent links in iCloud)

## Information collected by the developer

The developer does not collect your personal information.

- The App has no account registration
- The App does not send data to the developer's servers
  (the developer does not operate any servers)

## Location

The App uses your device's location only to show your current location on the map.
Location requires iOS permission; if you don't allow it, everything except showing your
current location still works. Location is used only for display within the App and is
never sent or stored outside your device.

## Data you create

Your own stations, lines, timetables (train nodes) and other data you create in the App
are stored only on your device. Data is shared only when you choose to export CSV, JSON
or work files yourself.

**The only exception is sharing a work by link.** Only when you use this feature, the
works you choose are uploaded to a server as described below. Otherwise, the developer
never obtains data you create.

## Sharing works by link (Apple iCloud / CloudKit)

Since version 1.0.6, you can send works you have made (your own stations, lines,
timetables, stop charts and route maps) to others by link. **Only when you use this
feature**, the following happens.

- The data of the works you choose is uploaded as one record to the **public database**
  of Apple's iCloud (CloudKit). It is stored in an area managed by the developer, and the
  developer can view its contents.
- Uploading requires iCloud sign-in on your device (this is how Apple's system works).
  Your Apple ID and email address are never passed to the developer or to recipients.
  However, an **anonymous identifier** issued by Apple (valid only within this App) is
  attached to the data, so it is technically possible to link works made by the same
  person. This identifier cannot identify you and is not shared with other apps.
- People who receive the link can get the contents without signing in to iCloud.
  **Anyone who knows the link can get it.** Share only content that is fine to make public.
- Uploaded data **expires 30 days after it is created**, after which the link no longer
  works. The sender can extend it **up to one year from the day of extending**
  (in the App: "Manage & share works" → "Sent links" → "Extend expiry"). Expired links cannot be
  extended. Expired data is **deleted from storage the next time the sender opens the App**.
- The sender can **revoke a link at any time** before it expires
  ("Manage & share works" → "Sent links"). Revoking deletes it from storage, and the
  people you gave the link to can no longer get it. The record of sent links is kept only
  on your device. After reinstalling the App, you can find links you sent with the same
  Apple ID using "Find links I sent via iCloud" and revoke them (version 1.0.8 and later).
- A creator name is included in a work only if you enter one yourself. Nothing from your
  device or account is added automatically.
- **Sending as a file (`.myrail` file) does not go through any server.**
  Use this if you prefer not to use links.
- There is a shortcut to post a link you made to X (formerly Twitter).
  **It only opens X's post screen**; the App sends nothing to X.
  You decide whether to post and what to write.
- If you find a work with inappropriate content, please contact support with the number
  shown on the receiving screen. We will check it and delete it.

## Advertising (Google AdMob)

The App shows ads (banner and video ads) from Google AdMob. To serve ads, Google may
collect the advertising identifier (IDFA), device information and similar data.

- At first launch or similar, the App shows the iOS App Tracking Transparency (ATT)
  permission prompt. You can still use the App if you choose "Ask App Not to Track"
  (non-personalized ads are shown)
- For how Google handles information, see:
  - [Google Privacy Policy](https://policies.google.com/privacy)
  - [How Google uses information from sites or apps that use its services](https://policies.google.com/technologies/ads)
  - [AdMob data collection](https://support.google.com/admob/answer/6128543)

If you buy Premium (a one-time purchase through the App Store), no ads are shown and the
ad-related data collection above does not take place.

## Purchases

Premium purchases are processed through the App Store (Apple).
The developer never obtains your payment information, such as credit card details.

## Changes to this policy

This policy may be revised for new features or changes in law. Important changes will be
announced on this page (GitHub repository). The latest version is always on this page.

## Contact

For questions about this policy, please use [Issues](../../issues) in this repository.
