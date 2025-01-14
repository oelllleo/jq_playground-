# Sorting and Counting


## sort
```
echo '[3,2,1]' | jq 'sort'
```

returns
```
[
  1,
  2,
  3
]
```

## Reverse

```
echo '["3","2","A"]' | jq 'reverse'
```

returns
```
[
  "A",
  "2",
  "3"
]
```


## length


```
echo '["3","2","B","A"]' | jq 'length'
```

it returns

```
4
```