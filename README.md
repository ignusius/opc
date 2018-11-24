# OPC DA in Go
Read process and automation data in Go from an OPC server for monitoring and data analysis purposes (OPC DA protocol).

```go get github.com/konimarti/opc```

## Installation

* Set ```$GOARCH``` based on your ```OPCDAAuto.dll``` or ```gbda_aut.dll``` in Powershell:
  - If the wrapper is in ```C:\Windows\System32```, use ```$ENV:GOARCH="amd64"```
  - If the wrapper is in ```C:\Windows\SysWOW64```, use ```$ENV:GOARCH="386"```
* ```go get github.com/go-ole/go-ole```
* ```go get github.com/konimarti/opc```

## Prerequisites

* OPC DA Automation Wrapper 2.02 should be installed on your system (```ÒPCDAAuto.dll``` or ```gbda_aut.dll```); the automation wrapper is usually shipped as part of the OPC Core Components of your OPC Server.
* You can get the Graybox DA Automation Wrapper [here](http://gray-box.net/download_daawrapper.php?lang=en).
* Follow the [installation instruction](http://gray-box.net/daawrapper.php) for this wrapper. 

## Testing

* Start Graybox Simulator v1.8 (OPC Simulation Server; this is optional but necessary for testing); can be obtained for free [here](http://www.gray-box.net/download_graysim.php).
* Test code with ```go test -v```

## Example 

```
package main

import (
	"fmt"
	"github.com/konimarti/opc"
)

func main() {
	client := opc.NewConnection(
		"Graybox.Simulator", // ProgId
		[]string{"localhost"}, //  OPC servers nodes
		[]string{"numeric.sin.int64", "numeric.saw.float"}, // slice of OPC tags
	)
	defer client.Close()

	// read single tag: value, quality, timestamp
	fmt.Println(client.ReadItem("numeric.sin.int64"))

	// read all added tags
	fmt.Println(client.Read())
}
``` 

with the following output:

```
{-34 192 2018-11-21 20:59:10 +0000 UTC}
map[numeric.sin.int64:-34 numeric.saw.float:88.9]
```

## OPCFLUX

* Application to write OPC data directly to InfluxDB.

## OPCAPI

* Application to expose OPC tags with a restful API.

