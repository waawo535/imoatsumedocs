
## 8. ER図

```
+---------------+       +----------------+       +------------------+
|    users      |       | caterpillars   |       |  discoveries     |
+---------------+       +----------------+       +------------------+
| id            |<---+  | id             |<------| id               |
| email         |    |  | common_name    |    +--| user_id          |
| username      |    |  | scientific_name|    |  | caterpillar_id   |
| password_hash |    |  | description    |    |  | discovery_time   |
| profile_image_url |  | features       |    |  | latitude         |
| bio           |    |  | habitats       |    |  | longitude        |
| level         |    |  | food_plants    |    |  | location_privacy |
| experience    |    |  | seasonal_activity|   |  | image_urls       |
| join_date     |    |  | rarity         |    |  | notes            |
| last_login    |    |  | image_url      |    |  | environment_info |
| is_private    |    |  | butterfly_id   |    |  | is_public        |
| location_sharing_level| | created_at    |    |  | likes_count      |
| notification_settings| | updated_at    |    |  | comments_count   |
| deleted_at    |    |  | is_active      |    |  | created_at       |
| status        |    |  +----------------+    |  | updated_at       |
| created_at    |    |                        |  +------------------+
| updated_at    |    |                        |
+---------------+    |                        |
      ^              |                        |
      |              |                        |
+-----+--------------+------------------------+---+
|     |              |                        |   |
|     |              |                        |   |
|     |              |                        |   |
| +---+------------+ | +------------------+   |   |  +-------------+
| | user_collections| | |     posts       |<--+   |  |  comments   |
| +----------------+ | +------------------+       |  +-------------+
| | id             | | | id              |<--------->| id          |
| | user_id        | | | user_id         |       |  | post_id     |
| | caterpillar_id | | | type            |       |  | user_id     |
| | first_discovery_id| | content        |       |  | content     |
| | discovery_count| | | discovery_id    |       |  | parent_comment_id|
| | is_favorite    | | | image_urls      |       |  | likes_count |
| | last_seen_date | | | latitude        |       |  | created_at  |
| | notes          | | | longitude       |       |  | updated_at  |
| | created_at     | | | tags            |       |  | is_deleted  |
| | updated_at     | | | likes_count     |       |  +-------------+
| +----------------+ | | comments_count  |       |         ^
|                    | | visibility_scope|       |         |
+--------------------+ | created_at      |       |         |
                       | updated_at      |       |         |
                       | is_deleted      |       |         |
                       +------------------+       |         |
                                                  |         |
+-----------------------+                         |         |
|        likes          |                         |         |
+-----------------------+                         |         |
| id                    |-------------------------+---------+
| user_id               |
| target_type           |
| target_id             |
| created_at            |
+-----------------------+

+---------------+       +----------------+       +-----------------+
|    follows    |       |     badges     |       |  user_badges    |
+---------------+       +----------------+       +-----------------+
| id            |       | id             |<------| id              |
| follower_id   |-------+ name           |       | user_id         |
| followed_id   |       | description    |       | badge_id        |
| status        |       | category       |       | earned_date     |
| created_at    |       | image_url      |       | is_displayed    |
| updated_at    |       | required_criteria|     +-----------------+
+---------------+       | rarity         |
                        | is_active      |
                        | created_at     |
                        | updated_at     |
                        +----------------+

+---------------+       +-------------------+       +------------------+
|    events     |       | event_participants |      |   notifications  |
+---------------+       +-------------------+       +------------------+
| id            |<------| id                |       | id               |
| title         |       | event_id          |       | user_id          |
| description   |       | user_id           |-------+ type             |
| start_date    |       | join_date         |       | source_id        |
| end_date      |       | progress          |       | source_type      |
| image_url     |       | completion_date   |       | content          |
| requirements  |       | rank              |       | is_read          |
| rewards       |       | reward_claimed    |       | created_at       |
| participants_count|   +-------------------+       | expires_at       |
| created_at    |                                   +------------------+
| updated_at    |
| status        |
+---------------+
```

