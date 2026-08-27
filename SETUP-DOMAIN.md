# 独自ドメインの当て方

公開するのは独自ドメインだけにする。`*.github.io` の URL は
別事業の GitHub アカウント名を含むので、どこにも出さない。

## 1. ドメインを取る

`beydex.app` が第一候補。`.app` は Google 管理の TLD で HTTPS が必須
(HSTS プリロード済み) なので、子供向けアプリのサポートページに向く。
取れなければ `beydex.dev`（同じ性質、空きを確認済み）。
`beydex.com` は既に取られている。

取得先はムームードメインか Cloudflare Registrar。
お名前.com は表示価格に「サービス維持調整費」(2026年6月時点 +26%) が乗る。

## 2. DNS を設定する

**A レコード**（apex ドメイン `beydex.app` 用）を4本:

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**AAAA レコード**（IPv6）を4本:

```
2606:50c0:8000::153
2606:50c0:8001::153
2606:50c0:8002::153
2606:50c0:8003::153
```

`www` を使うなら **CNAME** を1本:

```
www  →  <アカウント名>.github.io
```

## 3. GitHub 側に登録する

```bash
gh api -X PUT repos/<アカウント名>/beydex-site/pages -f cname=beydex.app
gh api -X POST repos/<アカウント名>/beydex-site/pages/https_certificate   # HTTPS を有効化
```

DNS が伝わるまで最大24時間。証明書が出るまで数分〜1時間。

## 4. 反映する場所

ドメインとメールアドレスが決まったら、以下を直す:

- `index.html` の `<!-- MAIL -->` の行
- `docs/legal/privacy-ja.md` / `privacy-en.md` の連絡先
- `docs/APP-STORE.md` の「まだ決めていないこと」
- App Store Connect のサポート URL / プライバシーポリシー URL
