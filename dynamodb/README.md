# DynamoDB の設計

今回のシステムでは以下のような構造を持つ DynamoDB のテーブルを基本的に使用する。

これのテーブルだけでは機能を満たせない(Local Secondary Index が必要など)場合は新たにテーブルを追加する。

```json
{
  "AttributeDefinitions": [
    {
      "AttributeName": "o",
      "AttributeType": "S"
    },
    {
      "AttributeName": "s",
      "AttributeType": "S"
    }
  ],
  "KeySchema": [
    {
      "AttributeName": "o",
      "KeyType": "HASH"
    },
    {
      "AttributeName": "s",
      "KeyType": "RANGE"
    }
  ]
}
```

テーブルを使用する場合はデータの構造や Query の利便性を考慮し、パーティションキーにプレフィックスを付ける。

| プレフィックス | 構造の用途                           |
| -------------- | ------------------------------------ |
| sc:            | 楽譜のデータ管理に使用する           |
| si:            | 楽譜のアイテムデータの管理に使用する |


## 楽譜のデータ管理

### サマリーデータ

```typescript

interface ScoreSummary{
    /** "sc:" + owner id */
    o: string;
    /** 固定値 */
    s: "summary";
    /** 楽譜の保存数 */
    c: number;
    /** 作成日時 */
    ca: number;
    /** 更新日時 */
    ua: number;
}

```

### メインデータ 

```typescript

interface ScoreMain{
    /** "sc:" + owner id */
    o: string;
    /** score id */
    s: string;
    /** 作成日時 */
    ca: number;
    /** 更新日時 */
    ua: number;
    /** アクセスについて */
    as: "pr" | "pu";
    /** リソースの特定バージョンの識別子(変更があった場合に変更される値) */
    e: string;
    /** トランザクションスタート Unix ミリ秒 */
    xs: number;
    /** トランザクションタイムアウト Unix ミリ秒 */
    xt: number;
    /** データ構造のバージョン */
    v: number;
    /** スナップショットの数 */
    nc: number;
    n: {
        /** スナップショットの ID */
        i: string;
        /** スナップショットの名前 */
        n: string;
        /** 作成日時 */
        ca: number;
    }[];
    /** データ */
    d: {
        /** タイトル */
        t: string;
        /** 説明 */
        d: string;
        /** ページ数 */
        pc: number;
        /** 次のページの id */
        pi: number;
        /** ページデータ */
        p: {
            /** ページの id */
            i: number;
            /** アイテムの id */
            t: string;
            /** アイテムオブジェクトの種類 (p: png, j: jpeg) */
            k: "p" | "j";
            /** ページの名前 */
            p: string;
        }[];
        /** 次のアノテーション の id */
        ai: number;
        /** アノテーションの数 */
        ac: number;
        /** アノテーションデータ */
        a: {
            /** アノテーションの id */
            i: number;
            /** アノテーションデータとの関連 id */
            r: number;
            /** アノテーションの文字列の長さ */
            l: number;
        }[];
    };
}

```

### アノテーションデータ

```typescript

interface ScoreAnnotation{
    /** "sc:" + owner id */
    o: string;
    /** score id + ":a:" + chunk 2桁の数字 + 拡張用の1桁の数字 */
    s: string;
    /** アノテーションデータ */
    a: {
        [id: string /** アノテーションデータとの関連 id 数字0埋め最大5桁 */]: string
    };
}

```


## 楽譜のアイテム

### サマリーデータ

```typescript

interface ItemSummary{
    /** "si:" + owner id */
    o: string;
    /** 固定値 summary */
    s: "summary";
    /** owner が所有しているアイテムの合計サイズ */
    t: number;
    /** owner が所有しているアイテムの数 */
    c: number;
    /** 作成日時 */
    ca: number;
    /** 更新日時 */
    ua: number;
}

```

### メインデータ

```typescript

interface ItemMain{
    /** プレフィックス si: + owner id */
    o: string;
    /** score id */
    s: string;
    /** 作成日時 */
    ca: number;
    /** 更新日時 */
    ua: number;
    /** トランザクションスタート Unix ミリ秒 */
    xs: number;
    /** トランザクションタイムアウト */
    xt: number;
    /** データ構造のバージョン */
    v: number;
    /** 楽譜に含まれるアイテムのトータルサイズ */
    t: number;
    /** アイテムの数 */
    c: number;
    /** item のリスト */
    i: {
        /** アイテムの ID */
        i: string;
        /** アイテムオブジェクトの種類 (p: png, j: jpeg) */
        k: "p" | "j";
        /** アイテム１つに含まれるデータのトータルサイズ */
        t: number;
        /** アイテムのオリジナル名 */
        n: string;
    }[];
}

```