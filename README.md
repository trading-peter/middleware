This repository provides a way to share any minor handlers for [iris](https://github.com/kataras/iris) web framework. You can view the built'n supported handlers by pressing [here](https://github.com/kataras/iris/tree/v8/middleware).

[![Build status](https://api.travis-ci.org/iris-contrib/middleware.svg?branch=v8&style=flat-square)](https://travis-ci.org/iris-contrib/middleware)

# Installation

A simple copy-paste and `go get ./...` to resolve the dependencies, the newest middleware repository can be retrieved by `go get` but this branch targets an older version of Iris and so its middleware repository is stored on its own branch for stability reasons.

Follow the instructions below:

1. clear yours previously `$GOPATH/src/github.com/iris-contrib/middleware` folder or create new
2. download the middleware packages: https://github.com/iris-contrib/middleware/archive/v8.zip
3. extract the contents of the `middleware-v8` folder that's inside the downloaded zip file to your `$GOPATH/src/github.comiris-contrib/middleware`
4. navigate to your `$GOPATH/src/github.com/iris-contrib/middleware` folder if you're not already there and open a terminal/command prompt, execute the command: `go get ./...` and you're ready to GO:)


Middleware is just a chain handlers which can be executed before or after the main handler, can transfer data between handlers and communicate with third-party libraries, they are just functions.

| Middleware | Description | Example |
| -----------|--------|-------------|
| [jwt](https://github.com/iris-contrib/middleware/tree/v8/jwt) | Middleware checks for a JWT on the `Authorization` header on incoming requests and decodes it. | [jwt/_example](https://github.com/iris-contrib/middleware/tree/v8/jwt/_example) |
| [cors](https://github.com/iris-contrib/middleware/tree/v8/cors) | HTTP Access Control. | [cors/_example](https://github.com/iris-contrib/middleware/tree/v8/cors/_example) |
| [secure](https://github.com/iris-contrib/middleware/tree/v8/secure) | Middleware that implements a few quick security wins. | [secure/_example](https://github.com/iris-contrib/middleware/tree/v8/secure/_example/main.go) |
| [tollbooth](https://github.com/iris-contrib/middleware/tree/v8/tollboothic) | Generic middleware to rate-limit HTTP requests. | [tollbooth/_examples/limit-handler](https://github.com/iris-contrib/middleware/tree/v8/tollbooth/_examples/limit-handler) |
| [cloudwatch](https://github.com/iris-contrib/middleware/tree/v8/cloudwatch) |  AWS cloudwatch metrics middleware. |[cloudwatch/_example](https://github.com/iris-contrib/middleware/tree/v8/cloudwatch/_example) |
| [new relic](https://github.com/iris-contrib/middleware/tree/v8/newrelic) | Official [New Relic Go Agent](https://github.com/newrelic/go-agent). | [newrelic/_example](https://github.com/iris-contrib/middleware/tree/v8/newrelic/_example) |
| [prometheus](https://github.com/iris-contrib/middleware/tree/v8/prometheus)| Easily create metrics endpoint for the [prometheus](http://prometheus.io) instrumentation tool | [prometheus/_example](https://github.com/iris-contrib/middleware/tree/v8/prometheus/_example) |
| [casbin](https://github.com/iris-contrib/middleware/tree/v8/casbin)| An authorization library that supports access control models like ACL, RBAC, ABAC | [casbin/_examples](https://github.com/iris-contrib/middleware/tree/v8/casbin/_examples) |
| [raven](https://github.com/iris-contrib/middleware/tree/v8/raven)| Sentry client in Go | [raven/_example](https://github.com/iris-contrib/middleware/blob/v8/raven/_example/main.go) |
| [csrf](https://github.com/iris-contrib/middleware/tree/v8/csrf)| Cross-Site Request Forgery Protection | [csrf/_example](https://github.com/iris-contrib/middleware/blob/v8/csrf/_example/main.go) |
### How can I register middleware?

**To a single route**

```go
app := iris.New()
app.Get("/mypath", myMiddleware1, myMiddleware2, func(ctx iris.Context){}, func(ctx iris.Context){}, myMiddleware5,myMainHandlerLast)
```

**To a party of routes or subdomain**

```go

myparty := app.Party("/myparty", myMiddleware1,func(ctx context.Context){},myMiddleware3)
{
	//....
}

```

**To all routes**

```go
app.Use(func(ctx iris.Context){}, myMiddleware2)
```

**To global, all routes, parties and subdomains**

```go
app.UseGlobal(func(ctx iris.Context){}, myMiddleware2)
```

## Can I use standard net/http handler with iris?

**Yes** you can, just pass the Handler inside the `iris.FromStd` in order to be converted into iris.Handler and register it as you saw before.

### Convert handler which has the form of `http.Handler/HandlerFunc`

```go
package main

import (
    "github.com/kataras/iris"
)

func main() {
    app := iris.New()

    sillyHTTPHandler := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request){
            println(r.RequestURI)
    })

    sillyConvertedToIon := iris.FromStd(sillyHTTPHandler)
    // FromStd can take (http.ResponseWriter, *http.Request, next http.Handler) too!
    app.Use(sillyConvertedToIon)

    app.Run(iris.Addr(":8080"))
}

```

## Contributing

If you are interested in contributing to this project, please push a PR.

## People

[List of all contributors](https://github.com/iris-contrib/middleware/graphs/contributors)