#### 事前準備____________________

get hello_index/_mapping

POST hello_index/_mapping
{
  "properties": {
    "city": {
      "type": "keyword"
    }
  }
}



PUT hello_index/_doc/11
{
  "city": "Los Angels"
}


PUT hello_index/_doc/12
{
  "city": "New York"
}

PUT hello_index/_doc/13
{
  "city": "Oregon"
}


PUT hello_index/_doc/14
{
  "city": "Versinia"
}


PUT hello_index/_doc/15
{
  "city": "Calfolnia"
}

#### ______________

# term: keyword型に対する完全一致検索をする際に使用する。
#### ※ 全文検索のように、含まれているからといってマッチするのではなく、そのfieldに対する完全一致をする。
##### ※ text型にはtermを使用してもマッチしないらしい。
```
GET hello_index/_search
{
  "query": {
    "term": {
      "city": {
        "value": "Calfolnia"
      }
    }
  }
}
```

## 上の省略形
```
GET hello_index/_search
{
  "query": {
    "term": {
      "city": "Los Angels"
    }
  }
}
```


# terms: 配列で複数マッチさせたい値を指定し、指定したもののうちどれか一つでもマッチすれば検索に引っかかる。
```
GET hello_index/_search
{
  "query": {
    "terms": {
      "city": ["Los Angels", "New York"]
    }
  }
}
```