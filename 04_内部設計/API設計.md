---

## 7. API設計

### 7.1 認証API

#### POST /api/auth/register
新規ユーザー登録
- リクエスト：
  ```json
  {
    "email": "user@example.com",
    "password": "securePassword",
    "username": "username"
  }
  ```
- レスポンス：
  ```json
  {
    "status": "success",
    "data": {
      "userId": 123,
      "username": "username",
      "token": "jwt-token"
    }
  }
  ```

#### POST /api/auth/login
ユーザーログイン
- リクエスト：
  ```json
  {
    "email": "user@example.com",
    "password": "securePassword"
  }
  ```
- レスポンス：
  ```json
  {
    "status": "success",
    "data": {
      "userId": 123,
      "username": "username",
      "token": "jwt-token"
    }
  }
  ```

#### POST /api/auth/refresh
トークン更新
- リクエスト：
  ```json
  {
    "refreshToken": "refresh-token"
  }
  ```
- レスポンス：
  ```json
  {
    "status": "success",
    "data": {
      "token": "new-jwt-token",
      "refreshToken": "new-refresh-token"
    }
  }
  ```

#### POST /api/auth/logout
ログアウト
- リクエスト：認証ヘッダーのみ
- レスポンス：
  ```json
  {
    "status": "success"
  }
  ```

### 7.2 ユーザーAPI

#### GET /api/users/me
自分のプロフィール取得
- リクエスト：認証ヘッダーのみ
- レスポンス：
  ```json
  {
    "status": "success",
    "data": {
      "userId": 123,
      "username": "username",
      "email": "user@example.com",
      "profileImage": "image-url",
      "bio": "自己紹介",
      "level": 10,
      "experience": 1500,
      "stats": {
        "discoveries": 25,
        "badges": 8,
        "followers": 12,
        "following": 20
      }
    }
  }
  ```

#### PUT /api/users/me
プロフィール更新
- リクエスト：
  ```json
  {
    "username": "newUsername",
    "bio": "新しい自己紹介",
    "profileImage": "new-image-url"
  }
  ```
- レスポンス：
  ```json
  {
    "status": "success",
    "data": {
      "userId": 123,
      "username": "newUsername",
      "bio": "新しい自己紹介",
      "profileImage": "new-image-url"
    }
  }
  ```

#### GET /api/users/{userId}
他ユーザーのプロフィール取得
- リクエスト：パスパラメータ userId
- レスポンス：
  ```json
  {
    "status": "success",
    "data": {
      "userId": 456,
      "username": "otherUser",
      "profileImage": "image-url",
      "bio": "自己紹介",
      "level": 15,
      "stats": {
        "discoveries": 45,
        "badges": 12,
        "followers": 30,
        "following": 25
      },
      "isFollowing": true
    }
  }
  ```
|