flashrd
=======

# flashrd installation

It's pretty simple. Using the prepare script to fetch and unpack all nessessary files.

## Step 1: 

    ./prepare 

All files from the spezific distribution will be downloaded, unpacked, patched and a sandbox will be created.
The single steps are

* download base, man, comp, src and sys
* check if valid
* unpack and copy to the right place

## Step 2: Build binaries for OpenBSD from source

Generate a new userland makes building an own flash much easier.

    cd /usr/src
    rm -Rf /usr/obj/*
    make obj
    cd /usr/src/etc && env DESTDIR=/ make distrib-dirs
    cd /usr/src
    make depend
    make

## Step 3: Get source tree

Running flashrd to finish an image or also copy the image to device. With the prepare script we have an sandbox with all files we need.

    # this writes the image directly to den sd0 device
    ./flashrd -d sd0 ./sandbox/
    # only generating an image flashimg.ARCH-DATE
    ./flashrd ./sandbox/
    
    # a very important step is configuring the serial console
    # otherwise you can not connect serial
    # you will also be asked for hostname, dns and password
    # configure directly the installation on the device
    ./cflashrd -d sd0 -c 38400
    
    # OR modify the image from before
    ./cflashrd -i flashimg.ARCH-DATE -c 38400
    
## Step 3: Writing flash to device if not done before

This step is for flashing the image to one or many devices. It finds the size of the device, expands the image and writes it down.

    ./growimg -t sd0 flashimg.ARCH-DATE
