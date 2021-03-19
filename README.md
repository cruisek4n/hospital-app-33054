# アプリ名

hospital-app-33054

# 概要

新規登録し、ログインすれば、受けたい施術の予約を取ることができるようになり、
さらに、クレジットカードで前払いができます。要は、ユーザとして知りたい情報
(予約状況や、施術項目と価格、施術者の経歴、医療機器、地図など)がトップページで一目瞭然になっており、
さらに、予約と決済を手っ取り早く済ませられます。

# 製作背景(意図)

日本の多くの個人病院でまだまだ現金払いしか対応できないことに気づきました。
また、病院のホームページがあるものの、昔のまま更新されず、予約も電話でしか対応していないことにも気づきました。
日本国の政府がキャッシュレスを奨励している今だからこそ、今後町の隅々の病院に至るまで、より手っ取り早く予約・決済が行われるように、
新アプリが登場すれば、既存顧客にも喜ばれるだけでなく新規顧客も増えるだろうと考え、作りはじめました。

# Demo

## 新規登録画面・ログイン画面

## トップページ(予約状況、先生紹介、医療機器、地図、施術項目と費用)

## 施術項目の選択画面

## 予約画面

## 予約詳細画面

## 予約編集画面

## 予約取り消し画面

## 決済画面

# 実装予定の内容

①ユーザ管理機能:名前、電話番号、暗証番号、eメール、性別、生年月日などの情報を入力し、新規登録できるようにします。
ユーザ情報は全てDBに保存できるようにします。

②トップページ一覧表示機能:新規登録し、ログインしたユーザは、受けたい施術項目の予約ページや、決済ページへのアクセスが可能になります。
その際、ユーザとして欲しい情報(先生紹介、医療機器、現在の予約及び支払い状況、地図、脱毛項目と費用)もトップページで確認できます。
また、支払いが済んたら、支払済画面をトップページで表示できるようにします

③施術項目の選択画面:1つ以上の施術項目を選択し、追加するたびに費用も変わるように設定します。
また、施術する項目と費用をDBに保存できるようにします。

④予約画面:都合の良い日、空いているかどうか一目瞭然に表し、予約できるようにします。
日付、施術スタートの時刻、施術終了の時刻をDBに保存できるようにします。

⑤予約詳細機能:ユーザのニックネーム、価格、担当先生、地図、施術項目を明らかにし、表示させるようにします。
「予約編集」、「予約取り消し」、「決済画面へ」といったボタンも表示できるようにします。

⑥予約編集機能:都合が悪くなったら、日時が過ぎていない場合、予約編集できるようにします。その際、施術項目と日時の変更が全部できるようにします。

⑦予約取り消し機能：日時が過ぎていない場合、全ての予約を取り消し、データベースからも削除できるようにします。

⑧決済機能：予約したユーザのみが、決済画面に移動できるようにカード情報を入力できるようにします。
その際、pay.jpを利用し支払い金額や決済日、売り上げがの情報などが記録に残るようにします。


# DB設計

## users テーブル

| Column             | Type   | Options                   |
| -------------------| ------ | --------------------------|
| nickname           | string | null: false, unique: true |
| email              | string | null: false, unique: true |
| encrypted_password | string | null: false               |
| name_kanji         | string | null: false               |
| name_kana          | string | null: false               |
| gender             | string | null: false               |
| birth_date         | date   | null: false               |
 
### Association

- has_many :categories
- has_many :reservations
- has_many :credits

## categories テーブル

| Column                        | Type       | Options                           |
| ------------------------------| ---------- | --------------------------------- |
| face_id                       | integer    | null: false                       |
| chin_id                       | integer    | null: false                       |
| neck_id                       | integer    | null: false                       |
| under_arm_id                  | integer    | null: false                       |
| chest_id                      | integer    | null: false                       |
| abdomen_id                    | integer    | null: false                       |
| vio_id                        | integer    | null: false                       |
| price                         | integer    | null: false                       |
| user                          | references | null: false, foreign_key: true    |

### Association

- belongs_to :user
- has_many :reservations
- has_many :credits

## Reservations テーブル

| Column         | Type       | Options                        |
| ---------      | ---------- | -------------------------------|
| start_datetime | datetime   | null: false                    |
| end_datetime   | datetime   | null: flase                    |          
| user           | references | null: false, foreign_key: true |
| category       | references | null: false, foreign_key: true |

### Association

- belongs_to :user
- belongs_to :category
- has_one :credit

## credits テーブル

| Column         | Type       | Options                        |
| -------------- | ---------- | ------------------------------ |
| user           | references | null: false, foreign_key: true |
| category       | references | null: false, foreign_key: true |
| reservation    | references | null: flase, foreign_key: true |

### Association

- belongs_to :user
- belongs_to :category
- belongs_to :reservation
