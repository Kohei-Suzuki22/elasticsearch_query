#### クラスターのヘルスチェック

```
GET /_cluster/health 
```

#### nodeの状態を確認

```
GET /_cat/nodes?v 
```

#### 存在するindexを確認

```
GET /_cat/indices?v
```

#### システムインデックスも含めて取得。

```
GET /_cat/indices?v&expand_wildcards=all
```

#### indexの作成

```
PUT /pages
```

#### シャードの状態を確認する

```
GET /_cat/shards?v
```

#### ================================

## ドキュメント操作

#### ドキュメントの登録。

- PUTリクエストの場合ドキュメントIDを指定する。
- 事前にインデックスが作られていない場合は、自動作成してくれる。

```
PUT my_index/_doc/1
{
  "user_name": "John Smith",
  "date": "2019-10-06T15:09:45",
  "message": "Hello Elasticsearch world."
}
```

#### ドキュメントの登録。

- POSTリクエストの場合はドキュメントIDを指定しない。（自動採番される)
- 事前にインデックスが作られていない場合は、自動作成してくれる。

```
POST hello_index/_doc
{
  "user_name": "John Smith",
  "date": "2019-10-06T15:09:45",
  "message": "Hello Elasticsearch world."
}
```

```
POST hello_index/_doc
{
  "user_name": "John Smith",
  "date": "2019-10-06T15:09:45",
  "message": "Hello Elasticsearch world.",
  "age": 20
}
```


#### ドキュメントの取得

- メタデータも取得される。

```
GET my_index/_doc/1
```

#### ドキュメントの取得

- ユーザーが登録した実データだけ。

```
GET my_index/_source/1
```


#### like検索

- messageフィールドに"Elasticsearch"という文言が入っているドキュメントを取得。

```
GET my_index/_search
{
  "query": {
    "match": {
      "message": "Elasticsearch"
    }
  }
}
```

- my_indexと_hello_indexから検索

```
GET my_index,hello_index/_search
{
  "query": {
    "match": {
      "message": "Elasticsearch"
    }
  }
}
```


- インデックスの指定をワイルドカードでできる。

```
GET *_index/_search
{
  "query": {
    "match": {
      "message": "Elasticsearch"
    }
  }
}
```

-  すべてのインデックスから検索する

```
GET /_search
{
  "query": {
    "match": {
      "message": "Elasticsearch"
    }
  }
}
```

#### ドキュメント全体のupdate

- bodyに指定したkey, value登録する。
- 更新する際のvalueは、前の型のものしかできない。 ex: 更新前の値が500(int) の場合は、"500"(str)に変換できない。

```
PUT /my_index/_doc/1
{
  "hello": 1,
  "hi": 2,
  "foo": 3
}
```

#### ドキュメントの一部フィールドの更新。

- 更新する際のvalueは、前の型のものしかできない。 ex: 更新前の値が500(int) の場合は、"500"(str)に変換できない。

```
POST /my_index/_update/1
{
  "doc": {
    "hello": 5000
  }
}
```

#### ドキュメントの削除

```
DELETE /hello_index/_doc/1
```

#### ドキュメントがない場合はerror

```
GET /hello_index/_source/1
```


#### indexの作成

```
PUT simple_default_setting_index
```

#### indexの作成とindexの設定

```
PUT sample_my_index
{
  "settings": {
    "number_of_shards": "3",
    "number_of_replicas": "1"
  }
}
```

#### indexの設定情報を取得

```
GET sample_my_index/_settings
```

#### indexの設定情報を更新

- ※ プライマリシャードの数は変更できない。

```
PUT sample_my_index/_settings
{
  "index": {
    "number_of_replicas": "4"
  }
}
```

#### indexの削除

```
DELETE simple_default_setting_index
```


#### クラスターの設定を変更。(設定ファイル. elasticsearch.ymlを変更するのと同義)

- persistentを指定した場合は、永続的な設定となる。

```
PUT _cluster/settings
{
  "persistent": {
    "action.destructive_requires_name": true
  }
}
```

-  transientを指定した場合は、クラスターが再起動した場合に元の設定に戻る。

```
PUT _cluster/settings
{
  "transient": {
    "action.destructive_requires_name": true
  }
}
```

#### mappingの定義を作成

```
PUT my_index/_mapping
{
  "properties": {
    "user_name": {
      "type": "text"
    },
    "date": {
      "type": "date"
    },
    "message": {
      "type": "text"
    }
    
  }
}
```

#### mappingの定義をしても、fieldはそのドキュメントによってフィールドを追加したり、減らしたりできる。

```
PUT my_index/_doc/2
{
  "user_name": "hi",
  "date": "2015-01-01",
  "message": "hello",
  "age": 19,
  "yeah": "bar"
}
```

#### 一度mappingを定義しても、定義を追加をすることはできる。

- ※ fieldに設定した定義を後から変更することはできない

```
PUT my_index/_mapping
{
  "properties": {
    "additional_comment": {
      "type": "text"
    }
  }
}
```

#### mappingの定義情報を取得

```
GET my_index/_mapping
```



#### indexテンプレートを作成

- "accesslog-"で始まる名前のインデックスが作られる際にこのテンプレートが適用される
```
PUT _template/my_template
{
  "index_patterns": "accesslog-*",
  "settings": {
    "number_of_shards": 1
  },
  "mappings": {
    "properties": {
      "host": {
        "type": "keyword"
      },
      "uri": {
        "type": "keyword"
      },
      "method": {
        "type": "keyword"
      },
      "accesstime": {
        "type": "date"
      }
    }
  }
}
```

```
POST accesslog-hello/_doc
{
  "host": "web01",
  "uri": "/user/2024",
  "method": "PUT",
  "accesstime": "2024-01-28T10:10:10"
}
```

#### mapping情報を取得

- (indexテンプレートが適用されているのが確認できる。)

```
GET accesslog-hello/_mapping
GET accesslog-hello/_doc/fCkWTI0B4EZSiz7nThO1
```


####  indexテンプレートの設定情報を取得

```
GET _template/my_template
```

#### 複数のindexテンプレートを作成

- 複数のテンプレートに当てはまるindexは、orderの小さいテンプレートから適用される。
- 最終的に、最後に適用されたテンプレートの値で上書きされる.
- orderの小さい方のテンプレートで設定されていて、orderの大きい方では設定されていない項目は、小さい方の値がそのまま設定される。

```
PUT _template/my_default_template
{
  "index_patterns": "*",
  "order": 1,
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 3
  }
}

PUT _template/my_tweet_template
{
  "index_patterns": "my_index-*",
  "order": 10,
  "settings": {
    "number_of_shards": 1
  },
  "mappings": {
    "properties": {
      "user_name": {
        "type": "text"
      },
      "date": {
        "type": "date"
      },
      "message": {
        "type": "text"
      }
    }
  }
}
```

#### 上で定義した複数のテンプレートが当てはまるindex名で作成
```
PUT /my_index-hello/_doc/1
{
  "user_name": "name",
  "message": "hello",
  "age": 19
}
```

#### テンプレートの削除

```
DELETE _template/my_template
```

#### テンプレート一覧の取得

```
GET _template/?pretty
```

#### 動的mapping定義

- dynamic_templatesの中は、任意の数の配列
- price_of_floatという名前の定義を作成
- price_* にマッチする かつ *_createdにマッチしないfieldを作成したバイは、float型で作成する。

```
PUT my_index/_mapping
{
  "properties": {
    "item": {
      "type": "text"
    }
  },
  "dynamic_templates": [
    {
      "price_of_float": {
        "match": "price_*",
        "unmatch": "*_created",
        "mapping": {
          "type": "float"
        }
      }
    }
  ]
}
```

-  price_book: 上記で定義した動的mapping定義に該当するため、float型で定義される
-  price_book_created: 上記で定義した動的mapping定義に該当しないので、Elasticsearchの自動型推論によって決まる。

```
POST my_index/_doc
{
  "item": "book",
  "price_book": 500,
  "price_book_created": "2019-10-06"
}
```

#### 上記確認

```
GET my_index/_doc/XCmYTY0B4EZSiz7nVRY2
GET my_index/_mapping
```

- 自動型推論で、longにされるようなfieldは、integer型で定義される

```
PUT my_index/_mapping
{
  "dynamic_templates": [
    {
      "long_to_integer": {
        "match_mapping_type": "long",
        "mapping": {
          "type": "integer"
        }
    }
    }
  ]
}
```

- 上記確認

```
POST my_index/_doc
{
  "price": 500
}

GET my_index/_mapping
```


