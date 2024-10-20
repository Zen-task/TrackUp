# README
このアプリケーションは、ユーザーのタイマーセッションや行動記録をシンプルに管理します。

## テーブル設計

### users table
ユーザー情報を管理するテーブルです。

| Column             | Type    | Options                    |
|--------------------|---------|----------------------------|
| email              | string  | null: false, unique: true   |
| encrypted_password | string  | null: false                |
| name               | string  | null: false                |

**Association**
- has_many :sessions

---

### sessions table
各サイクルごとの学習や休憩のタイマーセッションを管理するテーブルです。各セッションには一つのアクティビティが紐づきます。

| Column      | Type      | Options                       |
|-------------|-----------|-------------------------------|
| user        | references | null: false, foreign_key: true |
| start_time  | datetime   | null: false                   |
| end_time    | datetime   | null: false                   |
| activity    | string     | null: false                   |
| duration    | integer    | null: false                   |

**Association**
- belongs_to :user

---

### achievements table
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

### shortcuts table
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

### google_calendar_links table
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
- **Users と Shortcuts**: 1対多の関係
- **Users と GoogleCalendarLinks**: 1対多の関係
