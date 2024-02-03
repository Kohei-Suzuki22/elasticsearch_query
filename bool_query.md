# ---------
GET hello_index/_search


# boolクエリ: 複数条件を指定できる文法

### must: and検索
### mustの値は配列で定義し、その中に複数の基本クエリを定義する。すべての条件にマッチしたドキュメントを取得する。
```
GET hello_index/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {"message": "Elasticsearch"}},
        {"match": {"user_name": "John"}}
      ]
    }
  }
}
```

### should: or検索
### shouldの値は配列で定義し、その中に複数の基本クエリを定義する。どれかひとつの条件にマッチしたドキュメントを取得する。
```
GET hello_index/_search
{
  "query": {
    "bool": {
      "should": [
        {"match": {"message": "Elasticsearch"}},
        {"match": {"user_name": "Takahashi"}}
      ]
    }
  }
}
```

### minimum_sould_match: 2  -> 条件のうち、2つ以上がマッチ指定れば検索される。
```
GET hello_index/_search
{
  "query": {
    "bool": {
      "should": [
        {"match": {"message": "Elastic"}},
        {"match": {"message": "world"}},
        {"match": {"user_name": "John"}},
        {"match": {"user_name": "Takahashi"}}
      ],
      "minimum_should_match": 2
    }
  }
}
```

### minimum_sould_match: 50%  -> 条件のうち、50%以上がマッチ指定れば検索される。
```
GET hello_index/_search
{
  "query": {
    "bool": {
      "should": [
        {"match": {"message": "Elastic"}},
        {"match": {"message": "world"}},
        {"match": {"user_name": "John"}},
        {"match": {"user_name": "Takahashi"}}
      ],
      "minimum_should_match": "50%"
    }
  }
}
```

### must_not: 条件にマッチしないドキュメントを取得する
```
GET hello_index/_search
{
  "query": {
    "bool": {
      "must_not": [
        {"term": {"city": "New York"}}
      ]
    }
  }
}
```

### filter: 対象外のドキュメントを「足切り」する。 RDBのwhere句と同等。
#### filterは、検索結果のスコアに関連しない。
#### 同じfilter条件を実施した場合は、キャッシュが使われるため非常に高速に実行できる。(性能向上)
```
GET goods_index/_search
{
  "query": {
    "bool": {
      "must": [
        {"match": {"name": "cider"}}
      ],
      "filter": [
        {"range": {"expiration_date": {"gte": "now-1w"}}}
      ]
    }
  }
}
```
