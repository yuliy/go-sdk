# Official Go client for [Manticore Search](https://www.manticoresearch.com).

## Compatibility
The client is compatible with Manticore Search 2.8.2 and higher for majority of the commands.
It also may be used in many cases to access SphinxSearch daemon as well. However it's not guaranteed.

## Requirements
Go version 1.9 or higher

## Installation
```
go get github.com/manticoresoftware/go-sdk/manticore
```

## Usage
Here's a short example on how the client can be used:
Make sure there's some running Manticore instance, you can use our [docker image](https://hub.docker.com/r/manticoresearch/manticore) for a quick test:
```
docker run --name manticore -p 9313:9312 -p9306:9306 -d manticoresearch/manticore
```

Here's just a simplest script:
```
[root@srv ~]# cat manticore.go
package main

import "github.com/manticoresoftware/go-sdk/manticore"
import "fmt"

func main() {
	cl := manticore.NewClient()
	cl.SetServer("127.0.0.1", 9313)
	cl.Open()
	res, err := cl.Sphinxql(`replace into testrt values(1,'my subject', 'my content', 15)`)
	fmt.Println(res, err)
	res, err = cl.Sphinxql(`replace into testrt values(2,'another subject', 'more content', 15)`)
	fmt.Println(res, err)
	res, err = cl.Sphinxql(`replace into testrt values(5,'again subject', 'one more content', 10)`)
	fmt.Println(res, err)
	res2, err2 := cl.Query("more|another", "testrt")
	fmt.Println(res2, err2)
}
```

And here's how it works:
```
[root@srv ~]# go run manticore.go
[Query OK, 1 rows affected] <nil>
[Query OK, 1 rows affected] <nil>
[Query OK, 1 rows affected] <nil>
Status: ok
Query time: 0s
Total: 1
Total found: 1
Schema:
	Fields:
		title
		content
	Attributes:
		gid: int
Matches:
	Doc: 2, Weight: 2, attrs: [15]
Word stats:
	'more' (Docs:2, Hits:2)
	'another' (Docs:1, Hits:1)
 <nil>
```

Read [full documentation on godoc](https://godoc.org/github.com/manticoresoftware/go-sdk/manticore) to learn more about available functions and find more examples. You can also read it from the console as `go doc go-sdk/manticore`

