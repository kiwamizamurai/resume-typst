---
name: cv-create
description: |
  職務経歴書のYAMLコンテンツを対話形式で作成・更新し、Typstでコンパイルして cv.pdf を生成するスキル。
  「職務経歴書を作りたい」「CVを書いて」「経歴を入力したい」「スキルを更新して」「STAR法で職歴を書きたい」
  「プロジェクト経験を追加して」など、職務経歴書の内容入力・更新・PDF生成に関するあらゆる場面でこのスキルを使うこと。
---

# 職務経歴書作成スキル

`cv/content/` 以下のYAMLファイルを対話形式で埋めて、`mise run cv` でPDFを生成する。

## プロジェクトパス

```
cv/content/
  profile.yaml   # 基本情報・スキルサマリー
  overview.yaml  # 職歴概要（会社ごとのサマリー）
  jobs.yaml      # 業務詳細（プロジェクト単位）
```

## ワークフロー

### Step 1: 基本情報

氏名・ふりがな・生年月日・年齢・最寄り駅を AskUserQuestion で収集。

### Step 2: スキルサマリー

以下を収集（複数行OKの自由記述として聞く）：
- 言語・フレームワーク
- インフラ・クラウド
- ツール・その他
- 趣味・関心

収集後、`cv/content/profile.yaml` を書き出す：

```yaml
name: "姓 名"
kana: "せい めい"
birthday: "1990年1月1日"
age: "満XX歳"
nearest_station: "○○駅"
skills: |
  言語、フレームワーク: ...
  インフラ・クラウド: ...
  ツール: ...
hobbies: |
  ...
experience:
  environments: |
    ...
  languages: |
    ...
  frameworks: |
    ...
  infrastructure: |
    ...
  tools: |
    ...
```

### Step 3: 職歴概要（overview.yaml）

勤務した会社を新しい順に1社ずつ収集：
- 会社名
- 在籍期間（例: 2023年10月 ～ 2025年7月）
- 業務内容（会社概要 + 主な担当業務）
- 技術スタック（フロントエンド・バックエンド・インフラ・ツールなど）

`cv/content/overview.yaml` を書き出す：

```yaml
- title: "株式会社○○"
  period: "2023年10月 ～ 2025年7月"
  content: |
    ○○向けサービスを展開している会社。

    [業務内容]

    - ...
  tech_stack: |
    フロントエンド: ...
    バックエンド: ...
    インフラ: ...
    ツール: ...
```

### Step 4: 業務詳細（jobs.yaml）— STAR法で記述

プロジェクト単位で詳細を収集。「STAR法で整理すると伝わりやすいですよ」と案内しながら以下を聞く：

- プロジェクト名・会社名
- 期間（from/to: "YY/MM" 形式、span: "X年Xヶ月"）
- プロジェクト概要（業種・サービス種別・チーム規模）
- **S**ituation: 背景・課題（どんな状況だったか）
- **T**ask: 自分の役割・担当
- **A**ction: 具体的に行ったこと
- **R**esult: 成果・数値・改善
- 使用ツール
- 使用言語・FW・インフラ

STAR の内容は content フィールドにまとめて書く。

`cv/content/jobs.yaml` を書き出す：

```yaml
- title: "株式会社○○ ○○開発"
  period:
    from: "23/10"
    to: "25/7"
    span: "1年10ヶ月"
  workdetail:
    kind: "業種"
    type: "サービス種別"
    size: "X名"
  content: |
    [背景]
    ...

    [担当・役割]
    ...

    [取り組み]
    - ...

    [成果]
    - ...
  use-soft: |
    Git
    GitHub Actions
    Slack
  lang-othor: |
    TypeScript
    React
    AWS
```

### Step 5: ビルド

```bash
mise run cv
```

成功したら「`cv/cv.pdf` を生成しました。`open cv/cv.pdf` で確認できます。」と伝える。

## 注意点

- STAR法は強制しない。「まとめにくければ箇条書きでもOK」と伝える
- 数値で語れる成果（処理速度X%改善、売上X%増など）があれば積極的に引き出す
- 既存のYAMLがある場合は上書き前に「既存の内容を上書きしますがよいですか？」と確認する
- 各ステップ完了後に即座にYAMLを書き出す
- 会社・プロジェクトは「もう1件ありますか？」と毎回確認してループする
