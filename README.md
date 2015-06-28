# rpi-nano-httpd
A nano sized web server packed into a Docker Nano Container


## Step 1: Compile the assembly source code

Compiling the ASM source to a statically linked ARM binary for Raspberry Pi.

The source can be found in `src/` folder. You need a x86/amd64 Linux machine to compile the source assembly code with FASM (downloaded from http://arm.flatassembler.net). You only need the statically compiled `fasmarm` binary. I've done it just with a Ubuntu 14.04 (64bit) machine as a Docker Image.

Compile the thing: just with `./fasmarm httpd.fasm httpd`
```
cd ./src/
docker run --rm -ti -v $(pwd):/src ubuntu:14.04 bash -c 'cd /src && ./fasmarm httpd.fasm httpd'
flat assembler for ARM  version 1.71.39  (16384 kilobytes memory)
3 passes, 4328 bytes.
```
```
ls -al httpd
-rwxr-xr-x  1 dieter  staff  4328 Jun 28 16:27 httpd
```
```
file httpd
httpd: ELF 32-bit LSB executable, ARM, version 1 (SYSV), statically linked, stripped
```

And now copy it to `./docker/` folder.
```
cp httpd ../docker/
```

## Step 2: Create a Docker Image
```
cd ./docker/
make
```

Now we do have a ready-to-run Docker Image with a single statically linked ARM binary for use on a Raspberry Pi.
```
docker images
REPOSITORY               TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
hypriot/rpi-nano-httpd   0.1.0               0fd6d79d7479        15 minutes ago      87.7 kB
hypriot/rpi-nano-httpd   latest              0fd6d79d7479        15 minutes ago      87.7 kB
```
Please note, the size of the Docker Image includes the payload too!