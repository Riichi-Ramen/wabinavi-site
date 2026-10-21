# HP ドメイン移行メモ（wabinavi.com → GitHub Pages）  2026-10-20/21

## 現在の構成
- HP: GitHub `Riichi-Ramen/wabinavi-site`（main / root）→ **https://wabinavi.com**（独自ドメイン、HTTPS強制 2026-10-21 有効化）
- 旧 `Riichi-Ramen/Riichi-Ramen.github.io`: 独自ドメインは**設定しない**（注文アプリの URL・localStorage・印刷済みQRを守るため）。HPファイルの控えとして残置。
- 注文アプリ: `https://riichi-ramen.github.io/wabinavi-order/`（変更禁止。QR は `?table=1〜14`）
- ドメイン登録元: WordPress.com（登録2021-01-21、自動更新有効、更新日 2026-12-22、約$19）。**支払い方法を有効に保つこと。**

## DNS（WordPress.com 管理画面 → ドメイン → DNS レコード）
- A `@` → 185.199.108.153 / 109.153 / 110.153 / 111.153（TTL 3600）
- CNAME `www` → riichi-ramen.github.io
- 残したもの（触らない）: MX @（メール転送）、NS @、TXT _domainconnect / _dmarc / @(SPF)、CNAME `*`、CNAME wpcloud1/2._domainkey
- 元に戻す場合: 4つのA と www CNAME を削除し「デフォルトのAレコードを復元」。

## 会員制度（未開始）
- HP: `SHOW_MEMBER_FEATURES = false`（index.html / rezerwacja.html）
- GAS: `SHOW_MEMBER_INVITE = false`（24_Web.gs）。**開始時は3か所を同時に true に。**

## 旧URLの転送
- `menu-pl-2/`, `menu-en-2/` → /menu.html、`rezerwacja-reservation-お席のご予約/` → /rezerwacja.html（meta refresh）

## 完了（2026-10-21）
- GAS の `SITE_URL` を `https://wabinavi.com` に変更し再デプロイ済み（`APP_BASE` は変更禁止のまま）。
- https://wabinavi.com で試し予約 → 確認メール・入会案内なし・キャンセルまで正常動作を確認。

## 未完了
1. 数日動作確認後、WordPress.com 側の整理（Business プラン見直し。ドメインは更新を続ける）。旧WPページ内の `ramen@wabinavi.com` 記載・旧予約ページへのリンクも不要。
2. （任意）ギャラリー/トップ写真を foto: メール＋スプレッドシートで管理する仕組み。wabinavi-site を書き込み先にして実装（`wabinavi-gas` の foto 処理コードが必要）。
3. （任意）メニューの GAS 側キャッシュ化（getMenu が初回十数秒かかる件）。

## 今回のHP変更点
- 寿司: ギャラリー・トップの Maki カードを「準備中」表示（写真非表示）。
- ギャラリー: Starters/Rice の誤分類を整理、うな丼の重複削除。
- メニュー: 前回内容の即表示＋裏更新（localStorage キー `wabi_site_menu_v2`）、トップから先読み、「Pojemniki na Wynos」カテゴリ非表示。
- rezerwacja.html: 月曜は画面内の多言語案内、noindex 削除。
