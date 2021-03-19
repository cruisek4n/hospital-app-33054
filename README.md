# テーブル設計

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
