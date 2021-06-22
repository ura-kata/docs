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

| プレフィックス | 構造の用途                 |
| -------------- | -------------------------- |
| sc:            | 楽譜のデータ管理に使用する |


## 楽譜のデータ管理

```typescript

interface Summary{
    /** プレフィックス + owner id */
    o: "sc:Id";
    /** 固定値 */
    s: "summary";    
    /** 楽譜の保存数 */
    scoreCount: number;
}

interface Main{
    /** プレフィックス + owner id */
    o: "sc:Id";
    /** score id */
    s: "Id";
    /** 作成日時 */
    ca: "UnixMS";
    /** 更新日時 */
    ua: "UnixMS";
    /** アクセスについて */
    as: "pr" | "pu";
    /** スナップショットの数 */
    sc: number;
    s: {
        /** スナップショットの ID */
        i: string;
        /** スナップショットの名前 */
        n: string;
        /** 作成日時 */
        ca: number;
    }[];
    /** データ構造のバージョン */
    v: string;
    /** データ */
    d: {
        /** タイトル */
        t: string;
        /** 説明 */
        d: string;
        /** ページ数 */
        pc: number;
        /** ページデータ */
        p: {
            /** ページの id */
            i: number;
            /** アイテムの id */
            t: "Id";
            /** オブジェクトの種類 (p: png, j: jpeg) */
            k: "p" | "j";
            /** ページの名前 */
            p: string;
        }[];
        /** アノテーションの数 */
        ac: number;
        /** アノテーションデータ */
        a: {
            /** アノテーションの id */
            i: number;
            /** アノテーション */
            a: string;
            /** チャンクフラグ */
            c: boolean
        }[];
    };
}

interface Extension{
    /** プレフィックス + owner id */
    o: "sc:Id";
    /** score id + 4桁の数字 */
    s: "Id0000";
    /** データ */
    d: {
        /** ページデータ */
        p: {
            /** ページの id */
            i: number;
            /** アイテムの id */
            t: "Id";
            /** オブジェクトの種類 (p: png, j: jpeg) */
            k: "p" | "j";
            /** ページの名前 */
            p: string;
        }[];
        /** アノテーションデータ */
        a: {
            /** アノテーションの id */
            i: number;
            /** アノテーション */
            a: string;
            /** チャンクフラグ */
            c: boolean
        }[];
    };

}
```


## アノテーションの追加

アノテーションの更新処理で必要なパラメータ

- 新しいアノテーションデータ
- 更新日時
- 現在のアノテーション数

現在のアノテーション数からソートキーを推測する

