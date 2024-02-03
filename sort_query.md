

### sort: 配列で渡す
```
GET goods_index/_search
{
  "sort": [
    {
      "expiration_date": {
        "order": "desc"
      }
    }
  ]
}
```

```
GET goods_index/_search
{
  "sort": [
    {
      "expiration_date": {
        "order": "asc"
      }
    }
  ]
}
```

### sort条件を複数指定する
```
GET goods_index/_search
{
  "sort": [
    {
      "expiration_date": {"order": "desc"},
      "price": {"order": "asc"}
    }
  ]
}
```

### _scoreに関するsortも行える 
```
GET goods_index/_search
{
  "sort": [
    {"price": {"order": "asc"}},
    "_score"
  ]
}
```