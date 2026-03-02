---
name: resume-create
description: |
  履歴書（JIS規格）のYAMLコンテンツを対話形式で作成・更新し、Typstでコンパイルして resume.pdf を生成するスキル。
  「履歴書を作りたい」「履歴書を書いて」「resume作って」「個人情報を更新して」「学歴・職歴を入力したい」など、
  履歴書の内容入力・更新・PDF生成に関するあらゆる場面でこのスキルを使うこと。
---

# 履歴書作成スキル

`resume/content/` 以下のYAMLファイルを対話形式で埋めて、`mise run resume` でPDFを生成する。

## プロジェクトパス

```
resume/content/
  profile.yaml          # 氏名・生年月日
  address.yaml          # 住所・連絡先
  education_history.yaml # 学歴
  work_history.yaml     # 職歴
  certifications.yaml   # 免許・資格
  motivation_requests.yaml # 志望動機・希望条件
```

## ワークフロー

セクションごとに AskUserQuestion で情報を収集し、順番にYAMLへ書き出す。

### Step 1: 氏名・ふりがな

AskUserQuestion でそれぞれ聞く（まとめて聞くと入力しにくいので分割）。

収集後、`resume/content/profile.yaml` を書き出す：

```yaml
性読み: "ふりがな（姓）"
名読み: "ふりがな（名）"
性: "姓"
名: "名"
生年月日: "平成X年X月X日"
満年齢: "XX"
写真: ""
```

### Step 2: 住所・連絡先

現住所（郵便番号・住所・ふりがな・電話・Email）を聞く。
連絡先が現住所と異なる場合のみ2つ目も聞く（不要なら空白のまま）。

`resume/content/address.yaml` を書き出す：

```yaml
住所ふりがな1: "ふりがな"
住所1: "住所"
郵便番号1: "000-0000"
電話番号1: "000-0000-0000"
Email1: "example@example.com"
住所ふりがな2:
住所2:
郵便番号2:
電話番号2:
Email2:
```

### Step 3: 学歴

「高校から順番に入力してください。終わったら『完了』と教えてください。」と案内し、
1エントリずつ AskUserQuestion で収集（年・月・内容）。

`resume/content/education_history.yaml` を書き出す：

```yaml
- 年: "2014"
  月: "3"
  学歴: "○○高校 卒業"
- 年: "2014"
  月: "4"
  学歴: "○○大学 ○○学部 入学"
```

### Step 4: 職歴

「最初の職歴から順番に入力してください。終わったら『完了』と教えてください。」と案内し、
1エントリずつ（年・月・内容）収集。

`resume/content/work_history.yaml` を書き出す：

```yaml
- 年: "2018"
  月: "4"
  職歴: "株式会社○○ 入社"
- 年: "2022"
  月: "3"
  職歴: "株式会社○○ 退社"
```

### Step 5: 免許・資格

保有している免許・資格を取得順に収集（年・月・内容）。なければスキップ。

`resume/content/certifications.yaml` を書き出す：

```yaml
- 年: "2020"
  月: "6"
  資格: "普通自動車免許 取得"
```

### Step 6: 志望動機・希望条件

- 志望動機（自由記述）
- 本人希望（給与・勤務地など。なければ空白）

`resume/content/motivation_requests.yaml` を書き出す：

```yaml
志望動機: "..."
本人希望: "..."
```

### Step 7: ビルド

```bash
mise run resume
```

成功したら「`resume/resume.pdf` を生成しました。`open resume/resume.pdf` で確認できます。」と伝える。

## 注意点

- 各ステップ後に即座にYAMLを書き出す（全部聞いてからまとめて書かない）
- 年号は「令和」「平成」など元号で入力されたらそのままYAMLに書く
- 入力をスキップしたいセクションはユーザーが「スキップ」「なし」と言ったら空のYAMLを書いて次へ進む
- 既存のYAMLがある場合は上書き前に「既存の内容を上書きしますがよいですか？」と確認する
