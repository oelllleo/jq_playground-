# Intro 

JQ is a filter: 
1. it takes ***input, 
2. processs and 
3. output 

Foundamental concepts
1. How jq works 
2. Basic filters
3. Object identifier-index and Optional Object Identifier-index
4. Object Index and Array Index
5. Array/String Slice and Array/Object Value Iterator
6. Comma, Pipe and parenthesis 


## How jq wroks
jq filters run on <mark1>a stream of JSON data.</mark>

The input to jq is parsed as ***a sequence of whitespace-separated JSON values*** which are passed through the provided filter one at a time

The output(s) of the filter are ***written to standard output, as a sequence of newline-separated JSON data.***

## Basic filters

### Identity: <mark>***.***</mark>

The simplest and most common filter (or jq program) is <mark>.</mark>, which is the ***identity operator***, <mark>copying the inputs of the jq processor to the output stream. </mark>

Because the default behavior of the jq processor is to read JSON texts from the input stream, and to pretty-print outputs, the . program's main use is to validate and pretty-print the inputs.


but sometime the jq will losing precision 

```
1E1234567890 | . 

(return 1.7976931348623157e+308  jq has converted it to an IEEE754 double-precision representation, losing precision. on parsing this number)
```

the parsing procedures:

(1) Any arithmetic operation on a number that has not already been converted to an IEEE754 double precision representation will trigger a conversion to the IEEE754 representation.

(2) jq will attempt to maintain the original decimal precision of number literals, but in expressions such 1E1234567890, precision will be lost if the exponent is too large.

(3) In jq programs, a leading minus sign will trigger the conversion of the number to an IEEE754 representation.

(4) Comparisons are carried out using the untruncated big decimal representation of numbers if available, as illustrated in one of the following examples.

#### Identity example 

```
 echo '"Hello, world!"' | jq '.'
```

## Object Identifier-index


```
jq '.key.subkey.subsubley'
```

for example:

```
echo '{"foo": 42, "bar": "less interesting data"}' | jq '.foo'
```
The above example:
input: {"foo": 42, "bar": "less interesting data"}
process: '.foo' = get the 'foo' key value 
output: 42

A filter of the form .foo.bar is equivalent to .foo | .bar.

can consider the process is a stage pipeline 

If the key contains special characters or starts with a digit, you need to surround it with double quotes like this: ."foo$", or else .["foo$"].

## Optional Object Identifier-index

Just like .foo, but does not output an error when . is not an object.

```
echo '{"foo": 42, "bar": "less interesting data"}'| jq '.foo?'
```

The above example 

Input: {"foo": 42, "bar": "less interesting data"}
process: '.foo?' = foo is an object ?
output: 42


## Object Index: <mark>.[<String>]</mark>
You can also look up fields of an object using syntax like .["foo"] (.foo above is a shorthand version of this, but only for identifier-like strings).

## Array Index: <mark>.[<number>]</mark>
When the index value is an integer, .[<number>] can index arrays. Arrays are zero-based, so .[2] returns the third element.
Negative indices are allowed, with -1 referring to the last element, -2 referring to the next to last element, and so on

### Example 1
```
  echo '[{"name":"JSON", "good":true}, {"name":"XML", "good":false}]' | jq '.[0]'
```

The above example:
Input : [{"name":"JSON", "good":true}, {"name":"XML", "good":false}]
Process: 1. get root . 
         2. get root's object index[0]'s object 
Output: {"name":"JSON", "good":true}

### Example 2
```
  echo '[1,2,3]' | jq '.[-2]'
```
The above example:
Input : [1,2,3]
Process: 1. get root . 
         2. get root's object count from last 2 
Output: 2

## Array/Object Value Iterator: .[] 

If you use the .[index] syntax, but omit the index entirely, it will return all of the elements of an array. Running .[] with the input [1,2,3] will produce the numbers as three separate results, rather than as a single array. A filter of the form .foo[] is equivalent to .foo | .[].

```
  echo '[{"name":"JSON", "good":true}, {"name":"XML", "good":false}]' | jq '.[]'
```
The above example:
Input : [{"name":"JSON", "good":true}, {"name":"XML", "good":false}]
Process: 1. get root . 
         2. get all objects one by one 
Output: {"name":"JSON", "good":true}
        {"name":"XML", "good":false}



## Array/String Slice: .[<number>:<number>] 

The .[<number>:<number>] syntax can be used to return a subarray of an array or substring of a string. The array returned by .[10:15] will be of length 5, containing the elements from index 10 (inclusive) to index 15 (exclusive). Either index may be negative (in which case it counts backwards from the end of the array), or omitted (in which case it refers to the start or end of the array). Indices are zero-based.

### example 1
```
  echo '["a","b","c","d","e"]' | jq '.[2:4]'
```

The above example:
Input : ["a","b","c","d","e"]
Process: 1. get root . 
         2. get root's objects from 2 to 3 （* 4 exclusive）
Output: ["c","d"]

## Comma, Pipe and Parenthesis

### Comma: <mark>,</mark>
If two filters are separated by a comma, then the same input will be fed into both and the two filters' output value streams will be concatenated in order: 
1. first, all of the outputs produced by the left expression,
2. and then all of the outputs produced by the right. 

For instance, filter ***.foo, .bar***, produces both the "foo" fields and "bar" fields as <mark>separate outputs.</mark>

The , operator is one way to construct generators.

#### Example

```
echo '{"user":"stedolan", "projects": ["jq", "wikiflow"]}' | jq '.user, .project[]'
```
The above example:

Input : 

{"user":"stedolan", "projects": ["jq", "wikiflow"]}

Process: has 1 comma  so there is 2 stream

Stream 1 (.user)
1. get roots
2. get roots' user value

Stream 2 (.project[]) 

1. get roots
2. get roots; project value
3. get all from it 

Output: 

"stedolan" <- from Stream 1
"jq" <- from Stream 2
"wikiflow"

### Pipe: <mark> | </mark>
The | operator combines two filters by feeding the output(s) of the one on the left into the input of the one on the right. It's similar to the Unix shell's pipe, if you're used to that.

If the one on the left produces multiple results, the one on the right will be run for each of those results. So, the expression .[] | .foo retrieves the "foo" field of each element of the input array. This is a cartesian product, which can be surprising.

#### Example 

```
echo '[{"name":"JSON", "good":true}, {"name":"XML", "good":false}]' | jq '.[] | .name'
```
The above example:

Input : [{"name":"JSON", "good":true}, {"name":"XML", "good":false}]

Process: has 1 pipes means have 2 stage 

stage 1 (.[])
1. get root
2. get root all objects

stage 2 (.name) input .[] all object one by one 
1. get value of key with name 

Output: "JSON"
        "XML"

### Parenthesis <mark>()</mark>
Parenthesis work as a grouping operator just as in any typical programming language.

```
echo '1'|jq '(. + 2) * 5'
```

the above example:

Input : 1

process: 
1. get root object (1)
2. (1+2)
3. multiple 5

output :
15
