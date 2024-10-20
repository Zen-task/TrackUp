# README

This README would normally document whatever steps are necessary to get the
application up and running.

Things you may want to cover:

* Ruby version

* System dependencies

* Configuration

* Database creation

* Database initialization

* How to run the test suite

* Services (job queues, cache servers, search engines, etc.)

* Deployment instructions

* ...


## users table
ユーザー情報を管理するテーブルです。

| Column             | Type    | Options                    |
|--------------------|---------|----------------------------|
| email              | string  | null: false, unique: true   |
| encrypted_password | string  | null: false                |
| name               | string  | null: false                |

**Association**
- has_many :sessions
- has_many :achievements
- has_many :activity_records
- has_many :shortcuts
- has_many :google_calendar_links

---

## sessions table
サイクルごとの学習や休憩のタイマーセッションを管理するテーブルです。

| Column      | Type      | Options                       |
|-------------|-----------|-------------------------------|
| user        | references | null: false, foreign_key: true |
| start_time  | datetime   | null: false                   |
| end_time    | datetime   | null: false                   |
| activity    | string     | null: false                   |
| duration    | integer    | null: false                   |

**Association**
- belongs_to :user
- has_many :activity_records

---

## achievements table
ユーザーが特定の条件を満たしたときに得られる称号を管理するテーブルです。

| Column       | Type      | Options                       |
|--------------|-----------|-------------------------------|
| user         | references | null: false, foreign_key: true |
| title        | string     | null: false                   |
| description  | text       | null: false                   |
| achieved_at  | datetime   | null: false                   |

**Association**
- belongs_to :user

---

## activity_records table
各セッションの詳細な行動記録を管理するテーブルです。

| Column       | Type      | Options                       |
|--------------|-----------|-------------------------------|
| user         | references | null: false, foreign_key: true |
| session      | references | null: false, foreign_key: true |
| activity     | string     | null: false                   |
| timestamp    | datetime   | null: false                   |

**Association**
- belongs_to :user
- belongs_to :session

---

## shortcuts table
ショートカット機能を管理するテーブルです。

| Column      | Type      | Options                       |
|-------------|-----------|-------------------------------|
| user        | references | null: false, foreign_key: true |
| name        | string     | null: false                   |
| condition   | string     |                               |
| created_at  | datetime   | null: false                   |

**Association**
- belongs_to :user

---

## google_calendar_links table
Googleカレンダーとの連携情報を管理するテーブルです。

| Column              | Type      | Options                       |
|---------------------|-----------|-------------------------------|
| user                | references | null: false, foreign_key: true |
| calendar_event_id   | string     | null: false                   |
| synced_at           | datetime   | null: false                   |

**Association**
- belongs_to :user




## テーブル間のリレーション概要
- **Users と Sessions**: 1対多の関係
- **Users と Achievements**: 1対多の関係
- **Users と ActivityRecords**: 1対多の関係
- **Users と Shortcuts**: 1対多の関係
- **Users と GoogleCalendarLinks**: 1対多の関係
- **Sessions と ActivityRecords**: 1対多の関係
