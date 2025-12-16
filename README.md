## usersテーブル
| Column          | Type   | Options                                                             |
|:----------------|:-------|:--------------------------------------------------------------------|
| nickname        | string | null: false                                                         |
| email           | string | null: false, unique: true                                           |
| password        | string | null: false, length: { minimum: 6 }, format: { with: 半角英数字混合 } |
| last_name       | string | null: false, format: { with: 全角 }                                  |
| first_name      | string | null: false, format: { with: 全角 }                                  |
| last_name_kana  | string | null: false, format: { with: 全角カタカナ }                            |
| first_name_kana | string | null: false, format: { with: 全角カタカナ }                            |
| date_of_birth   | date   | null: false                                                          |

### Association
- has_many :items
- has_many :orders
- has_many :comments

## itemsテーブル
| Column            | Type       | Options                                  |
|:------------------|:-----------|:-----------------------------------------|
| name              | string     | null: false, length: { maximum: 40 }     |
| description       | text       | null: false, length: { maximum: 1000 }   |
| category_id       | integer    | null: false, ActiveHash                  |
| condition_id      | integer    | null: false, ActiveHash                  |
| shipping_payer_id | integer    | null: false, ActiveHash                  |
| shipping_area_id  | integer    | null: false, ActiveHash                  |
| days_to_ship_id   | integer    | null: false, ActiveHash                  |
| price             | integer    | null: false, ¥300〜¥9,999,999            |
| user_id           | references | null: false, foreign_key: true           |

### Association
- belongs_to :user
- has_one :order
- has_many :comments

## commentsテーブル
| Column  | Type       | Options                        |
|:--------|:-----------|:-------------------------------|
| text    | text       | null: false                    |
| user_id | references | null: false, foreign_key: true |
| item_id | references | null: false, foreign_key: true |

### Association
- belongs_to :user
- belongs_to :item

## ordersテーブル
| Column        | Type       | Options                        |
|:--------------|:-----------|:-------------------------------|
| user_id       | references | null: false, foreign_key: true |
| item_id       | references | null: false, foreign_key: true |
| total_price   | integer    | null: false                    |
| payment_token | string     | null: false                    |
| status        | string     | null: false                    |

### Association
- belongs_to :user
- belongs_to :item
- has_one :shipping_address

## shipping_addressesテーブル
| Column         | Type       | Options                                                          |
|:---------------|:-----------|:-----------------------------------------------------------------|
| postal_code    | string     | null: false, format: { with: ハイフンを含む7桁の半角数字 }           |
| prefecture_id  | integer    | null: false, ActiveHash                                          |
| city           | string     | null: false                                                      |
| street_address | string     | null: false                                                      |
| building_name  | string     | null: true                                                       |
| phone_number   | string     | null: false, format: { with: ハイフン無しの10桁or11桁の半角数字 }     |
| order_id       | references | null: false, foreign_key: true                                   |

### Association
- belongs_to :order
