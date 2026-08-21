# AS1 Studio 公式サイト

Google Play Console の**組織アカウントへの変更**に必要な、
「アカウントの詳細」のウェブサイト確認に使うサイト。

- 公開 URL: `https://as1studio.app/`
- ホスティング: GitHub Pages（ユーザーサイト）
- ドメイン: Cloudflare Registrar

## ファイル

| ファイル | 役割 |
|---|---|
| `index.html` | サイト本体。事業内容／提供アプリ／事業者情報 |
| `CNAME` | カスタムドメインの指定。**消さないこと**（消えると独自ドメインが外れる） |
| `.nojekyll` | Jekyll を通さず HTML をそのまま配信させる |

## 置き場所（重要）

**GitHub のユーザーサイト用リポジトリに置く。**

- リポジトリ名: `centralfield001-source.github.io`（この名前でないとユーザーサイトにならない）

**サブパスではなくドメインの直下に置く理由。**
Google は「その組織の公式サイト」として人が見て確認する。
`/positive-diary-privacy/` はアプリ1本の付属ページに見えるため、組織の代表サイトとしては弱い。

## 既存のプライバシーポリシーはどうなるか

**何もしなくてよい。** ユーザーサイトにカスタムドメインを設定すると、
**同じアカウントのプロジェクトサイトも自動でカスタムドメイン配下に移る**（GitHub の仕様）。

```
https://as1studio.app/                          ← このサイト
https://as1studio.app/positive-diary-privacy/   ← 既存のプライバシーポリシー（自動）
```

`positive-diary-privacy` リポジトリは触らない。`index.html` からの相対リンク
`./positive-diary-privacy/` はこのままで正しく繋がる。

**旧 URL は新 URL へリダイレクトされる**ので、Play Console のプライバシーポリシー URL は
すぐ壊れることはないが、**落ち着いたら新 URL に書き換える**こと（アプリの更新も再ビルドも不要）。

## 公開の手順

```
# 1. GitHub で centralfield001-source.github.io という名前のリポジトリを作る（Public）

# 2. このフォルダを push
git init
git add .
git commit -m "AS1 Studio 公式サイト"
git branch -M main
git remote add origin https://github.com/centralfield001-source/centralfield001-source.github.io.git
git push -u origin main

# 3. GitHub → Settings → Pages
#    Source: Deploy from a branch / Branch: main / フォルダ: / (root)
```

`CNAME` を同梱してあるので、**push した時点でカスタムドメインが設定される**。
Settings → Pages のカスタムドメイン欄に `as1studio.app` が入っていることを確認する。

**先に画面から設定して後から push すると、CNAME が上書きで消えることがある。**
同梱しているのはそれを避けるため。

## 4. DNS の確認と HTTPS

Settings → Pages でドメインのチェックが通るまで待つ（数分〜）。
通ったら **Enforce HTTPS** にチェックを入れる。証明書の発行まで最大24時間。

**`.app` は HTTPS 必須の TLD** なので、証明書が出るまでサイトは開かない。壊れたわけではない。

**Cloudflare 側のプロキシは DNS only（グレー雲）のままにする。**
オレンジ雲にすると Let's Encrypt の更新が遮られ、数ヶ月後に突然 SSL エラーになる。

## そのあと

1. **Search Console** に `https://as1studio.app/` を URL プレフィックスで登録し、所有権を確認する
   （HTML ファイルをこのリポジトリに置くか、meta タグを `index.html` の `<head>` に入れる）
2. **Play Console** → デベロッパー アカウント → アカウントの詳細 → ウェブサイトに
   `https://as1studio.app/` を入れて**確認リクエストを送信**する
3. 承認されると**「アカウントの種類を変更」**が押せるようになる

## 直す必要があるかもしれない箇所

- **屋号**: `AS1 Studio`。**開業届・DUNS・Play Console の4か所と完全に一致させること**
- **所在地**: 公開済みのプライバシーポリシーと同じ表記にしてある。DUNS 申請にも同じ日本語表記を使う
  （Cloudflare の登録者情報だけは ASCII 指定のためローマ字。混ぜないこと）
- **代表者名**: 意図的に載せていない。組織アカウントの確認は組織名と所在地の一致を見るため、
  氏名の掲載は求められない。載せると屋号で通す意味が薄れる
- **開業年**: 分からないため書いていない。信頼性を上げたければ事業者情報に1行足す
