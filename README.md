# rpi-nano-httpd

[![dockeri.co](http://dockeri.co/image/hypriot/rpi-nano-httpd)](https://registry.hub.docker.com/u/hypriot/rpi-nano-httpd/)

A nano sized web server packed into a Docker Nano Container


## Step 1: Compile the assembly source code

Compiling the ASM source to a statically linked ARM binary for Raspberry Pi.

The source can be found in `src/` folder. You need a x86/amd64 Linux machine to compile the source assembly code with FASM (can be downloaded from http://arm.flatassembler.net). You only need the statically compiled `fasmarm` binary. I've done it within a Ubuntu 14.04 (64bit) machine running as a Docker Container.

Compile the thing just with `./fasmarm httpd.fasm httpd`
```
cd ./src/
docker run --rm -ti -v $(pwd):/src ubuntu:14.04 bash -c 'cd /src && ./fasmarm httpd.fasm httpd'
flat assembler for ARM  version 1.71.39  (16384 kilobytes memory)
3 passes, 4328 bytes.
```
Now we've got a super small httpd binary with 4kByte only
```
ls -al httpd
-rwxr-xr-x  1 dieter  staff  4328 Jun 28 16:27 httpd
```
And hell yeah, it's statically linked
```
file httpd
httpd: ELF 32-bit LSB executable, ARM, version 1 (SYSV), statically linked, stripped
```

Let's copying it to `./docker/` folder
```
cp httpd ../docker/
```

## Step 2: Create the Docker Nano Image
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
Please note, the size of the Docker Image includes the payload too! Without the payload the image size would be 4kByte only! Ok, that's not `nano` anymore, that's more `pico` sized.

## Step 3: Now let's run a lot of these containers on our Raspberry Pi
To start 10 web servers use the following command
```
./start-webservers.sh 10
```
To ramp up to 100 web servers use the following command
```
./start-webservers.sh 100 10
```
That's it, have fun.


# Acknoledgements

## httpd, original source code
https://www.raspberrypi.org/forums/viewtopic.php?p=320919

I was so happy to find this small piece of source code after hours. It's a minimal web server or httpd written in assembly language to run on an ARM cpu. I did only a minor change and use `index.html` as the default resource to look for.
As the source code is written for FASM you also need to download this tool too.

## FASMARM: Freeware ARM cross assembler for FASM
http://arm.flatassembler.net

Just download the package for a Linux distro and extract the tool `fasmarm` which is a statically linked binary and doesn't need any additional dependencies installed.
