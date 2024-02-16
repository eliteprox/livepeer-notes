# livepeer-notes
A wiki for livepeer development notes


#### Clean FFMPEG installation

```
# Clean the cached folders from prior builds 
rm -rf ~/compiled && rm -rf ~/ffmpeg && rm -rf ~/nasm-2.14.02 && rm -rf ~/nv-codec-headers && rm -rf ~/x264 && rm -rf ~/compiled

# Set up the library config for cuda linking
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64
export PATH=$PATH:/usr/local/cuda/bin

#Install FFMPEG
./install_ffmpeg.sh
```

#### Sample launch.json config for debugging with visual studio code
The `PKG_CONFIG_PATH` and `LD_LIBRARY_PATH` must be full paths, they cannot reference home like `~/`
```
        {
            "name": "Launch Transcoder",
            "type": "go",
            "request": "launch",
            "mode": "auto",
            "program": "${workspaceFolder}/cmd/livepeer",
            "buildFlags":"-tags=mainnet,experimental",
            "env": {"GO111MODULE":"on", "CGO_ENABLED":"1", "CC":"", "CGO_LDFLAGS":"-L/usr/local/cuda/lib64 -L/home/elite/compiled/lib","PATH: ": "/usr/local/cuda/bin:${PATH}", "PKG_CONFIG_PATH":"/home/elite/compiled/lib/pkgconfig", "LD_LIBRARY_PATH":"/home/elite/compiled/lib"},
            "args": [
                "-transcoder",
                "-nvidia",
                "0",
                "-orchAddr",
                "https://127.0.0.1:8935",
                "-orchSecret",
                "1234",
                "-cliAddr",
                "127.0.0.1:7926",
                "-maxSessions",
                "5",
                "-dataDir",
                "/livepeer/arbitrum-one-mainnet/transcoder",
                "-v",
                "7",
            ],
        },
```
