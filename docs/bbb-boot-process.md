### Beaglebone Black (BBB) Boot Process

http://dumb-looks-free.blogspot.tw/2014/05/beaglebone-black-bbb-boot-process.html


_Stage 1_

- The ROM loads. This is on board read only and can't change.  It looks for the MLO file and runs it.

![rom-boot](http://api.ning.com/files/Ug6ORo4NSNSvcaF*3l6f9D47KVxHFY-e7bluI9gjfrCZR7Hx7P9f7E8TLZw6IHoygGZ69DccjANzxmWjlhNRzgkwY9CtCZ1E/ScreenShot20140620at22.13.51.png)

_Stage 2 (x-loader)_

- MLO file runs and looks for Zimage

_Stage 3 (u-boot)_

- Zimage loads & run with configuration uEnv.txt
- uEnv.txt files have info on where to find the linux kernal plus lots of other parameters
- Parameters can be passed from the uEnv.txt file into Linux kernal (provided the kernal was complied with the required modules for the parameters)

_Stage 4_

- Linux Kernal loads

_Stage 5_

- Root file system loads (e.g. debian, ubuntu, etc.)


[Booting up a BeagleBone Black](http://diydrones.com/profiles/blogs/booting-up-a-beaglebone-black)

