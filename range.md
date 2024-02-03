#### 事前準備----------------

PUT goods_index

GET goods_index/_search

PUT goods_index/_doc/1
{
  "name": "milk",
  "price": 100,
  "expiration_date": "2024-02-24"
}

PUT goods_index/_doc/2
{
  "name": "coffee",
  "price": 120,
  "expiration_date": "2024-02-17"
}

PUT goods_index/_doc/3
{
  "name": "cider",
  "price": 150,
  "expiration_date": "2024-02-10"
}
#### ----------------

# rangeクエリ: 数値や日付の比較に使う。

### gte: greater than equal (以上)
### lte: less than equal    (以下)
### gt:  greater than       (より大きい)
### lt:  less than          (より小さい)
```
GET goods_index/_search 
{
  "query": {
    "range": {
      "price": {
        "gte": 100,
        "lte": 120
      }
    }
  }
}
```

# 日付の比較。
```
GET goods_index/_search
{
  "query": {
    "range": {
      "expiration_date": {
        "gte": "2024-02-10",
        "lte": "2024-02-20"
      }
    }
  }
}
```

```
GET goods_index/_search
{
  "query": {
    "range": {
      "expiration_date": {
        "gte": "now"
      }
    }
  }
}
```

```
GET goods_index/_search
{
  "query": {
    "range": {
      "expiration_date": {
        "lte": "now-1w"
      }
    }
  }
}
```