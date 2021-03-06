# Ubuntu-Setup-Scripts
Everyone who has tried to mess around with their Ubuntu distro knows the pain of having to reinstall Ubuntu and set it up to their liking again

These are the scripts that I use to set my Ubuntu up as quick as possible. Feel free to fork it and create your own version, and any contributions are more than welcome :)

## Build Status:

Every script is rock stable and runs against https://travis-ci.org to make sure everything works as expected. Note that `3-ML-Build.sh` is not shown here as it takes >2 hours to build TF+Pytorch on the Travis systems from source, and 2 hours is the system limit for free accounts on Travis. You can, however, still see the results of it running [here](https://travis-ci.org/rsnk96/Ubuntu-Setup-Scripts)


| 1-BasicSetUp and 2-GenSoftware | opencvDirectInstall |  ML-Basic            
|-------------------|-------------------|-------------------|
| [![Build1][5]][9] | [![Build2][6]][9] | [![Build3][8]][9] |

[5]: https://travis-matrix-badges.herokuapp.com/repos/rsnk96/Ubuntu-Setup-Scripts/branches/master/5
[6]: https://travis-matrix-badges.herokuapp.com/repos/rsnk96/Ubuntu-Setup-Scripts/branches/master/6
[8]: https://travis-matrix-badges.herokuapp.com/repos/rsnk96/Ubuntu-Setup-Scripts/branches/master/8
[9]: https://travis-ci.org/rsnk96/Ubuntu-Setup-Scripts

## Usage instructions
First download/clone this repository

Run
`chmod u+x *.sh` to make all scripts executable
Then execute them in the terminal in the sequence of filenames.
* `1-BasicSetUp.sh` - Sets up terminal configuration, a download accelerator, anaconda python, and shell aliases.
* `2-GenSoftware.sh` - Sets up tools for programming(editor, etc), and other general purpose software I use
* `3-ML-Build.sh` - Compiles commonly used ML/DL libraries from source, so that it is optimized to run on your computer
* `opencvDirectInstall.sh` - Compiles the latest tag of OpenCV+Contrib from source on your machine with focus on optimization of execution of OpenCV code.
* `ML-Basic.sh` - Installs from pip commonly used DL libraries

### Notes
* Make sure that your system time and date is correct and synchronized before running the scripts, otherwise this will cause failure while trying to download the packages.
* During the `opencvDirectInstall` script, `yasm` package is downloaded while `ffmpeg` is being built. The download link for this package fails to work on certain networks, therefore check the download link as described below before running the script. If it doesn't work, then please switch to a different network. You can switch back to the original network after `yasm` has been downloaded if required

    * If you are on a browser, just open the following page - [http://www.tortall.net/](http://www.tortall.net/)  
      If a webpage opens up, then the download will work.

    * If you only have a terminal access, run this command : `curl -Is www.tortall.net | head -1`  
      Output with Status code `200 OK` means that the request has succeeded and the URL is reachable.
<br><br>

## Major Alterations
* Default python will be changed to Anaconda, with the latest Python 3. Anaconda Python will be installed in `/opt/anaconda3/` so that it is accessible by multiple users
* Default shell is changed to Zim, a zsh plugin, instead of bash. Why zsh? Because it simply has a much better autocomplete. And why zim? Because it's much faster than Oh My Zsh and Prezto

## Aliases that are added
* `maxvol` : Will set your volume to 150%
* `download <webpage-name>`: Download the webpage and all sub-directories linked to it
* `server` : Sets up a server for file sharing in your local network. Whatever is in your current directory will be visible on the ip. It will also print the possible set of IP addresses. To access from another computer, shoot up a browser and simply hit `ip_add:port`
* `gpom` : Alias for `git push origin master`. Will push your current directory
* `jn` : Starts a jupyter notebook in that directory
* `jl` : Starts a jupyter lab in that directory
* `ydl "URL"`: Downloads the song at `URL` at 128kbps, 44.1kHz in m4a format with the title and song name automatically set in the metadata
* `update`: Runs `sudo apt-get update && sudo apt-get dist-upgrade && sudo apt-get autoremove -y`
* `tsux`: Create a tmux session with `-u` (so that the icons(battery, etc) are properly displayed at the bottom), and with a window with `htop`, `nvidia-smi -l 1` and `lm-sensors` automatically activated.
    - Reason for not making this the default tmux: You cannot attach tmux sessions if you alias the `tmux` command itself
* `aria`: For accelerated download of files using aria2c. Runs the following command: `aria2c --file-allocation=none -c -x 10 -s 10 -d aria2-downloads`

<br>

## Programs that are installed
`Tmux`, `Tilda`, `Ubuntu-Restricted-Extras`, `VLC`, `Google Chrome and Firefox`, `Dropbox`, `Gparted`, `Boot-Repair`, `Shutter`,`Grub Customizer`, `Ffmpeg`, `gimp`, `meld` (To be used with `git mergetool`), `aria2`, `tor`, `redshift`, `lm-sensors`, `SimpleScreenRecorder` (might've missed some)  

Text Editors:  `VS Code`, `Sublime Text 3`, `Atom` (any 1)  
Arc themes with Tweak tool for Desktop customization

## Python Packages
* Machine Learning Libraries: Tensorflow built from source, optimized for user hardware. Theano, Keras, OpenAI Gym and Pytorch installed from pip
* OpenCV: Compiled from source, multithreaded and optimized to use your hardware
* Autopep8
* scdl - a soundcloud downloader
* org-e - An app to sort and declutter folders (like ~/Downloads/)
* youtube-dl: A youtube downloader

## Notes
* If you are using this script to set up a computer with many users,
    * You need to run these scripts using only one user, say `first_user`
    * We need to copy the configuration files to the new user, say `new_user`. From `first_user`'s account, run the following
        ```bash
        ln -s /opt/.zsh/zim/ /home/<new_user>/.zim
        cp /opt/.zsh/bash_aliases /home/<new_user>/.bash_aliases
        cp -s /opt/.zsh/zim/templates/zimrc /home/<new_user>/.zimrc
        cp -s /opt/.zsh/zim/templates/zlogin /home/<new_user>/.zlogin
        cp -s /opt/.zsh/zim/templates/zshrc /home/<new_user>/.zshrc

        cp ~/.xbindkeysrc /home/<new_user>
        mkdir -p /home/<new_user>/.config/tilda
        cp ~/.config/tilda/config_0 /home/<new_user>/.config/tilda/config_0

        sudo chown <new_user>: /home/<new_user>/*
        ```
* OpenCV is built to link to an `ffmpeg` that is built from scratch using [Markus' script](https://github.com/markus-perl/ffmpeg-build-script). The `ffmpeg` that is built is stored in `/opt/ffmpeg-build-script`. While the binaries are copied to `/usr/local/bin`, the specific versions of `libavcodec` and other referenced libraries are still maintained at `/opt/ffmpeg-build-script/workspace/lib`
* If you have Anaconda Python, OpenCV will be linked to Anaconda Python by default, not the Linux default python. If you would like to compile for the Linux default Python, remove Anaconda from your path before running the `opencvDirectInstall.sh` script
* If you would like to install with OpenCV for CUDA, change the flags `-D WITH_NVCUVID=0`, `-D WITH_CUDA=0`, `-D WITH_CUBLAS=0`, `-D WITH_CUFFT`,`-D CUDA_FAST_MATH` in the file `opencvDirectInstall.sh` to `ON`
* After OpenCV installation, if you get an error of the sort `illegal hardware instructions` when you try to run a python or c++ program, that is because your CPU is an older one (Pentium/Celeron/...). You can overcome this by adding the following to the end of the cmake (just before the `..`)

  ```bash
   -D ENABLE_SSE=OFF \
   -D ENABLE_SSE2=OFF \
   -D ENABLE_SSE3=OFF ..
  ```

  If you still want to be able to receive the benefits of CPU optimization to whatever extent you can, then hit `cat /proc/cpuinfo` and see what `sse`s are available under flags
* If you want to install a specific version of OpenCV or Tensorflow, i.e different from the latest release, make the following changes. The scripts should work with different versions but they haven't been tested
  * OpenCV   
  Comment out the line fetching the [latest release tag](https://github.com/rsnk96/Ubuntu-Setup-Scripts/blob/master/opencvDirectInstall.sh#L158) in the `opencvDirectInstall` script.  
  Add the line below the above commented out one specifying the OpenCV version which you want like this: `latest_tag="3.4.5"`  
  Alternatively, you could just replace `$latest_tag` with the tag of the version in the following 2 lines: `git checkout -f $latest_tag`  
  Make sure that the tag of the OpenCV version you want is correct. The tags of all the releases can be checked here - [https://github.com/opencv/opencv/tags](https://github.com/opencv/opencv/tags) 

  * Tensorflow  
  Similar to above, locate the line fetching the [latest release tag](https://github.com/rsnk96/Ubuntu-Setup-Scripts/blob/master/3-ML-Build.sh#L114) of Tensorflow and replace with the tag of the version required.  
  The tags of all the Tensorflow releases can be checked here - [https://github.com/tensorflow/tensorflow/tags](https://github.com/tensorflow/tensorflow/tags)
* These scripts are written and tested on the following configurations - 
  * Ubuntu 16.04 & 18.04
  * 32-bit and 64-bit Intel Processors
  * `ML-Build.sh` - NVIDIA GPUs including but not limited to GeForce GTX 1080, 1070, 940MX, 850M, and Titan X
  
  Although it should work on other configurations out of the box, I have not tested them


## Alternatives
* A [docker image](https://hub.docker.com/r/varun19299/cvi-iitm/) for this set-up (last updated Jan 30th, 2017)
* A Ubuntu customization dedicated to [robotics](https://github.com/ahundt/robotics_setup)

## To Dos 
- [x] CI
- [ ] configuring default wifi settings
