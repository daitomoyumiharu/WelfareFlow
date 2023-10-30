# WelfareFlow

## 概要

`WelfareFlow`は、福祉施設の間接業務をデジタル化・自動化することを目的としたシステムです。施設スタッフが直接的なケアやサービス提供に集中できるようサポートします。

## 主な機能

1. デジタル書類の自動生成・管理
2. AIアシスタントによるスケジューリング
3. 自動資材発注システム
4. 施設内のメンテナンスAI監視
5. 利用者とのコミュニケーションオートメーション
6. フィードバックループの自動化

## インストール方法

（具体的なインストール手順を記載します。例えば、`git clone`の手順や、依存関係のインストール方法など）

## 使用方法

（システムの基本的な使用方法や操作方法を記載します。）

## 開発環境

- Ruby on Rails
- PostgreSQL
- （その他の技術スタック）

### users テーブル

| Column             | Type    | Options                   |
| ------------------ | ------- | ------------------------- |
| name               | string  | null: false               |
| email              | string  | null: false, unique: true |
| encrypted_password | string  | null: false               |
| last_name          | string  | null: false               |
| first_name         | string  | null: false               |
| last_name_kana     | string  | null: false               |
| first_name_kana    | string  | null: false               |
| birthday           | date    | null: false               |

#### Association

- has_one_attached :avatar
- has_many :documents
- has_many :comments
- has_many :schedules 


<br>

### documents テーブル

| Column             | Type         | Options                        |
| ------------------ | ------------ | ------------------------------ |
| user_id            | references   | null: false, foreign_key: true |
| title              | string       | null: false,                   |
| content            | text         | null: false                    |
| attachment         | string       | null: false                    |




#### Association

- belongs_to :user
- has_one_attached :attachment


<br>

### inventory_items  テーブル

| Column                 | Type       | Options                        |
| ---------------------- | ---------- | ------------------------------ |
| name                   | string     | null: false                    |
| quautity               | integer    | null: false                    |
| reorder_point          | integer    |                                |


#### Association

- has_many :orders


<br>

### orders テーブル

| Column               | Type              | Options                          |
| -------------------- | ----------------- | ---------------------------------|
| inventory_item_id    | references        | null: false, foreign_key: true   |
| quantity             | integer           | null: false                      |
| status               | string            | null: false                      |


#### Association

- belongs_to :inventory_item

<br>

### comments テーブル

| Column    | Type       | Options                  |
| --------- | ---------- | ------------------------ |
| user_id   | references | null: false, foreign_key |
| content   | text       | null: false              |


#### Association

- belongs_to :user

### schedules テーブル

| Column       | Type              | Options                        |
| ------------ | ----------------- | -------------------------------|
| user_id      | references        | null: false, foreign_key: true |
| event        | integer           | null: false                    |
| start_time   | datetime          | null: false                    |
| end_time     | datetime          | null: false                    |

#### Association

- belongs_to :user

### media_contents  テーブル

| Column         | Type         | Options                        |
| -------------- | ------------ | -------------------------------|
| user_id        | references   | null: false, foreign_key: true |
| description    | text         |                                |
| content_type   | string       | null: false                    |
| media          | string       | null: false                    |

#### Association

- belongs_to :user
- has_one_attached :media

##各テーブルの説明
1. **テーブル名**: **`users`説明**:
    - このテーブルはユーザーの情報を保持します。
    - スタッフ、管理者、利用者など、異なる役割のユーザー情報を管理します。
    - ユーザーのプロフィール画像やログイン認証に使用する情報もここに格納されます。
2. **テーブル名**: **`documents`説明**:
    - 福祉施設での業務に関連する文書やファイルを管理します。
    - どのユーザーが文書を作成・編集したかの情報も保存します。
3. **テーブル名**: **`inventory_items`説明**:
    - 施設の在庫アイテム（消耗品や備品など）の情報を管理します。
    - 在庫の数量や再発注のしきい値もこのテーブルで管理します。
4. **テーブル名**: **`orders`説明**:
    - 在庫アイテムの発注情報を保持します。
    - どのアイテムがいつ、どれだけ発注されたか、その状態（処理中、発送済みなど）が保存されます。
5. **テーブル名**: **`comments`説明**:
    - ユーザーからのコメントや意見を収集します。
    - 施設のサービスや活動に関するフィードバックなどがここに格納されます。
6. **テーブル名**: **`schedules`説明**:
    - ユーザーのスケジュールや予定を管理します。
    - 例えば、スタッフの勤務スケジュールや利用者の活動予定などが保存されます。
7. **テーブル名**: **`media_contents`説明**:
    - 画像や動画などのメディアコンテンツを管理します。
    - 施設のイベントの写真や、施設のプロモーション用の動画などがここに保存される可能性があります。
8. **モデル (ActiveHash)**: **`RoleType`説明**:
    - このモデルはデータベースのテーブルとしては存在しない静的なデータを管理するためのものです。
    - ユーザーの役割（例: 管理者, スタッフ, 利用者）など、頻繁に変わることのないデータを効率的に管理するために使用します。