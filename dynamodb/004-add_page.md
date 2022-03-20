# ページを追加

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
- 応答でページのデータを取得する
- etag が同じか確認する

### パラメータ

| パラメータ    | 値                                   | 変換後                 | 備考 |
| ------------- | ------------------------------------ | ---------------------- | ---- |
| now           | 2022-01-05T00:00:00+09:00            | 1641308400000          |      |
| timeout       | 2022-01-05T00:00:10+09:00            | 1641308410000          |      |
| previous etag | 11111111-1111-1111-1111-000000000000 | EREREREREREREQAAAAAAAA |      |

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
        xs: 1641308400000,
        xt: 1641308410000,
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

## 2. ページを追加する

- etag が同じか確認する
- ページを追加する
- ScoreMain を書き換える
- ScoreMain の d.pc の最大値を確認する
- ScoreMain の xs が変わっていないことを確認する
- ScoreMain の xt がきれていないことを確認する
- pi から id を計算し設定する

### パラメータ

| パラメータ        | 値                                   | 変換後                 | 備考 |
| ----------------- | ------------------------------------ | ---------------------- | ---- |
| transaction start | 2022-01-05T00:00:00+09:00            | 1641308400000          |      |
| now               | 2022-01-05T00:00:05+09:00            | 1641308405000          |      |
| previous etag     | 11111111-1111-1111-1111-000000000000 | EREREREREREREQAAAAAAAA |      |
| new etag          | 11111111-1111-1111-1111-000000000001 | EREREREREREREQAAAAAAAQ |      |
| item id           | 00000000-0000-0000-2222-000000000001 | AAAAAAAAAAAiIgAAAAAAAQ |      |
| item kind         | PING                                 | p                      |      |
| page name         | "page1"                              | -                      |      |

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
        ua: 1641308405000,
        as: "pu",
        e: "EREREREREREREQAAAAAAAQ",
        xs: 0,
        xt: 0,
        v: 1,
        nc: 0,
        n: [],
        d: {
            t: "スコアのタイトル",
            d: "スコアの説明",
            pc: 1,
            pi: 1,
            p: [
                {
                    i: 0,
                    t: "AAAAAAAAAAAiIgAAAAAAAQ",
                    k: "p",
                    p: "page1"
                }
            ],
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
        xs: 1641308400000,
        xt: 1641308410000,
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
