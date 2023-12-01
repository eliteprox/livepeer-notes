### Running go-livepeer tests in VS Code
1. From VSCode, click the "gear" icon to access settings.
2. Search for `go.testEnvVars`
3. Click "Edit in settings.json"
4. Add the following variables to your config, updating the paths to match your compiled ffmpeg and cuda libraries:

```
"go.testEnvVars": {
        "GO111MODULE": "on",
        "CGO_ENABLED": "1",
        "CC": "",
        "CGO_LDFLAGS": "-L/usr/local/cuda/lib64 -L/home/elite/compiled/lib",
        "PATH": "/usr/local/cuda/bin:/usr/bin/:${PATH}", 
        "PKG_CONFIG_PATH":"/home/elite/compiled/lib/pkgconfig", 
        "LD_LIBRARY_PATH":"/home/elite/compiled/lib"
    }
```
If you are missing gcc compiler, you can install it with 
`sudo apt install gcc`

In case of any other missing path entries, append those with `:` above.
