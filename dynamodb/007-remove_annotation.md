# ページを削除

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
        ua: 1641394805000,
        as: "pu",
        e: "EREREREREREREQAAAAAAAg",
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
            ac: 2,
            ai: 2,
            a: [
                {
                    i: 0,
                    r: 0,
                    l: 240
                },
                {
                    i: 1,
                    r: 1,
                    l: 480
                }
            ]
        }
    },
    // Annotation
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA:a:000",
        a:{
            "00000": "aaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccdddddddddd",
            "00001": "aaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccdddddddddd"
        }
    },
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA:a:001",
        a:{
            "00001": "aaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccdddddddddd"
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
| now           | 2022-01-07T00:00:00+09:00            | 1641481200000          |      |
| timeout       | 2022-01-07T00:00:10+09:00            | 1641481210000          |      |
| previous etag | 11111111-1111-1111-1111-000000000002 | EREREREREREREQAAAAAAAg |      |

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
        e: "EREREREREREREQAAAAAAAg",
        xs: 1641481200000,
        xt: 1641481210000,
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
            ac: 2,
            ai: 2,
            a: [
                {
                    i: 0,
                    r: 0,
                    l: 240
                },
                {
                    i: 1,
                    r: 1,
                    l: 480
                }
            ]
        }
    },
    // Annotation
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA:a:000",
        a:{
            "00000": "aaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccdddddddddd",
            "00001": "aaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccdddddddddd"
        }
    },
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA:a:001",
        a:{
            "00001": "aaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccdddddddddd"
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

## 2. アノテーションを削除する

- etag が同じか確認する
- アノテーションを削除する
- ScoreMain を書き換える
- ScoreMain の xs が変わっていないことを確認する
- ScoreMain の xt がきれていないことを確認する

### パラメータ

| パラメータ              | 値                                   | 変換後                 | 備考 |
| ----------------------- | ------------------------------------ | ---------------------- | ---- |
| transaction start       | 2022-01-07T00:00:00+09:00            | 1641481200000          |      |
| now                     | 2022-01-07T00:00:05+09:00            | 1641481205000          |      |
| previous etag           | 11111111-1111-1111-1111-000000000002 | EREREREREREREQAAAAAAAg |      |
| new etag                | 11111111-1111-1111-1111-000000000003 | EREREREREREREQAAAAAAAw |      |
| remove annotation index | 1                                    | -                      |      |

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
        ua: 1641481205000,
        as: "pu",
        e: "EREREREREREREQAAAAAAAw",
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
            ac: 1,
            ai: 2,
            a: [
                {
                    i: 0,
                    r: 0,
                    l: 240
                }
            ]
        }
    },
    // Annotation
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA:a:000",
        a:{
            "00000": "aaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccddddddddddeeeeeeeeeeaaaaaaaaaabbbbbbbbbbccccccccccdddddddddd",
        }
    }
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
