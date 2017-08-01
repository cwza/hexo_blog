title: golang test 筆記
categories: golang
tags: [golang, test]
date: 2017-03-16 18:00:57
---

# Table Driven Test
``` go
func TestAdd(t * testing.T) {
  tcs := []struct{ 
    Name string 
    A, B, Expected int
  }{
    {"foo", 1, 1, 2},
    {"bar", 1, -1, 0},
  }
  for _, tc := range tcs {
    t.Run(tc.Name, func(t *testing.T) {
      actual := tc.A + tc.B
      if actual != expected {
        t.Errorf("%d + %d = %d, exected %d", tc.A, tc.B, actual, tc.Expected)
      }
    })
  }
}
```

## Test fixtures
go test set pwd as package directory  
use relative path "test-fixtures" directory as a place to store test data(loading config, model data, binary data ...etc)  
``` go
func TestAdd(t *testing.T) {
  data := filepath.Join("test-fixtures", "daa_data.json")
  // Do something with data
}
```

## Golden files(Test flag)
expected output放在.golden file中  
當update flag為true時, 代表需要更新expected output file  
Very useful to test complex structures  
``` go
var update = flag.Bool("update", false, "update golden files")

func TestAdd(t *testing.T) {
  actual := doSomething(tc)
  golden := filepath.Join("test-fixtures", tc.Name+".golden")
  if *update {
    ioUtil.WriteFile(golden, actual, 0644)
  }

  expected, _ := ioutil.ReadFile(golden)
  if !bytes.Equal(actual, expected) {
    //FAIL!
  }
}
```
``` bash
go test
go test -update
```

## Global State
Do not use global state.
If you have to do, Use default, and let test can easily mock it.
``` go
const defaultPort = 1000

type ServerOpts {
  Port int // default it to defaultPort somewhere
}
```

## Test Helpers
### TestTempFile
Test helper接受testing.T參數, helper中的錯誤直接在helper中處理掉  
helper return a function to clean
``` go
func testTempFile(t * testing.T) (string, func()) {
  tf, err := iotuil.TempFile("", "test")
  if err != nil {
    t.Fatalf("err: %s", err)
  }
  tf.Close()
  return tf.Name(), func() { os.Remove(tf.Name()) }
}

func TestThing(t *testing T) {
  tf, tfclose := testTempFile(t)
  defer tfclose()
  // doSomething with tf
}
```
### TestChdir
切換dir並在function結束前切換回來  
注意testChdir會回傳一function, 該function會將dir切回原本old
由於defer會在defer當下處理完參數所以在defer當下即會切換到dir
然後在TestThing結束前呼叫os.chdir(old)
``` go
func testChdir(t *testing.T, dir string) func() {
  old, err := os.Getwd()
  if err != nil {
    t.Fatalf("err: %s", err)
  }
  if err := os.Chdir(dir); err != nil {
    t.Fatalf("err: %s", err)
  }

  return func() { os.Chdir(old) }
}

func TestThing(t *testing.T) {
  defer testChdir(t, "/other")()
}
```

## Networking
不需要mock net.Conn
``` go
func TestConn(t *testing.T) (client, server net.Conn) {
  ln, err := net.Listen("tcp", "127.0.0.1:0")

  var server net.Conn
  go func() {
    defer ln.Close()
    server, err = ln.Acccept()
  }()

  client, err := net.Dial("tcp", ln.Addr().String())
  return client, server
}
```

## Test Subprocessing
Check if git is installed. If not, skip test
``` go
var testHasGit bool

func init() {
  if _, err := exec.LookPath("git"); err == nil {
    testHasGit = true
  }
}

func TestGitGetter(t *testing.T) {
  if !testHasGit {
    t.Log("git not found, skipping")
    t.Skip()
  }
}
```

## Testing as a public API
提供使用你的package的user一個測試你的package的public API(Maybe a mock or a helper)
*_test.go will be ignored by go, so use testing_*.go for Test for API
Example:
* API Server
  + TestServer(t) (net.Addr, io.Closer) => Returns a fully started in-memory server and a closer to close it.
* interface for downloading files
  + TestDownloader(t, Downloader) => Tests all the properties a downloader should have.
  + struct DownloaderMock[] => Implements Downloader as a mock

## Timing-Dependent Test
timeMultiplier is configurable.
maybe lower at laptop and higher at server
``` go
func TestThing(t * testing.T) {
  timeout := 3 * time.Minute * timeMultiplier

  select {
  case <- thingHappened:
  case <- time.After(timeout):
    t.Fatal("timeout")
  }
}
```