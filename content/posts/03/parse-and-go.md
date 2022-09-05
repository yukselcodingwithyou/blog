---
weight: 3
title: "Parse & Go"
date: 2021-12-03T21:29:01+08:00
lastmod: 2021-12-03T21:29:01+08:00
draft: false
author: "Yuksel Ozdemir"
authorLink: "https://yukselcodingwithyou.github.io/resume"
description: "A Logger In Golang"
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"

tags: ["parse", "configs", "go"]
categories: ["blog-posts"]

lightgallery: true

toc:
  auto: false
---


## Parse & Go

While we develop our applications, we need to have changeable values for our application to ease the management. For this purpose, we use configurations. 

<br/>

These configurations may be defined on your application directory path, or in a remote config server. These configurations also can be in many formats like JSON, YAML, PROPERTIES, ENV etc. This module enables you to read your configurations in your local or in your remote config server.

This weekend, for fun and broaden the knowledge of GoLang, I tried to implement a configuration parser. I published this package on my Github account in this [repository](https://github.com/yukselcodingwithyou/parseandgo).

<br/>

I will be giving the details about how to use this utility to read your configurations in your applications.

<br/>

## Steps
### Step #1

First, you need to get package, using the command below.
<br/>

```go get github.com/yukselcodingwithyou/parseandgo@latest```

<br/>

### Step #2
Then, you need to import parser, as visualized below.

<br/>

```import configparser "github.com/yukselcodingwithyou/parseandgo"```

<br/>

### Step #3
Now, you can create your parser with the defined types below.

- JSON
- YML
- ENV
- PROPERTIES
  
Also, you need to give an address for your file, can be a remote file in a server, for example (Amazon S3), or in your local directory.

## Examples
### JSON Example
Example JSON file is shown below.

<br/>
```
{
    "fileFormat": "json",
    "name": "yuksel",
    "surname": "ozdemir",
    "otherInfo": {
        "age": 26,
        "city": "Ankara",
        "student": false
    }
}
```
<br/>


Example code for usage of JSON parser is shown below.

<br/>

```
package main

import (
	"fmt"
	configparser "github.com/yukselcodingwithyou/parseandgo"
)

func main() {
	jsonParserFilePath := configparser.NewParser(configparser.JSON, "example.json")
	jsonConfigFilePath := configparser.Parse(jsonParserFilePath)
	fmt.Println("---- start JSON File Path----")
	testJSON(jsonConfigFilePath)
	fmt.Println("---- end JSON File Path----")

	jsonParserURL := configparser.NewParser(configparser.JSON, "CONFIG_SERVER_URL")
	jsonConfigURL := configparser.Parse(jsonParserURL)
	fmt.Println("---- start JSON URL----")
	testJSON(jsonConfigURL)
	fmt.Println("---- end JSON URL----")
}

func testJSON(config configparser.Config) {
	student, err := config.Value("otherInfo", "student").Bool()
	if err == nil {
		fmt.Println(*student)
	}

	age, err := config.Value("otherInfo", "age").Int()
	if err == nil {
		fmt.Println(*age)
	}

	city, err := config.Value("otherInfo", "city").String()
	if err == nil {
		fmt.Println(*city)
	}

	surname, err := config.Value("surname").String()
	if err == nil {
		fmt.Println(*surname)
	}

	name, err := config.Value("name").String()
	if err == nil {
		fmt.Println(*name)
	}

	fileFormat, err := config.Value("fileFormat").String()
	if err == nil {
		fmt.Println(*fileFormat)
	}
}
```


### YML Example
Example YML file is shown below.

<br/>

```
file:
    format: yml

person:
    name: yuksel
    surname: ozdemir

other:
    info:
    person:
        age: 26
        city: Ankara
        student: false
```

<br/>

Example code for usage of YML parser is shown below.

```
package main

import (
	"fmt"
	configparser "github.com/yukselcodingwithyou/parseandgo"
)

func main() {
	yamlParserFilePath := configparser.NewParser(configparser.YAML, "example.yml")
	yamlConfigFilePath := configparser.Parse(yamlParserFilePath)
	fmt.Println("---- start YAML FilePath----")
	testYaml(yamlConfigFilePath)
	fmt.Println("---- end YAML FilePath----")

	yamlParserURL := configparser.NewParser(configparser.YAML, "CONFIG_SERVER_URL")
	yamlConfigURL := configparser.Parse(yamlParserURL)
	fmt.Println("---- start YAML URL----")
	testYaml(yamlConfigURL)
	fmt.Println("---- end YAML URL----")
}

func testYaml(config configparser.Config) {
	student, err := config.Value("other", "info", "person", "student").Bool()
	if err == nil {
		fmt.Println(*student)
	}
	age, err := config.Value("other", "info", "person", "age").Int()
	if err == nil {
		fmt.Println(*age)
	}
	city, err := config.Value("other", "info", "person", "city").String()
	if err == nil {
		fmt.Println(*city)
	}

	surname, err := config.Value("person", "surname").String()
	if err == nil {
		fmt.Println(*surname)
	}

	name, err := config.Value("person", "name").String()
	if err == nil {
		fmt.Println(*name)
	}

	fileFormat, err := config.Value("file", "format").String()
	if err == nil {
		fmt.Println(*fileFormat)
	}
}
```


### PROPERTIES Example
Example PROPERTIES file is shown below.

<br/>

```
file.format=properties
person.name=yuksel
person.surname=ozdemir
other.info.person.age=26
other.info.person.city=Ankara
other.info.person.student=false
```
<br/>

Example code for usage of PROPERTIES parser is shown below.

```
package main

import (
	"fmt"
	configparser "github.com/yukselcodingwithyou/parseandgo"
)

func main() {
	propertiesParserFilePath := configparser.NewParser(configparser.PROPERTIES, "example.properties")
	propertiesConfigFilePath := configparser.Parse(propertiesParserFilePath)
	fmt.Println("---- start PROPERTIES FilePath----")
	testProperties(propertiesConfigFilePath)
	fmt.Println("---- end PROPERTIES FilePath----")

	propertiesParserURL := configparser.NewParser(configparser.PROPERTIES, "CONFIG_SERVER_URL")
	propertiesConfigURL := configparser.Parse(propertiesParserURL)
	fmt.Println("---- start PROPERTIES URL----")
	testProperties(propertiesConfigURL)
	fmt.Println("---- end PROPERTIES URL----")
}

func testProperties(config configparser.Config) {
	student, err := config.Value("other.info.person.student").Bool()
	if err == nil {
		fmt.Println(*student)
	}
	age, err := config.Value("other.info.person.age").Int()
	if err == nil {
		fmt.Println(*age)
	}
	city, err := config.Value("other.info.person.city").String()
	if err == nil {
		fmt.Println(*city)
	}

	surname, err := config.Value("person.surname").String()
	if err == nil {
		fmt.Println(*surname)
	}

	name, err := config.Value("person.name").String()
	if err == nil {
		fmt.Println(*name)
	}

	fileFormat, err := config.Value("file.format").String()
	if err == nil {
		fmt.Println(*fileFormat)
	}
}
```

### ENV Example
Example ENV file is shown below.

<br/>

```
fileFormat=env
name=yuksel
surname=ozdemir
age=26
city=Ankara
student=false
```

<br/>


Example code for usage of ENV parser is shown below.


```
package main

import (
	"fmt"
	configparser "github.com/yukselcodingwithyou/parseandgo"
)

func main() {
    envParserFilePath := configparser.NewParser(configparser.ENV, "example.env")
	envConfigFilePath := configparser.Parse(envParserFilePath)
	fmt.Println("---- start ENV File Path----")
	testEnv(envConfigFilePath)
	fmt.Println("---- end ENV File Path----")

	envParserURL := configparser.NewParser(configparser.ENV, "CONFIG_SERVER_URL")
	envConfigURL := configparser.Parse(envParserURL)
	fmt.Println("---- start ENV URL----")
	testEnv(envConfigURL)
	fmt.Println("---- end ENV URL----")
}

func testEnv(config configparser.Config) {
	student, err := config.Value("student").Bool()
	if err == nil {
		fmt.Println(*student)
	}
	age, err := config.Value("age").Int()
	if err == nil {
		fmt.Println(*age)
	}
	city, err := config.Value("city").String()
	if err == nil {
		fmt.Println(*city)
	}

	surname, err := config.Value("surname").String()
	if err == nil {
		fmt.Println(*surname)
	}

	name, err := config.Value("name").String()
	if err == nil {
		fmt.Println(*name)
	}

	fileFormat, err := config.Value("fileFormat").String()
	if err == nil {
		fmt.Println(*fileFormat)
	}
}
```

Supported types for a value in config are listed below:

- String
- Float
- Int
- Bool

### Sum up

<br/>
This is a lightweight parser for the use of parsing specific types of files from your local directory, or from a remote configuration server.

<br/>

For any type of questions, advices you can mail me, or open issues in repository.

<br/>

Happy Parsing !… :)
