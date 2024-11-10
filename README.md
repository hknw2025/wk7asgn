# Week 7 Assignment - AI Assisited Programming

Below are the suggestions that I obtained for improving the Go For Stats Assignment using GitHub CoPilot for programming assistance and ChatGPT for code generation. In the attempts to solict AI advice below, my questions concerned main.go in my week 4 repository. 

# GitHub CoPilot

Suggested Code

package main

import (
    "encoding/csv"
    "fmt"
    "log"
    "os"
    "strconv"

    "github.com/sajari/regression"
)

// RunReg initializes and returns four regression models based on the provided data.
func RunReg(data [][]string) (regression.Regression, regression.Regression, regression.Regression, regression.Regression) {
    r1 := initializeRegression("yvar1", "xvar1")
    r2 := initializeRegression("yvar2", "xvar2")
    r3 := initializeRegression("yvar3", "xvar3")
    r4 := initializeRegression("yvar4", "xvar4") // Assuming you need a fourth regression model

    // Add data to the regression models here if needed

    return r1, r2, r3, r4
}

// initializeRegression creates and initializes a regression model with the given observed and variable names.
func initializeRegression(observed, variable string) regression.Regression {
    var r regression.Regression
    r.SetObserved(observed)
    r.SetVar(0, variable)
    return r
}

func main() {
    // Example usage of RunReg function
    data := [][]string{
        // Add your CSV data here
    }

    r1, r2, r3, r4 := RunReg(data)

    // Use the regression models r1, r2, r3, r4 as needed
    fmt.Println(r1, r2, r3, r4)
}

GitHub CoPilot suggested that I add error handling, code comments, function documentation for the RunReg function, and avoid the repetition of code chunks by writing a helper function. 

In its improved code, it created a new function called initializeRegression which does help to make the final code much more readable and concise.

Additionally, it added some comments that would make the file easier to understand for a new user. Those comments included explaining the RunReg function, which was certainly a good suggestion as functions should usually have comments explaining their usage and purpose.

While the comments provided on my code are helpful and the suggested code handles many of those comments by itself, the suggested code leaves out large chunks of code that is needed for the main code to function. It leaves out the code used to read the CSV data and add it into the regression models for evaluation. There are specific place holders built into the suggested code for the code to accomplish those tasks though. Still, that issue and problems such as the suggested code simplifying things such as the final print statement in undesireable ways means the suggested code would need to be fixed and modified before usage.


# Chat GPT

Suggested Code


yvar1, err := strconv.ParseFloat(record[4], 64)
if err != nil {
    log.Fatalf("Failed to parse yvar1 at row %d: %v", i, err)
}

func trainRegression(r *regression.Regression, yIndex, xIndex int, data [][]string) {
    for i, record := range data {
        if i == 0 {
            continue // Skip header
        }
        y, err := strconv.ParseFloat(record[yIndex], 64)
        if err != nil {
            log.Fatalf("Failed to parse y at row %d: %v", i, err)
        }
        x, err := strconv.ParseFloat(record[xIndex], 64)
        if err != nil {
            log.Fatalf("Failed to parse x at row %d: %v", i, err)
        }
        r.Train(regression.DataPoint(y, []float64{x}))
    }
    r.Run()
}

func RunReg(data [][]string) (regression.Regression, regression.Regression, regression.Regression, regression.Regression) {
    var r1, r2, r3, r4 regression.Regression
    r1.SetObserved("yvar1")
    r1.SetVar(0, "xvar1")
    r2.SetObserved("yvar2")
    r2.SetVar(0, "xvar2")
    r3.SetObserved("yvar3")
    r3.SetVar(0, "xvar3")
    r4.SetObserved("yvar4")
    r4.SetVar(0, "xvar4")

    trainRegression(&r1, 4, 0, data)
    trainRegression(&r2, 5, 1, data)
    trainRegression(&r3, 6, 2, data)
    trainRegression(&r4, 7, 3, data)

    return r1, r2, r3, r4
}

fmt.Printf("\nRegression Formulas:\n")
fmt.Printf("r1: %v\n", r1.Formula)
fmt.Printf("r2: %v\n", r2.Formula)
fmt.Printf("r3: %v\n", r3.Formula)
fmt.Printf("r4: %v\n", r4.Formula)

func RunReg(data [][]string) []regression.Regression {
    regressions := make([]regression.Regression, 4)
    for i := range regressions {
        regressions[i].SetObserved(fmt.Sprintf("yvar%d", i+1))
        regressions[i].SetVar(0, fmt.Sprintf("xvar%d", i+1))
    }
    // Then loop through and train each using the trainRegression function...
    return regressions
}


package main

import (
	"encoding/csv"
	"fmt"
	"log"
	"os"
	"strconv"

	"github.com/sajari/regression"
)

// trainRegression trains a regression model using y and x values from the given columns in data.
func trainRegression(r *regression.Regression, yIndex, xIndex int, data [][]string) {
	for i, record := range data {
		if i == 0 {
			continue // Skip header
		}

		y, err := strconv.ParseFloat(record[yIndex], 64)
		if err != nil {
			log.Fatalf("Failed to parse y at row %d, column %d: %v", i, yIndex, err)
		}
		x, err := strconv.ParseFloat(record[xIndex], 64)
		if err != nil {
			log.Fatalf("Failed to parse x at row %d, column %d: %v", i, xIndex, err)
		}
		r.Train(regression.DataPoint(y, []float64{x}))
	}
	r.Run()
}

// RunReg initializes and trains four regression models based on the provided data.
func RunReg(data [][]string) []regression.Regression {
	regressions := make([]regression.Regression, 4)
	varLabels := []string{"xvar1", "xvar2", "xvar3", "xvar4"}
	obsLabels := []string{"yvar1", "yvar2", "yvar3", "yvar4"}

	// Initialize each regression model with variable names
	for i := range regressions {
		regressions[i].SetObserved(obsLabels[i])
		regressions[i].SetVar(0, varLabels[i])
	}

	// Train each regression on corresponding data columns
	trainRegression(&regressions[0], 4, 0, data)
	trainRegression(&regressions[1], 5, 1, data)
	trainRegression(&regressions[2], 6, 2, data)
	trainRegression(&regressions[3], 7, 3, data)

	return regressions
}

func main() {
	// Open CSV file
	f, err := os.Open("anscombe.csv")
	if err != nil {
		log.Fatal(err)
	}
	defer f.Close()

	// Read CSV data
	testdata := csv.NewReader(f)
	testdata.FieldsPerRecord = 8
	records, err := testdata.ReadAll()
	if err != nil {
		log.Fatal(err)
	}

	// Run regression analysis
	regressions := RunReg(records)

	// Output the trained model formulas
	fmt.Println("\nRegression Formulas:")
	for i, r := range regressions {
		fmt.Printf("Regression %d: %v\n", i+1, r.Formula)
	}
}

package main

import (
	"encoding/csv"
	"flag"
	"fmt"
	"log"
	"os"
	"strconv"

	"github.com/sajari/regression"
)

// VariableSpec defines which columns correspond to the independent and dependent variables.
type VariableSpec struct {
	yIndex int
	xIndex int
	yLabel string
	xLabel string
}

// trainRegression trains a regression model based on the given column indices and labels.
func trainRegression(r *regression.Regression, spec VariableSpec, data [][]string) {
	for i, record := range data {
		if i == 0 {
			continue // Skip header
		}

		y, err := strconv.ParseFloat(record[spec.yIndex], 64)
		if err != nil {
			log.Fatalf("Failed to parse %s at row %d: %v", spec.yLabel, i, err)
		}
		x, err := strconv.ParseFloat(record[spec.xIndex], 64)
		if err != nil {
			log.Fatalf("Failed to parse %s at row %d: %v", spec.xLabel, i, err)
		}
		r.Train(regression.DataPoint(y, []float64{x}))
	}
	r.Run()
}

// RunReg initializes and trains regression models based on the provided data and variable specifications.
func RunReg(data [][]string, specs []VariableSpec) []regression.Regression {
	regressions := make([]regression.Regression, len(specs))

	for i, spec := range specs {
		r := &regressions[i]
		r.SetObserved(spec.yLabel)
		r.SetVar(0, spec.xLabel)
		trainRegression(r, spec, data)
	}

	return regressions
}

func main() {
	// Parse command-line flag for the CSV file path
	filePath := flag.String("file", "anscombe.csv", "Path to the CSV file")
	flag.Parse()

	// Open CSV file
	f, err := os.Open(*filePath)
	if err != nil {
		log.Fatalf("Failed to open file %s: %v", *filePath, err)
	}
	defer f.Close()

	// Read CSV data
	testdata := csv.NewReader(f)
	records, err := testdata.ReadAll()
	if err != nil {
		log.Fatalf("Failed to read CSV data: %v", err)
	}

	// Verify that the data has the expected number of columns
	if len(records[0]) < 8 {
		log.Fatalf("Expected at least 8 columns in the CSV, but found %d", len(records[0]))
	}

	// Define the regression models we want to train
	specs := []VariableSpec{
		{yIndex: 4, xIndex: 0, yLabel: "yvar1", xLabel: "xvar1"},
		{yIndex: 5, xIndex: 1, yLabel: "yvar2", xLabel: "xvar2"},
		{yIndex: 6, xIndex: 2, yLabel: "yvar3", xLabel: "xvar3"},
		{yIndex: 7, xIndex: 3, yLabel: "yvar4", xLabel: "xvar4"},
	}

	// Run regression analysis
	regressions := RunReg(records, specs)

	// Output the trained model formulas and metrics
	fmt.Println("\nRegression Results:")
	for i, r := range regressions {
		fmt.Printf("Regression %d: %v\n", i+1, r.Formula)
		fmt.Printf("R-squared: %.4f\n\n", r.R2)
	}
}


The suggestions ChatGPT came up with were similar to CoPilot, but it did a much better job of implementing its suggestions in its suggested code. It suggested I add error handling for the strconv.ParseFloat function which my first version lacked. This was also a suggestion of CoPilot, but CoPilot did not implement this suggestion in its example code while ChatGPT did. It also suggested writing a helper function to avoid code repetition and implemented that idea in a slightly different way than CoPilot. Finally, it suggested I think about chaning the format of the output of my script and further documenting my functions. Finally, when I asked for ideas for further improvements to the code, it came up with an interesting list of ideas for improvements including adding dynamic regression counts instead of always being pinned at 4 regressions, command line inputs, other metrics like R-squared in its outputs, and handling missing data. 

I was incredibly impressed by the suggested code that ChatGPT came up with. The first few chunks that it suggested would have taken some effort to stitch together into a working  script, but when  asked for a full working example script, it came up with a fully operational improved version of my existing code. I copied and pasted the revised code into VS Code and it worked and generated the correct regression results with no edits. I was prepared for the code to have some errors that required changes to get it to run, but this code had no issues. 



# Summary of Experience with AI

I was underwhelmed with GitHub CoPilot and incredibly impressed with ChatGPT. I thought after completing the CoPilot portion of the assignment that AI didn't really have anything to offer the tech startup worried about monthly burn, but I think by using ChatGPT their programmers could really become much more productive. I will say however, that the tasks ChatGPT succeeded in helping me with were relatively simple as I am still a Go newbie. It is possible that programmers at a tech start up are good enough that they would gain significantly less by getting the assistance of ChatGPT.  I would suggest that the company attempt to run a trial period where its programmers attempt similar tests to what I have done in this assignment. Possibly for their next round of development, there is a control group that uses no AI, one group that uses CoPilot, and one that uses ChatGPT. After that round is over, the results of those three groups can be assesed and compared and a more informed decision can be made. If my experience is any indication though, I would guess that the ChatGPT group will come out on top.

Utilized resources: 

https://code.visualstudio.com/docs/copilot/overview - somewhat useful

https://chatgpt.com/share/67311b23-e248-800d-a9d7-5c3420826ac6 - incredibly useful



