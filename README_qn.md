# 七牛定制化开发

## 需求目标

 - 拉取远端 HTTP-FLV 流做为源，并提供 HTTP Server 为其它端提供拉取能力，协议支持 HTTP-FLV、RTMP 等。
 - 对 SRS 进行裁剪，满足最低功能需求的前提下，提供最小的包体。
 - 方便业务代码集成接入。

## 裁剪记录

裁剪分两步进行，首先基于当前 SRS 提供的模块化编译能力进行基础功能模块的定制，然后再通过代码的删减实现最优的包体裁剪。

### 编译命令

srs 官方提供的Linux Docker Image 中直接运行的 srs 二进制大小为 34MB，在 Mac 下使用默认编译命令 `./configure` 生成的 `srs` 大小为 25MB。

通过以下参数进行编译:

```
$ cd trunk
$ ./configure --static=on --backtrace=off --sanitizer=off --srt=off --rtc=off --ffmpeg=off --nasm=on --srtp-nasm=off  --osx=off --ffmpeg-fit=off --gperf=on
make
```

编译完成后，二进制文件大小为 10MB。

```
$ ls -lh objs/srs
-rwxr-xr-x  1 eddie  staff    10M Dec 27 11:01 objs/srs

$ otool -L objs/srs
objs/srs:
	/usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 1319.0.0)
	/usr/lib/libc++.1.dylib (compatibility version 1.0.0, current version 1300.36.0)
 ```