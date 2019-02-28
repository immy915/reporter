# データベース設計

- ~~postgres 11.2~~
- ~~接続：`psql -U postgres -W`~~
- ~~切断：`\q`~~
- 当初はpostgresql使用としていたが、レンタルサーヴァーではMySQLのほうが選択が広がるため、MySQLとする
    - https://teratail.com/questions/22388
        - > レンタルサーバを使うよりは閉じた環境（例えばvmwareやvirtualboxで仮想環境上にlinuxサーバを立てる）という方向性を検討してもいいかと思いますよ。
        - 仮想環境について要勉強
        - 必要な容量の検討
- データベース名：reporter

## ユーザー情報(table:`user_info`)

- ユーザーid(`user_id`)
    - 主キー
    - ユニーク
    - varchar(20)
- 表示名(`user_name`)
    - varchar(20)
- パスワード(`password`)
    - varchar(20)
- メールアドレス(`address`)
    - ユニーク
    - varchar(100)
- 推し(`oshi`)
    - varchar(20)

### SQL文

`CREATE TABLE user_info (user_id varchar(20) PRIMARY KEY UNIQUE, user_name varchar(20), password varchar(20), address varchar(100), oshi varchar(20));`
`INSERT user_info VALUES ('test', 'テストさん', 'pass' , 'test@test.co.jp', 'ヒュー・ジャックマン');`

## レポートID(table:`id_relation`)

- ユーザーid(`user_id`)
    - 主キー
    - ユニーク
    - varchar(20)
- レポートid(`report_id`)
    - 主キー
    - ユニーク
    - varchar(20)
        - 日時とユーザー名のハッシュ値とか？
        - SHA-1

### SQL文

`CREATE TABLE id_relation (user_id varchar(20) PRIMARY KEY UNIQUE, report_id varchar(20) UNIQUE);`

## レポート情報(table:`reports`)

- レポートid(`report_id`)
    - 主キー
    - ユニーク
    - verchar(20)
- 公演名(`title`)
    - text
- 本文(`report_text`)
    - text
- 公開可否(`visibility`)
    - int(1)
        - true（可） = 1
        - false（否） = 0
    - default: 1
- ネタバレフラグ(`netabare`)
    - int(1)
        - true = 1
        - false = 0
    - default: 1
- 日時(`day`)
    - datetime
- 時間(`type`)
    - int(1)
        - default(指定なし) = 0
        - 昼公演 = 1
        - 夜公演 = 2
        - その他 = 3
- 席位置（前後）(`position_depth`)
    - int(1)
        - default(指定なし) = 0
        - 前方 = 1
        - 中央 = 2
        - 後方 = 3
- 席位置（上下）(`position_rol`)
    - int(1)
        - default(指定なし) = 0
        - 上手 = 1
        - 中央 = 2
        - 下手 = 3
- 階(`floor`)
    - int(1)
        - default(指定なし) = 0
        - 1階席 = 1
        - 2階席 = 2
        - 3階席 = 3
        - 4階席 = 4
        - アリーナ = 5
        - その他 = 9
- 劇場(`theater`)
    - text
- 投稿日（`posted`）
    - datetime

### SQL文

`CREATE TABLE report_table (report_id varchar(20) PRIMARY KEY UNIQUE, title text, report_text text, visibiliry int(1), netabare int(1), day datetime, type int(1), position_depth int(1), posistion_rol int(1), floor int(1), theater text, posted datetime);`

## タグ(table:`tag`)

- レポートid(`id`)
    - 主キー
    - varchar(20)
- 登録タグ(`tag`)
    - varchar(20)

### SQL文

`CREATE TABLE tag (report_id varchar(20) PRIMARY KEY UNIQUE, tag varchar(20));`

## ブックマーク(table:`bookmark`)

- ユーザーid(`id`)
    - 主キー
    - varchar(20)
- ブックマーク済みレポートid(`report_id`)
    - varchar(20)
- リアクション種別(`reaction`)
    - int(1)
        - default（指定なし） = 0
        - いいね = 1
        - わかりみ = 2

### SQL文

`CREATE TABLE bookmark (user_id varchar(20) PRIMARY KEY, report_id varchar(20), reaction int(1));`
