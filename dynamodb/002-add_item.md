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
            pi: 0,
            p: [],
            ac: 0,
            ai: 0,
            a: []
        }
    },
    // ItemSummary
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        t: 0,
        c: 0,
        ca: 1641049200000,
        ua: 1641049200000
    },
    // ItemMain
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA",
        xs: 0,
        xt: 0,
        v: 1,
        t: 0,
        c: 0,
        i: []
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
| now        | 2022-01-03T00:00:00+09:00 | 1641135600000 |      |
| timeout    | 2022-01-03T00:00:10+09:00 | 1641135610000 |      |

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
            pi: 0,
            p: [],
            ac: 0,
            ai: 0,
            a: []
        }
    },
    // ItemSummary
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        t: 0,
        c: 0,
        ca: 1641049200000,
        ua: 1641049200000
    },
    // ItemMain
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA",
        xs: 1641135600000,
        xt: 1641135610000,
        v: 1,
        t: 0,
        c: 0,
        i: []
    }
]
```

--------------------------------------------------------------------------------

## 2. アイテムを S3 に追加する

- アップロードされた画像などのアイテムを S3 に保存する
- アイテムの ID を発行する

--------------------------------------------------------------------------------

## 3. アイテムを DB に書き込む

- TransactWrriteItems で ItemSummary と ItemMain を書き換える
- ItemSummary は t のマックス値を確認する
- ItemMain の xs が変わっていないことを確認する
- ItemMain の xt がきれていないことを確認する


### パラメータ

| パラメータ            | 値                                   | 変換後                 | 備考                          |
| --------------------- | ------------------------------------ | ---------------------- | ----------------------------- |
| transaction start     | 2022-01-03T00:00:00+09:00            | 1641135600000          | xs が変更していないことを確認 |
| now                   | 2022-01-03T00:00:05+09:00            | 1641135605000          |                               |
| item id 01            | 00000000-0000-0000-2222-000000000001 | AAAAAAAAAAAiIgAAAAAAAQ |                               |
| item size 01          | 1M                                   | 1048576                |                               |
| item kind 01          | PING                                 | p                      |                               |
| item original name 01 | 画像01.png                           | -                      |                               |
| item id 02            | 00000000-0000-0000-2222-000000000002 | AAAAAAAAAAAiIgAAAAAAAg |                               |
| item size 02          | 1.5M                                 | 1572864                |                               |
| item kind 02          | JPEG                                 | j                      |                               |
| item original name 02 | 画像02.jpg                           | -                      |                               |

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
            pi: 0,
            p: [],
            ac: 0,
            ai: 0,
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