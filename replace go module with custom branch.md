
### go mod replace trick
Useful when you need local system to map to a forked go module

1. Create a new branch of the go module project.
2. Push branch
3. From project that needs the go-module, run `go get github.com/user/reponame@custombranch`. For example: `go get github.com/eliteprox/go-tools@develop-s3-public`
4. From project that needs the go-module, open the `go.mod` file.
5. Locate the module you need to customize, take note of it's version number.
6. From the project that needs the module, run `go mod edit -replace="github.com/vendor/reponame@tagversion=github.com/user/reponame@tagversion`
- For Example: `go mod edit -replace="github.com/livepeer/go-tools@v0.0.0-20220805063103-76df6beb6506=github.com/eliteprox/go-tools@v0.0.0-20240106004016-da414bdab33c`

`go mod edit -replace="github.com/livepeer/lpms@v0.0.0-20231002131146-663c62246a3c=github.com/eliteprox/lpms@v0.0.0-20231002131146-663c62246a3c"`
