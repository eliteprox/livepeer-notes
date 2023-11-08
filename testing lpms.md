### Developing LPMS for NETINT
The following `launch.json` can be used with LPMS to develop/debug libxcoder flags for the netint ffmpeg encoders/decoders


```
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Launch LPMS test",
            "type": "go",
            "request": "launch",
            "mode": "auto",
            "program": "${workspaceFolder}/cmd/transcoding/transcoding.go",
            "buildFlags":"-tags=mainnet,experimental",
            "env": {"CGO_LDFLAGS":"-L/usr/local/cuda/lib64 -L/home/elite/buildoutput/compiled/lib -Wl,--copy-dt-needed-entries", "PATH: ": "/home/elite/buildoutput/compiled/bin:${PATH}", "PKG_CONFIG_PATH":"/home/elite/buildoutput/compiled/lib/pkgconfig", "LD_LIBRARY_PATH":"/home/elite/buildoutput/compiled/lib"},
            "args":[
                "transcoder/test.ts",
                "P144p30fps16x9,P240p30fps16x9",
                "nt 0"
            ]
        }
    ]
}
```
