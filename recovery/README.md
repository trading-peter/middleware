## Middleware information

This folder contains a middleware for safety recover the server from panic

## Install

```sh
$ go get -u github.com/iris-contrib/middleware.v5/recovery
```

## How to use

```go

package main

import (
	"gopkg.in/kataras/iris.v5"
	"github.com/iris-contrib/middleware/recovery"
)

func main() {

	iris.Use(recovery.Handler) 
	iris.Get("/", func(ctx *iris.Context) {
		ctx.Write("Hi, let's panic")
		panic("errorrrrrrrrrrrrrrr")
	})

	iris.Listen(":8080")
}

```
