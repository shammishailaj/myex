[Home](../../) | [Alpine Linux](../../alpine-linux/) | [Docker](../../docker/) | [S3CMD](../../s3cmd/) | [Go](../go/) | [System](../../system/) | [Security](../../security/) | [MySQL](../../mysql/) | [InfluxDB](../../influxdb/)

# Following is an illustration of how different functions of the time package work

The same can be tested at: [https://play.golang.org/p/RsarEGMYPFz](https://play.golang.org/p/RsarEGMYPFz)

```
package main

import (
	"fmt"
	"time"
)

func main() {

	fmt.Printf("\n time.now() = %s", time.Now())
	fmt.Printf("\n time.Now().Unix() = %d", time.Now().Unix())
	fmt.Printf("\n time.Now().UnixNano() = %d", time.Now().UnixNano())
	fmt.Printf("\n time.Now().Format(time.UnixDate) = %s", time.Now().Format(time.UnixDate))
	fmt.Printf("\n time.Now().Format(time.ANSIC) = %s", time.Now().Format(time.ANSIC))
	fmt.Printf("\n time.Now().Format(time.Kitchen) = %s", time.Now().Format(time.Kitchen))
	fmt.Printf("\n time.Now().Format(time.RFC1123) = %s", time.Now().Format(time.RFC1123))
	fmt.Printf("\n time.Now().Format(time.RFC1123Z) = %s", time.Now().Format(time.RFC1123Z))
	fmt.Printf("\n time.Now().Format(time.RFC3339) = %s", time.Now().Format(time.RFC3339))
	fmt.Printf("\n time.Now().Format(time.RFC3339Nano) = %s", time.Now().Format(time.RFC3339Nano))
	fmt.Printf("\n time.Now().Format(time.RFC822) = %s", time.Now().Format(time.RFC822))
	fmt.Printf("\n time.Now().Format(time.RFC822Z) = %s", time.Now().Format(time.RFC822Z))
	fmt.Printf("\n time.Now().Format(time.RFC850) = %s", time.Now().Format(time.RFC850))
	fmt.Printf("\n time.Now().Format(time.RubyDate) = %s", time.Now().Format(time.RubyDate))
	fmt.Printf("\n time.Now().Format(time.Stamp) = %s", time.Now().Format(time.Stamp))
	fmt.Printf("\n time.Now().Format(time.StampMicro) = %s", time.Now().Format(time.StampMicro))
	fmt.Printf("\n time.Now().Format(time.StampMilli) = %s", time.Now().Format(time.StampMilli))
	fmt.Printf("\n time.Now().Format(time.StampNano) = %s", time.Now().Format(time.StampNano))
	fmt.Printf("\n time.String() = %s", time.Now().String())
	fmt.Printf("\n time.Now().Year() = %d", time.Now().Year())
	fmt.Printf("\n time.Now().Month() = %d", time.Now().Month())
	fmt.Printf("\n time.Now().Day() = %d", time.Now().Day())
	tzString, offsetSeconds := time.Now().Zone()
	fmt.Printf("\n time.Now().Zone() = %s, %d", tzString, offsetSeconds)
	// Cf. https://programming.guide/go/format-parse-string-time-date-example.html
	fmt.Printf("\n time.Now().Format(\"2006/01/02\") = %s", time.Now().Format("2006/01/02/"))
	fmt.Printf("\n time.Now().Format(\"Mon Jan 2 15:04:05 MST 2006\") = %s", time.Now().Format("Mon Jan 2 15:04:05.123456 MST 2006"))

	// Cf. https://stackoverflow.com/a/48068086/6670698
	fromString := "Mon, 2 Jan 2006 15:04:05 -0700"
	t, e := time.Parse("Mon, _2 Jan 2006 15:04:05 -0700", fromString)
	if e != nil {
		fmt.Printf("err: %s\n", e)
	}
	fmt.Printf("\n time.Parse(\"Mon, _2 Jan 2006 15:04:05 -0700\", fromString): %v", t.Unix())
	fmt.Printf("\n Execution Finished\n\n")
}
```

- Convert IST to UTC / Convert from one Timezone to UTC

Please see code [here](https://play.golang.com/p/vmAtDmsOgJK)

- Add time Example

Please see code [here](https://play.golang.org/p/It-B1jS7nlc)

