# fleamarket_sample_76b

## 概要
プログラミングスクール「TECH CAMP」にてフリーマーケットのアプリケーションを作成しました。

## 接続先情報
- デプロイ先：http://54.95.199.71/

- ID/Pass
  - ID: admin
  - Pass: 7676b
  
- テスト用アカウント
  - 購入者用アカウント
    - メールアドレス：buyer@buyer
    - パスワード：bbbbbbb
  - 購入用カード情報
    - 番号：4242424242424242
    - 期限：10/24
    - セキュリティコード：380
  - 出品者用
    - メールアドレス：seller@seller
    - パスワード：sssssss

## 開発状況
- 開発環境
  - Ruby/Ruby on Rails/MySQL/Github/AWS/Visual Studio Code
  
- 開発期間と平均作業時間
  - 開発期間：約4週間
  - 1日あたりの平均作業時間：約8時間
  
- 開発体制
  - 人数：6名
  - アジャイル型開発（スクラム）
  - Trelloによるタスク管理

## DB設計
### ①usersテーブル
|Column|Type|Options|
|------|----|-------|
|nickname|string|null: false|
|email|string|null: false|
|password|string|null: false|
|last_name|string|null: false|
|first_name|string|null: false|
|last_name_kana|string|null: false|
|first_name_kana|string|null: false|
|birthday|date|null: false|

#### Association
- has_one :address
- has_one :credit

- has_many :selling_products, -> { where("buyer_id is NULL") },class_name: 'Product', foreign_key: 'seller_id'
- has_many :sold_out_products, -> { where("buyer_id is not NULL") },class_name: 'Product', foreign_key: 'seller_id'


#### index
- add_index :users, :email,unique: true

### ②productsテーブル
|Column|Type|Options|
|------|----|-------|
|seller_id|integer|null: false, foreign_key: true|
|buyer_id|integer|foreign_key: true|
|name|string|null: false|
|content|string|null: false|
|category_id|integer|null: false, foreign_key: true|
|bland_name|string||
|price|integer|null: false|
|product_status_id|integer|null: false|
|delively_cost_id|integer|null: false|
|prefecture_id|integer|null: false|
|delively_days_id|integer|null: false|
|delively_method_id|integer|null: false|

#### Association
- belongs_to :category
- belongs_to_active_hash :product_status
- belongs_to_active_hash :delively_cost
- belongs_to_active_hash :prefecture
- belongs_to_active_hash :delively_days
- belongs_to_active_hash :delively_method

- belongs_to :seller, class_name: 'User', foreign_key: 'seller_id'
- belongs_to :buyer, class_name: 'User', optional: true, foreign_key: 'buyer_id'

- has_many :images


### ③categoriesテーブル
|Column|Type|Options|
|------|----|-------|
|name|string|null: false|
|ancestry|string||

#### Association
- has_ancestry
- has_many :products

#### index
- add_index :categories, :ancestry

### ④imagesテーブル
|Column|Type|Options|
|------|----|-------|
|product_id|integer|null: false, foreign_key: true|
|src|string|null: false|

#### Association
- belongs_to :product

### ⑤addressesテーブル
|Column|Type|Options|
|------|----|-------|
|user_id|integer|null: false, foreign_key: true|
|delively_last_name|string|null: false|
|delively_first_name|string|null: false|
|delively_last_name_kana|string|null: false|
|delively_first_name_kana|string|null: false|
|postcode|string|null: false|
|prefecture_id|integer|null: false|
|city|string|null: false|
|block|string|null: false|
|building|string||
|phone_number|string||

#### Association
- belongs_to :user
- belongs_to_active_hash :prefecture

### ⑥creditsテーブル
|Column|Type|Options|
|------|----|-------|
|user_id|integer|null: false, foreign_key: true|
|customer_id|string|null: false|
|card_id|string|null: false|

#### Association
- belongs_to :user

### ⑦commentsテーブル
|Column|Type|Options|
|------|----|-------|
|user_id|integer|null: false, foreign_key: true|
|product_id|integer|null: false, foreign_key: true|
|text|string||

#### Association
- belongs_to :user
- belongs_to :product
