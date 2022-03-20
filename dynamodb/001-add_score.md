# スコアの追加

## 初期値

### DB の状態

```typescript
[
    // ScoreSummary
    {
        o: "sc:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        c: 0,
        ca: 1640962800000,
        ua: 1640962800000
    },
    // ItemSummary
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "summary",
        t: 0,
        c: 0,
        ca: 1640962800000,
        ua: 1640962800000
    }
]
```

--------------------------------------------------------------------------------

## 1. スコアデータの追加

- TransactWrriteItems で ScoreSummary と ScoreMain 、 ItemMain を書き換える
- ScoreSummary は c のマックス値を確認する

### パラメータ

| パラメータ  | 値                                   | 変換後                 | 備考 |
| ----------- | ------------------------------------ | ---------------------- | ---- |
| ownerId     | 00000000-0000-0000-0000-000000000000 | AAAAAAAAAAAAAAAAAAAAAA |      |
| scoreId     | 00000000-0000-0000-1111-000000000000 | AAAAAAAAAAAREQAAAAAAAA |      |
| title       | "スコアのタイトル"                   | -                      |      |
| description | "スコアの説明"                       | -                      |      |
| new etag    | 11111111-1111-1111-1111-000000000000 | EREREREREREREQAAAAAAAA |      |
| now         | 2022-01-02T00:00:00+09:00            | 1641049200000          |      |

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
        t: 0,
        c: 0,
        ca: 1641049200000,
        ua: 1641049200000
    },
    // ItemMain
    {
        o: "si:AAAAAAAAAAAAAAAAAAAAAA",
        s: "AAAAAAAAAAAREQAAAAAAAA",
        ca: 1641049200000,
        ua: 1641049200000,
        xs: 0,
        xt: 0,
        v: 1,
        t: 0,
        c: 0,
        i: []
    }
]

```