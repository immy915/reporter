# データベース設計

- postgres 11.2
- 接続：`psql -U postgres -W`
- 切断：`\q`
- データベース名：reporter

## ユーザー情報(table:`user`)

- ユーザーid(`id`)
    - 主キー
    - ユニーク
    - varchar(20)
- 表示名(`name`)
    - varchar(20)
- パスワード(`pass`)
    - varchar(20)
- メールアドレス(`mail`)
    - ユニーク
    - varchar(100)
- 推し(`oshi`)
    - varchar(20)

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

## レポート情報(table:`reports`)

- レポートid(`id`)
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
    - date
- 時間(`type`)
    - int(1)
        - default(指定なし) = 0
        - 昼公演 = 1
        - 夜公演 = 2
        - その他 = 3
- 席位置（前後）(`position1`)
    - int(1)
        - default(指定なし) = 0
        - 前方 = 1
        - 中央 = 2
        - 後方 = 3
- 席位置（上下）(`position2`)
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

## タグ(table:`tag`)

- レポートid(`id`)
    - 主キー
    - varchar(20)
- 登録タグ(`tag`)
    - varchar(20)

## ブックマーク(table:`bookmark`)

- ユーザーid(`id`)
    - 主キー
    - varchar(20)
- ブックマーク済みレポートid(`bookmark`)
    - varchar(20)
