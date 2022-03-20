# アイテムの追加

## 初期値

### DB の状態

```typescript
[
    // ScoreSummary
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        c: 1,
        ca: 1640962800000,
        ua: 1641049200000
    },
    // ScoreMain
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA",
        ca: 1641049200000,
        ua: 1641049200000,
        as: "pu",
        e: "EREREREREREREQAAAAAAAA",
        xs: 0,
        xt: 0,
        v: 1,
        nc: 0,
        n: [],
        d: {
            t: "スコアのタイトル",
            d: "スコアの説明",
            pc: 0,
            p: [],
            ac: 0,
            a: []
        }
    },
    // ItemSummary
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        t: 2621440,
        c: 2,
        ca: 1641049200000,
        ua: 1641135605000
    },
    // ItemMain
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA",
        ca: 1641049200000,
        ua: 1641135605000,
        xs: 0,
        xt: 0,
        v: 1,
        t: 2621440,
        c: 2,
        i: [
            {
                i: "AAAAAAAAAAAiIgAAAAAAAQ",
                k: "p",
                t: 1048576,
                n: "画像01.png"
            },
            {
                i: "AAAAAAAAAAAiIgAAAAAAAg",
                k: "j",
                t: 1572864,
                n: "画像02.jpg"
            }
        ]
    }
]
```

--------------------------------------------------------------------------------

## 1. トランザクションを設定する

- トランザクションに時間を設定する
- トランザクションのタイムアウトを現在時間の10秒後に設定する

### パラメータ

| パラメータ | 値                        | 変換後        | 備考 |
| ---------- | ------------------------- | ------------- | ---- |
| now        | 2022-01-04T00:00:00+09:00 | 1641222000000 |      |
| timeout    | 2022-01-04T00:00:10+09:00 | 1641222010000 |      |

### DB の状態

```typescript
[
    // ScoreSummary
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        c: 1,
        ca: 1640962800000,
        ua: 1641049200000
    },
    // ScoreMain
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA",
        ca: 1641049200000,
        ua: 1641049200000,
        as: "pu",
        e: "EREREREREREREQAAAAAAAA",
        xs: 0,
        xt: 0,
        v: 1,
        nc: 0,
        n: [],
        d: {
            t: "スコアのタイトル",
            d: "スコアの説明",
            pc: 0,
            p: [],
            ac: 0,
            a: []
        }
    },
    // ItemSummary
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        t: 2621440,
        c: 2,
        ca: 1641049200000,
        ua: 1641135605000
    },
    // ItemMain
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA",
        ca: 1641049200000,
        ua: 1641135605000,
        xs: 1641222000000,
        xt: 1641222010000,
        v: 1,
        t: 2621440,
        c: 2,
        i: [
            {
                i: "AAAAAAAAAAAiIgAAAAAAAQ",
                k: "p",
                t: 1048576,
                n: "画像01.png"
            },
            {
                i: "AAAAAAAAAAAiIgAAAAAAAg",
                k: "j",
                t: 1572864,
                n: "画像02.jpg"
            }
        ]
    }
]
```

--------------------------------------------------------------------------------

## 2. アイテムを DB から削除する

- TransactWrriteItems で ItemSummary と ItemMain を書き換える
- ItemMain の xs が変わっていないことを確認する
- ItemMain の xt がきれていないことを確認する

### パラメータ

| パラメータ        | 値                                   | 変換後                 | 備考                          |
| ----------------- | ------------------------------------ | ---------------------- | ----------------------------- |
| transaction start | 2022-01-04T00:00:00+09:00            | 1641222000000          | xs が変更していないことを確認 |
| now               | 2022-01-04T00:00:05+09:00            | 1641222005000          |                               |
| remove item id    | 00000000-0000-0000-2222-000000000001 | AAAAAAAAAAAiIgAAAAAAAQ |                               |

### DB の状態

```typescript
[
    // ScoreSummary
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        c: 1,
        ca: 1640962800000,
        ua: 1641049200000
    },
    // ScoreMain
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA",
        ca: 1641049200000,
        ua: 1641049200000,
        as: "pu",
        e: "EREREREREREREQAAAAAAAA",
        xs: 0,
        xt: 0,
        v: 1,
        nc: 0,
        n: [],
        d: {
            t: "スコアのタイトル",
            d: "スコアの説明",
            pc: 0,
            p: [],
            ac: 0,
            a: []
        }
    },
    // ItemSummary
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        t: 1572864,
        c: 1,
        ca: 1641049200000,
        ua: 1641222005000
    },
    // ItemMain
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA",
        ca: 1641049200000,
        ua: 1641222005000,
        xs: 0,
        xt: 0,
        v: 1,
        t: 1572864,
        c: 1,
        i: [
            {
                i: "AAAAAAAAAAAiIgAAAAAAAg",
                k: "j",
                t: 1572864,
                n: "画像02.jpg"
            }
        ]
    }
]
```

--------------------------------------------------------------------------------

## 3. アイテムを S3 から削除する

- DynamoDB から削除したアイテムを削除する
- もしここで失敗してもリクエスト自体は成功とする
- 失敗したアイテムはログなどどこかに記録しておく
    - とりあえずは Lambda のログに残しておく
    - あとでガーベジコレクトするために
