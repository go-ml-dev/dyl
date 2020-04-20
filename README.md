
[![CircleCI](https://circleci.com/gh/sudachen/go-dl.svg?style=svg)](https://circleci.com/gh/sudachen/go-dl)
[![Maintainability](https://api.codeclimate.com/v1/badges/3b8d5bd3fe992a6ce7f2/maintainability)](https://codeclimate.com/github/sudachen/go-dl/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/3b8d5bd3fe992a6ce7f2/test_coverage)](https://codeclimate.com/github/sudachen/go-dl/test_coverage)
[![Go Report Card](https://goreportcard.com/badge/github.com/sudachen/go-dl)](https://goreportcard.com/report/github.com/sudachen/go-dl)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

```golang
/*

int function(int);

#define DEFINE_JUMPER(x) \
        void *_dyl_##x = (void*)0; \
        __asm__(".global "#x"\n\t"#x":\n\tmovq _dyl_"#x"(%rip),%rax\n\tjmp *%rax\n")
  
DEFINE_JUMPER(function);

*/
import "C"

import (
	"go-ml.dev/dyl"
	"runtime"
	"unsafe"
)

func init() {
    urlbase := "https://github.com/sudachen/go-ml/nativelibs/releases/download/files/"
    if runtime.GOOS == "linux" && runtime.GOARCH == "amd64"{
        so := dyl.Load(
            dyl.Cache("go-ml/dyl/libfunction.so"),
            dyl.LzmaExternal(urlbase+"libfunction_lin64.lzma"))
    } else if runtime.GOOS == "windows" && runtime.GOARCH == "amd64" {
        so := dyl.Load(
            dyl.Cache("go-ml/dyl/function.dll"),
            dyl.LzmaExternal(urlbase+"libfunction_win64.lzma"))
    }
    so.Bind("function",unsafe.Pointer(&C._dyl_function))
}

func main() {
    C.function(0)
}
```
