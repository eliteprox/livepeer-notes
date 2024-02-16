Useful when you need the local system to map to a go module you've forked (like lpms or go-tools)

1. Run the following with your branch name instead of master, like `go get github.com/eliteprox/lpms@master`
  Example Output: `github.com/livepeer/lpms@v0.0.0-20231002131146-663c62246a3c`

2. From the parent project Open the `go.mod` file and locate the reference to the original module that you want to replace, like `github.com/livepeer/lpms`. Use the output from above combined with the original module version to write: `go mod edit -replace="github.com/livepeer/lpms@v0.0.0-20231002131146-663c62246a3c=github.com/eliteprox/lpms@v0.0.0-20231002131146-663c62246a3c"`
