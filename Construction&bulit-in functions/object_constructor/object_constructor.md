# object constructor

like array constructor jq also can add obeject constructor

## Basic

```
cat example.json | jq '.[].title,.[].number'
```
it returns 
```
"`reduce` and `foreach` with backtracking init have observable `DUPN`"
"Redundant subexpression in Generators and iterators example in doc"
"website: generate website from markdown docs"
"build: fix insecure RUNPATH"
"`reduce`/`foreach` behaviour when `UPDATE` yields not exactly one output"
3227
3222
3221
3212
3211
```

we want to wrap it, so ....
```
cat example.json | jq '[{"titles":.[].title,"number":.[].number}]'
```

but it is incorrect 

in fact jq is work like this 

```
jq '{"key1": <jq filter>,"key2" : <jq filter2>}'
```

so it not works in array 

```
cat example.json | jq '.[1]' | jq '{"title":.title, "number":.number}'

```
it works now 

also if do not change key name 
```
cat example.json | jq '.[1]' | jq '{title, number}'

```

for arrays 
```
cat example.json | jq '[.[]| {title,number}]'
```
```
cat example.json | jq '[.[]'| |jq '{title,number}]'
```

pipe is a short form for this


