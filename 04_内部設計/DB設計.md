---

## 6. データベース設計

### 6.1 論理データモデル

#### users（ユーザー）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | ユーザーID | PK |
| email | VARCHAR(255) | メールアドレス | NOT NULL, UNIQUE |
| username | VARCHAR(50) | ユーザー名 | NOT NULL, UNIQUE |
| password_hash | VARCHAR(255) | ハッシュ化されたパスワード | NOT NULL |
| profile_image_url | VARCHAR(500) | プロフィール画像URL | NULL |
| bio | TEXT | 自己紹介文 | NULL |
| level | INTEGER | ユーザーレベル | NOT NULL, DEFAULT 1 |
| experience | INTEGER | 経験値 | NOT NULL, DEFAULT 0 |
| join_date | TIMESTAMP | 登録日 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| last_login | TIMESTAMP | 最終ログイン日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| is_private | BOOLEAN | プロフィール公開設定 | NOT NULL, DEFAULT FALSE |
| location_sharing_level | VARCHAR(20) | 位置情報共有レベル | NOT NULL, DEFAULT 'area' |
| notification_settings | JSONB | 通知設定 | NULL |
| deleted_at | TIMESTAMP | アカウント削除日時 | NULL |
| status | VARCHAR(20) | アカウント状態 | NOT NULL, DEFAULT 'active' |
| created_at | TIMESTAMP | 作成日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | 更新日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |

#### caterpillars（イモムシ基本データ）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | イモムシID | PK |
| common_name | VARCHAR(100) | 一般名 | NOT NULL |
| scientific_name | VARCHAR(100) | 学名 | NOT NULL |
| description | TEXT | 説明文 | NOT NULL |
| features | TEXT[] | 特徴 | NULL |
| habitats | TEXT[] | 生息環境 | NULL |
| food_plants | TEXT[] | 食草 | NULL |
| seasonal_activity | JSONB | 季節ごとの活動期 | NULL |
| rarity | VARCHAR(20) | レア度 | NOT NULL |
| image_url | VARCHAR(500) | 基準画像URL | NULL |
| butterfly_id | BIGINT | 成虫（蝶/蛾）との関連ID | NULL |
| created_at | TIMESTAMP | 作成日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | 更新日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| is_active | BOOLEAN | 有効/無効フラグ | NOT NULL, DEFAULT TRUE |

#### discoveries（発見記録）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | 発見記録ID | PK |
| user_id | BIGINT | 発見ユーザーID | FK(users.id), NOT NULL |
| caterpillar_id | BIGINT | イモムシID | FK(caterpillars.id), NOT NULL |
| discovery_time | TIMESTAMP | 発見日時 | NOT NULL |
| latitude | DECIMAL(10,7) | 緯度 | NULL |
| longitude | DECIMAL(10,7) | 経度 | NULL |
| location_privacy | VARCHAR(20) | 位置情報公開レベル | NOT NULL, DEFAULT 'area' |
| image_urls | TEXT[] | 画像URL配列 | NOT NULL |
| notes | TEXT | メモ | NULL |
| environment_info | JSONB | 環境情報 | NULL |
| is_public | BOOLEAN | 公開/非公開 | NOT NULL, DEFAULT TRUE |
| likes_count | INTEGER | いいね数 | NOT NULL, DEFAULT 0 |
| comments_count | INTEGER | コメント数 | NOT NULL, DEFAULT 0 |
| created_at | TIMESTAMP | 作成日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | 更新日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |

#### user_collections（ユーザーコレクション）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | コレクションID | PK |
| user_id | BIGINT | ユーザーID | FK(users.id), NOT NULL |
| caterpillar_id | BIGINT | イモムシID | FK(caterpillars.id), NOT NULL |
| first_discovery_id | BIGINT | 初発見記録ID | FK(discoveries.id), NOT NULL |
| discovery_count | INTEGER | 発見回数 | NOT NULL, DEFAULT 1 |
| is_favorite | BOOLEAN | お気に入りフラグ | NOT NULL, DEFAULT FALSE |
| last_seen_date | TIMESTAMP | 最終発見日 | NOT NULL |
| notes | TEXT | ユーザーノート | NULL |
| created_at | TIMESTAMP | 作成日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | 更新日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |

#### posts（投稿）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | 投稿ID | PK |
| user_id | BIGINT | 投稿ユーザーID | FK(users.id), NOT NULL |
| type | VARCHAR(20) | 投稿タイプ | NOT NULL |
| content | TEXT | 投稿内容 | NULL |
| discovery_id | BIGINT | 関連発見記録ID | FK(discoveries.id), NULL |
| image_urls | TEXT[] | 画像URL配列 | NULL |
| latitude | DECIMAL(10,7) | 緯度 | NULL |
| longitude | DECIMAL(10,7) | 経度 | NULL |
| tags | TEXT[] | タグ配列 | NULL |
| likes_count | INTEGER | いいね数 | NOT NULL, DEFAULT 0 |
| comments_count | INTEGER | コメント数 | NOT NULL, DEFAULT 0 |
| visibility_scope | VARCHAR(20) | 公開範囲 | NOT NULL, DEFAULT 'public' |
| created_at | TIMESTAMP | 投稿日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | 更新日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| is_deleted | BOOLEAN | 削除フラグ | NOT NULL, DEFAULT FALSE |

#### comments（コメント）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | コメントID | PK |
| post_id | BIGINT | 対象投稿ID | FK(posts.id), NOT NULL |
| user_id | BIGINT | コメントユーザーID | FK(users.id), NOT NULL |
| content | TEXT | コメント内容 | NOT NULL |
| parent_comment_id | BIGINT | 親コメントID | FK(comments.id), NULL |
| likes_count | INTEGER | いいね数 | NOT NULL, DEFAULT 0 |
| created_at | TIMESTAMP | 投稿日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | 更新日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| is_deleted | BOOLEAN | 削除フラグ | NOT NULL, DEFAULT FALSE |

#### likes（いいね）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | いいねID | PK |
| user_id | BIGINT | ユーザーID | FK(users.id), NOT NULL |
| target_type | VARCHAR(20) | 対象タイプ | NOT NULL |
| target_id | BIGINT | 対象ID | NOT NULL |
| created_at | TIMESTAMP | いいね日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |

#### follows（フォロー関係）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | フォローID | PK |
| follower_id | BIGINT | フォローするユーザーID | FK(users.id), NOT NULL |
| followed_id | BIGINT | フォローされるユーザーID | FK(users.id), NOT NULL |
| status | VARCHAR(20) | ステータス | NOT NULL, DEFAULT 'accepted' |
| created_at | TIMESTAMP | フォロー日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | 更新日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |

#### badges（バッジ）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | バッジID | PK |
| name | VARCHAR(100) | バッジ名 | NOT NULL, UNIQUE |
| description | TEXT | 説明 | NOT NULL |
| category | VARCHAR(30) | カテゴリ | NOT NULL |
| image_url | VARCHAR(500) | バッジ画像URL | NOT NULL |
| required_criteria | JSONB | 獲得条件 | NOT NULL |
| rarity | VARCHAR(20) | レア度 | NOT NULL |
| is_active | BOOLEAN | 有効/無効フラグ | NOT NULL, DEFAULT TRUE |
| created_at | TIMESTAMP | 作成日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | 更新日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |

#### user_badges（ユーザーバッジ）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | ユーザーバッジID | PK |
| user_id | BIGINT | ユーザーID | FK(users.id), NOT NULL |
| badge_id | BIGINT | バッジID | FK(badges.id), NOT NULL |
| earned_date | TIMESTAMP | 獲得日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| is_displayed | BOOLEAN | プロフィール表示フラグ | NOT NULL, DEFAULT TRUE |

#### events（イベント）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | イベントID | PK |
| title | VARCHAR(100) | イベント名 | NOT NULL |
| description | TEXT | 説明 | NOT NULL |
| start_date | TIMESTAMP | 開始日時 | NOT NULL |
| end_date | TIMESTAMP | 終了日時 | NOT NULL |
| image_url | VARCHAR(500) | イベント画像URL | NULL |
| requirements | JSONB | 参加条件 | NULL |
| rewards | JSONB | 報酬情報 | NULL |
| participants_count | INTEGER | 参加者数 | NOT NULL, DEFAULT 0 |
| created_at | TIMESTAMP | 作成日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| updated_at | TIMESTAMP | 更新日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| status | VARCHAR(20) | ステータス | NOT NULL, DEFAULT 'upcoming' |

#### event_participants（イベント参加者）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | 参加ID | PK |
| event_id | BIGINT | イベントID | FK(events.id), NOT NULL |
| user_id | BIGINT | ユーザーID | FK(users.id), NOT NULL |
| join_date | TIMESTAMP | 参加日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| progress | JSONB | 進捗状況 | NULL |
| completion_date | TIMESTAMP | 完了日時 | NULL |
| rank | INTEGER | 順位 | NULL |
| reward_claimed | BOOLEAN | 報酬受け取りフラグ | NOT NULL, DEFAULT FALSE |

#### notifications（通知）
| フィールド名 | データ型 | 説明 | 制約 |
|------------|---------|------|------|
| id | BIGSERIAL | 通知ID | PK |
| user_id | BIGINT | 通知先ユーザーID | FK(users.id), NOT NULL |
| type | VARCHAR(30) | 通知タイプ | NOT NULL |
| source_id | BIGINT | 通知元ID | NULL |
| source_type | VARCHAR(30) | 通知元タイプ | NULL |
| content | TEXT | 通知内容 | NOT NULL |
| is_read | BOOLEAN | 既読フラグ | NOT NULL, DEFAULT FALSE |
| created_at | TIMESTAMP | 作成日時 | NOT NULL, DEFAULT CURRENT_TIMESTAMP |
| expires_at | TIMESTAMP | 有効期限 | NULL |

### 6.2 インデックス設計

#### users
- email（UNIQUE INDEX）
- username（UNIQUE INDEX）
- level（INDEX）
- status（INDEX）

#### caterpillars
- common_name（INDEX）
- scientific_name（INDEX）
- rarity（INDEX）
- CREATE INDEX caterpillars_seasonal_idx ON caterpillars USING GIN (seasonal_activity);

#### discoveries
- user_id（INDEX）
- caterpillar_id（INDEX）
- discovery_time（INDEX）
- CREATE INDEX discoveries_location_idx ON discoveries USING GIST (point(longitude, latitude));
- is_public（INDEX）

#### user_collections
- user_id, caterpillar_id（UNIQUE INDEX）
- user_id（INDEX）
- caterpillar_id（INDEX）

#### posts
- user_id（INDEX）
- type（INDEX）
- created_at（INDEX）
- discovery_id（INDEX）
- visibility_scope（INDEX）

#### comments
- post_id（INDEX）
- user_id（INDEX）
- parent_comment_id（INDEX）

#### likes
- CREATE UNIQUE INDEX likes_user_target_idx ON likes (user_id, target_type, target_id);
- CREATE INDEX likes_target_idx ON likes (target_type, target_id);

#### follows
- follower_id（INDEX）
- followed_id（INDEX）
- CREATE UNIQUE INDEX follows_unique_idx ON follows (follower_id, followed_id);

#### event_participants
- event_id（INDEX）
- user_id（INDEX）
- CREATE UNIQUE INDEX event_participants_unique_idx ON event_participants (event_id, user_id);

#### notifications
- user_id（INDEX）
- is_read（INDEX）
- CREATE INDEX notifications_user_read_idx ON notifications (user_id, is_read);
- created_at（INDEX）

### 6.3 外部キー制約

```sql
-- discoveries テーブルの外部キー
ALTER TABLE discoveries ADD CONSTRAINT fk_discoveries_user
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE;
ALTER TABLE discoveries ADD CONSTRAINT fk_discoveries_caterpillar
    FOREIGN KEY (caterpillar_id) REFERENCES caterpillars(id) ON DELETE RESTRICT;

-- user_collections テーブルの外部キー
ALTER TABLE user_collections ADD CONSTRAINT fk_collections_user
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE;
ALTER TABLE user_collections ADD CONSTRAINT fk_collections_caterpillar
    FOREIGN KEY (caterpillar_id) REFERENCES caterpillars(id) ON DELETE RESTRICT;
ALTER TABLE user_collections ADD CONSTRAINT fk_collections_discovery
    FOREIGN KEY (first_discovery_id) REFERENCES discoveries(id) ON DELETE SET NULL;

-- posts テーブルの外部キー
ALTER TABLE posts ADD CONSTRAINT fk_posts_user
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE;
ALTER TABLE posts ADD CONSTRAINT fk_posts_discovery
    FOREIGN KEY (discovery_id) REFERENCES discoveries(id) ON DELETE SET NULL;

-- comments テーブルの外部キー
ALTER TABLE comments ADD CONSTRAINT fk_comments_post
    FOREIGN KEY (post_id) REFERENCES posts(id) ON DELETE CASCADE;
ALTER TABLE comments ADD CONSTRAINT fk_comments_user
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE;
ALTER TABLE comments ADD CONSTRAINT fk_comments_parent
    FOREIGN KEY (parent_comment_id) REFERENCES comments(id) ON DELETE CASCADE;

-- likes テーブルの外部キー
ALTER TABLE likes ADD CONSTRAINT fk_likes_user
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE;

-- follows テーブルの外部キー
ALTER TABLE follows ADD CONSTRAINT fk_follows_follower
    FOREIGN KEY (follower_id) REFERENCES users(id) ON DELETE CASCADE;
ALTER TABLE follows ADD CONSTRAINT fk_follows_followed
    FOREIGN KEY (followed_id) REFERENCES users(id) ON DELETE CASCADE;

-- user_badges テーブルの外部キー
ALTER TABLE user_badges ADD CONSTRAINT fk_user_badges_user
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE;
ALTER TABLE user_badges ADD CONSTRAINT fk_user_badges_badge
    FOREIGN KEY (badge_id) REFERENCES badges(id) ON DELETE RESTRICT;

-- event_participants テーブルの外部キー
ALTER TABLE event_participants ADD CONSTRAINT fk_event_participants_event
    FOREIGN KEY (event_id) REFERENCES events(id) ON DELETE CASCADE;
ALTER TABLE event_participants ADD CONSTRAINT fk_event_participants_user
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE;

-- notifications テーブルの外部キー
ALTER TABLE notifications ADD CONSTRAINT fk_notifications_user
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE;
```

---