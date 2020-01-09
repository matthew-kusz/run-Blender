Bootstrap: docker
From: ubuntu:18.04

%post

# We need gnupg to setup the PPA
apt-get update
apt-get install -y gnupg clinfo

# We will use a ppa to get the latest blender
echo "deb http://ppa.launchpad.net/thomas-schiex/blender/ubuntu bionic main" >> /etc/apt/sources.list
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys D32A3245446233723DECE00F7281E3E842A98114

# Install blender
apt-get update
apt-get install -y x11-utils
apt-get -y install alsa-utils
apt-get -y install blender

%hel
$ singularity run blender.sif [scene file] [output directory] <frame | start frame:end frame>

Example:
Using `my_scene.blend`, render all frames, and output into `run/output`

    $ singularity run blender.sif my_scene.blend run/output

Using `my_scene.blend`, render frames 100->200, and output into `run/output`

    $ singularity run blender.sif my_scene.blend run/output 100:200

Using `my_scene.blend`, render frame 5, and output into `run/output`

    $ singularity run blender.sif my_scene.blend run/output 5

%runscript
FRAME="-a"
if [ -n "$3" ]; then
    if echo $3 | grep -q ":"; then
        STARTF=$(echo $3 | cut -f 1 -d ':')
        ENDF=$(echo $3 | cut -f 2 -d ':')

        FRAME="-s ${STARTF} -e ${ENDF} -a"
    else
        FRAME="-f $3"
    fi
fi

echo "Command to run is: /usr/bin/blender -b -noaudio $1 -o $2 ${FRAME}"
/usr/bin/blender -b -noaudio $1 -o $2 ${FRAME}
