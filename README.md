flashrd
=======

# flashrd installation

It's pretty simple. do a make build, or unpack the openbsd baseXX.tgz, etcXX.tgz, and manXX.tgz (minimum recommended) into a directory. (Feel free to omit any sets that aren't necessary, but baseXX.tgz and etcXX.tgz are the minimum required.)

Please note that invocations of flashrd must be made on the same architecture that the image is destined for (you must be on i386 to create an image for i386). This limitation is due to the compiler. You can fix mkkern to use a cross-compiler, if you like. The cfgflashrd tool can configure images from other architectures, no such limitation exists there.
## Step 1: Unpack OpenBSD


    mkdir /tmp/openbsd
    cd /tmp/openbsd
    tar xzpf ~/baseXX.tgz
    tar xzpf ~/etcXX.tgz
    tar xzpf ~/manXX.tgz
    tar xzpf ~/miscXX.tgz
    tar xzpf ~/compXX.tgz
    tar xzpf ~/gameXX.tgz 

(Alternately, you could 'make build' after getting the source tree in Step 2)
## Step 2: Get source tree

Make sure you have a source tree for the same system version at /usr/src. If you don't, get a new one. (flashrd compiles the kernel, plus certain specific commands with crunchgen)

    cd /usr
    cvs -d anoncvs@anoncvs.openbsd.org:/cvs -q get -rOPENBSD_X_X src 

(If you are building against a snapshot, you would not specfiy a tag with -r)
## Step 3: Run Flashrd

Then run flashrd against it.

    cd ~
    tar xzf flashrd-X.Y.tar.gz
    cd flashrd-X.Y
    ./flashrd /tmp/openbsd 

You'll end up with a disk image that you can write to flash. Follow the image installation instructions for further guidance.
## alternate Step 3:

flashrd can also write directly to a disk. Whatever geometry the device shows up as will be used for the disklabel this way. This tends to be a bit slower than first creating a disk image and transferring it with growimg (the above instructions).

    cd ~
    tar xzf flashrd-X.Y.tar.gz
    cd flashrd-X.Y
    ./flashrd -disk sd2 /tmp/openbsd
    ./cfgflashrd -disk sd2 
