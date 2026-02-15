# 🚀 Next.js + Spring Boot プロジェクト規約および全体フロー

---

## 1️⃣ プロジェクト概要

| 区分 | 内容 |
|------|------|
| プロジェクト形態 | 4人チーム協業Webサービス |
| フロントエンド | Next.js（App Router） |
| バックエンド | Spring Boot（REST API） |
| DB | PostgreSQL |
| サーバー | AWS |

---

## 2️⃣ 全体アーキテクチャフロー

[ Client (Browser) ]

        ↓

[ Next.js (Frontend) ]

        ↓  REST API (JSON)

[ Spring Boot (Backend) ]

        ↓

[ Database (PostgreSQL) ]


### 役割分離

- Next.js → UIレンダリング + API呼び出し担当  
- Spring Boot → ビジネスロジック + DB処理担当  
- すべてのデータ通信は JSON ベースの REST API を使用  

---

## 3️⃣ Git & ブランチ戦略

### 📌 3.1 ブランチ構成

| ブランチ | 説明 |
|----------|------|
| master | 本番デプロイ可能な安定ブランチ |
| develop | 開発統合ブランチ |
| ST | ST用ブランチ |
| LT | LT用ブランチ |
| feature/{機能名} | 機能単位の開発ブランチ（このブランチでUT実施） |
| hotfix/{イシュー名} | 緊急修正用（このブランチでUT実施） |

---

### 📌 3.2 作業フロー

- develop から開発ブランチを作成  
- 機能開発後、Pull Request を作成  
- 開発ブランチで UT 実施  
- コードレビュー1名以上の承認必須（任意）  
- UTブランチの成果物を LTブランチへマージ後、LT実施  
- LTブランチの成果物を STブランチへマージ後、ST実施  

---

### 📌 3.3 機密情報管理

- APIキーやDBパスワードなどの機密情報をコードにハードコーディング禁止  
- `.env` ファイルや `application.properties` などの環境変数で管理  
- `.gitignore` に機密情報ファイルを追加  
- 公開リポジトリ（GitHub等）へ機密情報を絶対にアップロードしない  
- 必要に応じて AWS Secrets Manager、GitHub Secrets などのセキュリティツールを活用  

---

## 4️⃣ コミットメッセージ規則

### 例)
feat: ログインAPI実装

fix: 会員登録バリデーションエラー修正

refactor: ユーザーサービス構造改善


| 타입	| 설명| 
|----------|------|
| feat	| 新規| 
| fix	| バグ修正| 
| refactor	| リファクタリング| 
| docs	| ドキュメント修正 |
| chore | 設定／ビルド関連 |
## 5️⃣ バックエンド規約（Spring Boot）

### 📌 5.1 パッケージ構造
```
com.example.project
 ├─ controller
 │   └─ user
 │       └─ UserController.java
 │
 ├─ service
 │   └─ user
 │       ├─ UserService.java
 │       └─ UserServiceImpl.java
 │
 ├─ repository
 │   └─ user
 │       ├─ UserRepository.java
 │       ├─ UserRepositoryCustom.java
 │       └─ UserRepositoryImpl.java
 │
 ├─ mapper
 │   └─ user
 │       ├─ UserMapper.java
 │       └─ UserMapper.xml
 │
 ├─ domain
 │   └─ user
 │       ├─ User.java
 │       ├─ UserPolicy.java
 │       └─ UserValidator.java
 │
 ├─ dto
 │   └─ user
 │       ├─ UserRequestDto.java
 │       ├─ UserResponseDto.java
 │       └─ UserSummaryDto.java
 │
 ├─ exception
 │   ├─ GlobalExceptionHandler.java
 │   ├─ BusinessException.java
 │   └─ ErrorCode.java
 │
 └─ config
     ├─ JpaConfig.java
     ├─ MyBatisConfig.java
     ├─ WebConfig.java
     └─ SecurityConfig.java
```

### 📌 5.2 アーキテクチャ & コーディング規則

#### 🔹 Controller

- リクエスト（Request）とレスポンス（Response）のみ処理  
- ビジネスロジックは Service へ委譲  
- URI設計およびHTTPステータスコード管理  

#### 🔹 Service

- コアビジネスロジックを担当  
- `@Transactional` は Service 層で管理  
- Repository または Mapper を通じてデータアクセス  
- インターフェース／実装クラスを分離  

#### 🔹 Repository（JPA）

- JPAベースのデータアクセス  
- 基本CRUDは `JpaRepository` または `CrudRepository` 使用  
- 複雑なクエリは Custom Repository 使用  
- Entity単位でデータ操作  

#### 🔹 Mapper（MyBatis）

- SQL中心の複雑クエリ処理  
- インターフェース + XML 分離  
- JPAとの役割を明確に区分  

#### 🔹 Domain

- Entityおよびドメインロジック管理  
- Validator / Policy を含む  
- Entity と DTO を明確に分離  

#### 🔹 DTO

- Request / Response 転送オブジェクト  
- Controller ↔ Service 間データ伝達  
- APIレスポンス構造の柔軟性確保  

#### 🔹 Exception

- グローバル例外処理  
- `@ControllerAdvice` 使用  
- 共通エラーコード管理  

#### 🔹 Config

- JPA設定  
- MyBatis設定  
- Web設定（CORS、Interceptorなど）  
- Security設定  

---

## 6️⃣ API規約

### 📌 URL規則

- `/api/v1/...` 形式使用  
- 名詞は複数形使用  


GET    /api/v1/users
POST   /api/v1/users
GET    /api/v1/users/{id}

### 📌 APIレスポンス形式

#### 正常レスポンス
{
  "success": true,
  "data": {...},
  "message": null
}

エラーレスポンス

{
  "success": false,
  "data": null,
  "message": "INVALID_REQUEST"
}

## 7️⃣ 開発進行フロー

| 段階 | 内容 | 成果物 |
|------|------|--------|
| 1 | 要件定義 | 要件定義書、機能説明書、画面設計書、画面遷移図 |
| 2 | 基本設計 | API仕様書、テーブル定義書、共通例外／エラー処理規則書 |
| 3 | 詳細設計 | 詳細設計書 |
| 4 | バックエンドAPI開発 | コード、DBスキーマ／初期データスクリプト |
| 5 | UT | UT仕様書、テストコード、テスト結果 |
| 6 | LT | LT仕様書、テストコード、テスト結果 |
| 7 | ST | ST仕様書、テストコード、テスト結果 |

## 8️⃣ 協業ルール

週1回以上進捗共有

PRは小さく、機能単位で作成

共通規約変更時はドキュメント更新必須

判断に迷う実装は単独決定せず共有

## 9️⃣ その他

共通定数／Enumは中央管理

ログレベル区分（INFO / WARN / ERROR）

TODOはイシュー登録後対応

※ 本ドキュメントはプロジェクト進行に伴い継続的に更新する。
