## Week 7 Assignment - AI Assisited Programming

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

While the comments provided on my code are helpful and the suggested code handles many of those comments by itself, the suggested code leaves out large chunks of code that is needed for the main code to function. I leaves out the code used to read the CSV data and add it into the regression models for evaluation. There are specific place holders built into the suggested code for the code to accomplish those tasks though. Still, that issue and problems such as the suggested code simplifying things such as the final print statement in undesireable ways means the suggested code would need to be fixed and modified before usage.


# Chat GPT




# Summary of Experience with AI





