# Installing NETINT
This guide covers the basics for how to compile netint support for `go-livepeer`

#### Prerequisites
You will need to be on the 3.1 firmware as well as use the 3.1.0 software release (there might be a newer version since, but this is the first one to support quadra and had breaking changes from prior 2.x versions)
Drivers https://drive.google.com/drive/folders/1UEJ5dpz5uUUD7EmwyQvAq5wRPA8VJXtA

#### Extract drivers and pull livepeer netint code
```
tar -zxvf Codensity_T4XX_Software_Release_V3.1.0.tar.gz
cd ~/ && git clone https://github.com/livepeer/go-livepeer.git livepeer-netint && cd livepeer-netint
git checkout f66717fc7aeca11579aac04e6f09ab735711f770
#NOTE: This is a clean patch of 4.4 which is missing livepeer signature functions...
```


#### Clean and Copy libxcoder to the proper output folder
```
export ROOT=~/buildoutput
rm -rf $ROOT
mkdir $ROOT && cp -r ~/release/libxcoder_logan $ROOT/libxcoder_logan
```

#### Set environment vars
```
export LD_LIBRARY_PATH="$ROOT/compiled/lib:/usr/local/cuda/lib64:$LD_LIBRARY_PATH"
export PKG_CONFIG_PATH="$ROOT/compiled/lib/pkgconfig:$PKG_CONFIG_PATH"
export PATH="$ROOT/compiled/bin:/usr/local/cuda/bin:$PATH"
```

#### Build ffmpeg
`BUILD_TAGS="experimental debug-video" ./install_ffmpeg.sh $ROOT`

#### Test netint codecs are listed for both h264 and hevc
`ffmpeg -codecs|grep logan` #TODO ensure this points to the correct path

#### build and run bench
`go build cmd/livepeer_bench/livepeer_bench.go`

For building a production release, you will need to set the build tag of mainnet
`go build -tags mainnet cmd/livepeer/livepeer.go`

#### Build go-livepeer
```
go build -ldflags="-X github.com/livepeer/go-livepeer/core.LivepeerVersion=0.7.2-alpha" -tags mainnet cmd/livepeer/livepeer.go

TOOD - New build command? GO111MODULE=on CGO_ENABLED=1 CC="" CGO_CFLAGS="" CGO_LDFLAGS=" -L/usr/local/cuda/lib64" go build -o "./" -tags "" -ldflags="-X github.com/livepeer/go-livepeer/core.LivepeerVersion=0.7.1-2823d1cf-dirty" cmd/livepeer/*.go
```

### Runtime Steps
#### Install pre-requisites
`sudo apt-get install -y nvme-cli`

To run FFmpeg without root privilege, the user needs to be added to the user ‘disk’
group:
```
sudo usermod -a -G disk <user>
sudo usermod -a -G disk elitencoder #for end user
sudo usermod -a -G disk livepeer #for service account
```
Reboot system or start new bash shell for new group membership to take

#### Initialize netint devices
You must set LD_LIBRARY_PATH and initialize the NETINT cards first:
```
export ROOT=~/buildoutput
export LD_LIBRARY_PATH="$ROOT/compiled/lib"
export PATH=$ROOT/compiled/bin/:$PATH

$ROOT/compiled/bin/./init_rsrc_logan
```
#### Monitor netint device load
`ni_rsrc_mon_logan`

You can also test ffmpeg (set the path to netint ffmpeg first)
```
export PATH=$ROOT/compiled/bin/:$PATH
ffmpeg -y -c:v h264_ni_logan_dec -i bbb.mp4 -c:v  h265_ni_logan_enc bbb_265_ni.mp4
```

Run and test
`./livepeer_bench  -in bbb/source.m3u8 -transcodingOptions P240p30fps16x9 -concurrentSessions 5 -outPrefix /tmp/ -netint all`

### ffmpeg dev notes
Get information about the encoder/decoder supported parameters

x264
```
ffmpeg -help encoder=h264_ni_logan_en
ffmpeg -help decoder=h264_ni_logan_dec
```

x265
```
ffmpeg -help encoder=h265_ni_logan_en
ffmpeg -help decoder=h265_ni_logan_dec
```
