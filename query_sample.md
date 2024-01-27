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





























