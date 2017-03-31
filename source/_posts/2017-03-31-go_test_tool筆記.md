title: go test tool筆記
categories: golang
tags: [golang, test]
date: 2017-03-31 11:38:57
---

## run all test except vendor
``` sh
go test $(go list ./... | grep -v /vendor/)
```

## go tool cover
``` sh
go test -coverprofile coverage.txt <package name>
go tool cover -html=coverage.txt -o coverage.html
chrome coverage.html
```

## example script to run all package test and generate coverage file
``` sh
#!/usr/bin/env bash

set -e
out="coverage.txt"
tmp="profile.coverage.out"
except="example|testproto"

: > $out

i=0
for d in $(go list ./... | grep -v vendor); do
    # except some package
    if [[ $d =~ $except ]]; then
        continue
    fi
    
    echo -e "TESTS FOR: for \033[0;35m${d}\033[0m"
    go test -v -coverprofile=$tmp $d
    if [ -f $tmp ]; then
        # remove mode:xxx if not first package for go tool cover
        if [ "$i" != 0 ]; then
            sed -i '' -e '/^mode:/d' ./$tmp
        fi
        cat $tmp >> $out
        rm $tmp
    fi
    echo ""
    ((i++))
done
```