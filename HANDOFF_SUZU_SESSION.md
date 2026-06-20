# 引き継ぎ: 運命数計算ツール → すずセッションへ

- date: 2026-06-20
- from: kimi-researchセッション
- to: すずセッション（x-quote-research/note_articles/suuji/ オーナー）

## 本番情報

| 項目 | 値 |
|---|---|
| **本番URL** | https://suzu-suuhi-calc.vercel.app/ |
| GitHubリポ | https://github.com/planetofflower-alt/suzu-suuhi-calc |
| ローカルパス | `~/Projects/x-quote-research/tools/suzu_calc/` |
| ホスト | Vercel Hobby（個人無料・$0/月） |
| デプロイ方式 | mainブランチ push → 自動再デプロイ |
| 設計 | 純クライアントサイドJS・外部API/secrets無し |

## 現状の動作

生年月日入力 → 運命数算出（1-9, 11, 22, 33）→ マスター判定 → 1行性格＋つまずき → CTA「取扱説明書を読む →」

## ⚠️ 改造してほしいこと（優先順）

### 1. 取説CTAを直リンク化（最優先・必須）
現状の `note.com/suzu_suuhi/search?q=...`（note内検索）を、各運命数の**実URL**に置換。すずセッションは全12記事のURL（`note.com/suzu_suuhi/n/nXXXXXX`）を `POSTING_MANIFEST_codex_HANDOFF.md` か `06_次セッション_持ち越しタスク.md` で把握しているはず。

修正箇所＝ `index.html` の `PROFILES` 定数の各 `title` を URL に置換、または別フィールド `url` を足す。`noteSearchUrl()` 関数も不要に。

```js
const PROFILES = {
  1: { essence: '...', stumble: '...', url: 'https://note.com/suzu_suuhi/n/nXXX' },
  // ...
};
```

### 2. テキスト微調整（任意）
`essence`/`stumble` の1行コピーを、すずの「ですます温め文体」と完璧に合わせる。今は本文12記事と整合的に書いたが、すずセッションが本家正本を持ってるので最終調整推奨。

### 3. すず全25記事への貼り込み（オペ作業・別タスク）
固定ポスト＋取説12＋決断12＋相性10。冒頭または末尾に：
```
▼ まず「自分の運命数」を出してください（30秒・無料）
https://suzu-suuhi-calc.vercel.app/
```
ミラ式更新ルート（editor→insertHTML→一時保存→公開に進む→更新する）で投入。

## 改造→反映の手順

```bash
cd ~/Projects/x-quote-research/tools/suzu_calc
# 編集（Edit toolでindex.htmlのPROFILES差替）
git add -A && git commit -m "feat: 取説CTAを各運命数の実URLへ直リンク化"
git push origin main
# → Vercelが30秒で自動再デプロイ・URLは不変
```

## 動作確認

- `https://suzu-suuhi-calc.vercel.app/` ブラウザで開く
- 1995-03-21 → 運命数3 / 1989-12-08 → 運命数11(マスター) 検証パス済

## ヒロシ規律準拠ポイント

- secrets無し（grep済）・public repo（流出する物が無いことの証明）
- 自動デプロイは main push のみ・preview deploymentsはbranch切れば任意
- 動作変更は必ずヒロシGOで（投稿バッチ含む）
