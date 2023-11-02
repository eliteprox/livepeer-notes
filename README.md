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
