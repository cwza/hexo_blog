title: go-middleware筆記
categories: golang
tags: [golang]
date: 2017-03-15 16:00:57
---

Two example about middleware in go
Reference: http://www.alexedwards.net/blog/making-and-using-middleware

<!-- more -->
## A basic middleware chain example
``` go
package main

import (
  "log"
  "net/http"
)

func middlewareOne(next http.Handler) http.Handler {
  return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
    log.Println("Executing middlewareOne")
    next.ServeHTTP(w, r)
    log.Println("Executing middlewareOne again")
  })
}

func middlewareTwo(next http.Handler) http.Handler {
  return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
    log.Println("Executing middlewareTwo")
    if r.URL.Path != "/" {
      return
    }
    next.ServeHTTP(w, r)
    log.Println("Executing middlewareTwo again")
  })
}

func final(w http.ResponseWriter, r *http.Request) {
  log.Println("Executing finalHandler")
  w.Write([]byte("OK"))
}

func main() {
  finalHandler := http.HandlerFunc(final)

  http.Handle("/", middlewareOne(middlewareTwo(finalHandler)))
  http.ListenAndServe(":3000", nil)
}
```

## example of chaining 3rd party middleware

``` go

package main

import (
	"bytes"
	"github.com/goji/httpauth"
	"github.com/gorilla/handlers"
	"net/http"
	"os"
)

func enforceXMLHandler(next http.Handler) http.Handler {
	return http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
		if r.ContentLength == 0 {
			http.Error(w, http.StatusText(400), 400)
			return
		}

		buf := new(bytes.Buffer)
		buf.ReadFrom(r.Body)
		if http.DetectContentType(buf.Bytes()) != "text/xml; charset=utf-8" {
			http.Error(w, http.StatusText(415), 415)
			return
		}

		next.ServeHTTP(w, r)
	})
}

func myLoggingHandler(h http.Handler) http.Handler {
	logFile, err := os.OpenFile("server.log", os.O_RDWR|os.O_CREATE|os.O_APPEND, 0666)
	if err != nil {
		panic(err)
	}
	return handlers.LoggingHandler(logFile, h)
}

func main() {
	indexHandler := http.HandlerFunc(index)
	authHandler := httpauth.SimpleBasicAuth("username", "password")

	http.Handle("/", myLoggingHandler(authHandler(enforceXMLHandler(indexHandler))))
	http.ListenAndServe(":3000", nil)
}

func index(w http.ResponseWriter, r *http.Request) {
	w.Write([]byte("OK"))
}

```