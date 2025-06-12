---
title: "Using Go's json.Number to Handle Quoted and Unquoted Numbers in JSON"
date: 2025-06-12T07:38:53-04:00
url: /2025/06/use-go-json-number
---

When working with JSON in Go, it's common to encounter numeric values represented both as numbers and as strings. This inconsistency can lead to issues during unmarshaling, as Go's `encoding/json` package expects a consistent type. Fortunately, Go provides the `json.Number` type to handle such scenarios gracefully.

## The Challenge
Consider the following JSON snippets:

``` json
{"value": 123}
```

``` json
{"value": "123"}
```
In the first example, value is a number, while in the second, it's a string. If you define your Go struct as:

``` go
type Data struct {
    Value float64 `json:"value"`
}
```

Unmarshaling the first JSON will succeed, but the second will fail with an error:

```
json: cannot unmarshal string into Go struct field Data.value of type float64
```
Alternatively, using the `string` tag will allow the second JSON to unmarshal successfully, but the first will now fail

## The solution: `json.Number`

Go's `encoding/json` package provides the `json.Number` type, which is a string representation of a JSON number. It implements the `UnmarshalJSON` interface and can handle both quoted and unquoted numbers.

By defining your struct as:
``` go
type Data struct {
    Value json.Number `json:"value"`
}
```

you can unmarshal both JSON examples without errors. You can then convert `json.Number` to the desired numeric type using its methods

```go
// If you want to convert to a float: 
num, err := data.Value.Float64()

// If you want to convert to an int64:
num, err := data.Value.Int64()
```

This approach provides flexibility in handling numeric values that may come as strings or numbers.

Handling inconsistent numeric representations in JSON can be challenging, but Go's json.Number type offers a robust solution. By leveraging it or implementing custom unmarshaling, you can ensure your applications handle JSON data gracefully, regardless of how numbers are represented.

For more information, refer to the [official Go documentation on `encoding/json`](https://pkg.go.dev/encoding/json).


