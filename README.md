# README #

To install:  `go get github.com/technicalviking/tulipindicators`

### What is this repository for? ###

* Go bindings for TulipIndicators C Lib (tulipindicators.org)
* Covers all 103 indicators as found in version 0.8.0 of the c lib.

### Usage ###
* This package exports 3 things:
** an IndicatorInfo struct type
** a map of strings to IndicatorInfo structs
** a map of strings to Indicator methods.
* The IndicatorInfo struct is a port of the C ti_indicator_info struct with one caveat.  As it seems the sole purpose of the 'start' function is to tell you how to build the 'outputs' structure when using this lib in a C project, I've not exposed it.  Instead it's the start method is referenced internally in the Indicator method.
* In keeping with the conventions demonstrated in the C lib, all indicator methods have the same parameters, which are equivalents to the c parameters required. Again, with a caveat.  in C, the outputs parameter has to be created in advance of calling the indicator method because it's modified in place by said method.  Here, I'm leveraging the internal call to the start method and the multiple return value feature of Go to pass the outputs structure back as a return value.  Cool, right?
* In a fit of laziness, I'm not including the documentation of each indicator method here.  There's already really good documentation for each of the indicators on the tulipindicators site (https://tulipindicators.org/list).

### Examples ###


## abs ##
Not only the first indicator on the list,  but one of the easiest to use!  Requires one slice of inputs, and no options!

```
import (
	"github.com/technicalviking/tulipindicators"
	"fmt"
)
func test() {
	sampleInputs := []float64{-1.1, -2.2, -3.3}
	sampleOptions := []float64{}

	sampleOutputs, err:= tulipindicators.Indicators["abs"](
		[][]float64{sampleInputs},
		sampleOptions,
	)
	
	//sampleOutputs is always going to be a [][]float64.
	//this should output [[1.1, 2.2, 3.3]]
	fmt.Printf("Output values: %v", sampleOutputs)

	//if the c code would result in returning 1 (TI_INVALID_OPTION), this presents in the 'err' result.
	//err will also indicate if there were insufficient inputs (where funky things with 'start' and negative numbers happen)
	fmt.Printf("Error: %v", err)
}

```


### What do I need to know? ###

* Go (with specific attention paid to CGO)
* I developed this on a 64 bit Windows 10 machine.  I've tried my best to not do anything Windows-specific here, but if I've goofed in that regard, lemme know!
* The main source of this repo takes advantage of an aspect of go where if the "C" pseudopackage is included, all c and h files in that directory are also compiled and included.
* To that end, in an effort to keep things portable, I've simply copied files from https://github.com/TulipCharts/tulipindicators into this repo, and changed include paths where necessary.
* using a C compiler should not be required, outside of the one Go uses internally.

### Contribution guidelines ###

* `go test -cover` indicates this lib is 100% covered by tests.  If you'd like to contribute, I'd ask that any PR keeps that percentage.
* This was my first go project using CGO, so if you're a CGO expert and you see glaring issues, I'd be happy to discuss them with you!

### Feedback? Questions? Bugs? ###

* Raise an issue here (preferred), or email me with 'tulipindicators' in the subject.

### Todo ###
* More examples!  (Feel free to make a PR just for that!)