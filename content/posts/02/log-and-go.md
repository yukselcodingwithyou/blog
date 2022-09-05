---
weight: 2
title: "Log & Go"
date: 2021-11-06T21:29:01+08:00
lastmod: 2021-11-06T21:29:01+08:00
draft: false
author: "Yuksel Ozdemir"
authorLink: "https://yukselcodingwithyou.github.io/resume"
description: "A Logger In Golang"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["logging", "development", "go"]
categories: ["blog-posts"]

lightgallery: true

toc:
  auto: false
---
## Log & Go
This weekend, I decided to build a logger library and publish this in my GitHub account, for people to use, also for fun :) and broaden my perspective and knowledge in GoLang domain.

<br/>

The logger utility that is implemented can be found in this [repository](https://github.com/yukselcodingwithyou/logandgo).

<br/>

I will be giving the details about how to use this utility to log every event occurred in your applications.

<br/>

## Steps
### Step #1

<br/>

```go get github.com/yukselcodingwithyou/logandgo@latest```

<br/>

This command above is necessary for you to use this utility in your application.

<br/>

### Step #2

<br/>

```import logger "github.com/yukselcodingwithyou/logandgo"```

<br/>

Import the downloaded utility to your go files, as visualized above.

### Step #3

Create a new logger with a format and level as you wish, format and level information should be like below.
<br/>
LogLevels
- PANIC = 0
- ERROR = 1
- WARN = 2
- INFO = 3
- DEBUG = 4

<br/>

Formats
- JSON
- TEXT

## Examples

### JSON Logger Example

The example below is going to log in WARN level with JSON format. In case you have configured your log level in application level to be WARN, then our logger utility will not log INFO or above levels but just PANIC, ERROR, WARN levels

<br/>
```
package main
import logger "github.com/yukselcodingwithyou/logandgo"
func main() {
    jsonLogger := logger.NewLogger(logger.JSON, 2)
    jsonLogger.Debug(logger.LogFields{
        "title": "title",
        "reason": "debug",
        "payload": "payload",
    })
    jsonLogger.Error(logger.LogFields{
        "title": "title",
        "reason": "error",
        "payload": "payload",
    })
    jsonLogger.Info(logger.LogFields{
        "title": "title",
        "reason": "info",
        "payload": "payload",
    })
    jsonLogger.Panic(logger.LogFields{
        "title": "title",
        "reason": "panic",
        "payload": "payload",
    })
    jsonLogger.Warn(logger.LogFields{
        "title": "title",
        "reason": "warn",
        "payload": "payload",
    })
}
```

<br/>

### JSON Logger Output Example

<br/>

```
ERROR: 2021/11/21 16:12:00 main.go:14 {"payload":"payload","reason":"error","title":"title"}
PANIC: 2021/11/21 16:12:00 main.go:26: {"payload":"payload","reason":"panic","title":"title"}
WARNING: 2021/11/21 16:12:00 main.go:32: {"payload":"payload","reason":"warn","title":"title"}
```

### TEXT Logger Example

The example below is going to log in ALL levels defined with TEXT format.
```
  package main

  import logger "github.com/yukselcodingwithyou/logandgo"
  func main() {
     textLogger := logger.NewLogger(logger.TEXT, 4)
    
     textLogger.Debug(logger.LogFields{
        "title": "title",
        "reason": "debug",
        "payload": "payload",
     })
     textLogger.Error(logger.LogFields{
        "title": "title",
        "reason": "error",
        "payload": "payload",
    })
     
     textLogger.Info(logger.LogFields{
        "title": "title",
        "reason": "info",
        "payload": "payload",
     })
    
     textLogger.Panic(logger.LogFields{
          "title": "title",
          "reason": "panic",
          "payload": "payload",
     })
    
     textLogger.Warn(logger.LogFields{
          "title": "title",
          "reason": "warn",
          "payload": "payload",
     })
  }
```

<br/>

### TEXT Logger Output Example

<br/>

```
DEBUG: 2021/11/21 16:39:06 main.go:8: title=title reason=debug payload=payload
ERROR: 2021/11/21 16:39:06 main.go:14: reason=error payload=payload title=title
INFO: 2021/11/21 16:39:06 main.go:20: reason=info payload=payload title=title
PANIC: 2021/11/21 16:39:06 main.go:26: title=title reason=panic payload=payload
WARNING: 2021/11/21 16:39:06 main.go:32: title=title reason=warn payload=payload
```

<br/>

## Sum up
This is a lightweight logger utility that can be used in your go applications. For any kind of feedback, you can reach me from LinkedIn. In case you start using the module, you should know that I will be very happy & proud.. :)

<br/>
Happy Logging..!