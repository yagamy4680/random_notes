The experiment leaverages Dell760 workstations and Intel NUC boxes with Docker cluster to measure the compilation performance improvements by [distcc](https://code.google.com/p/distcc/).

The target compilation item is opencv 2.4.8. Here is the script to download and build opencv 2.4.8:
```bash
wget -O 2.4.8.zip http://downloads.sourceforge.net/project/opencvlibrary/opencv-unix/2.4.8/opencv-2.4.8.zip?r=http%3A%2F%2Fopencv.org%2Fdownloads.html
unzip 2.4.8.zip
cd opencv-2.4.8
mkdir release
cd release
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..
time make -j$(distcc -j)
```

### Experiment Results
#### Cores and Performance
![](https://doc-0g-ac-docs.googleusercontent.com/docs/securesc/ha0ro937gcuc7l7deffksulhg5h7mbp1/qd8is9fp0k6rc8d65jh7p77auaaum28v/1395223200000/16336832464913243550/*/0ByNrSvgz6ePDMFBHUFlOWFAwVWM?h=16653014193614665626&e=download)

| experiment | cpu cores | distcc -j | build command | build time (s) |
|--------|--------|--------|--------|--------|--------|
| `nuc_vagrant_precise64_nodistcc_j1` | 1 | n/a | make -j1 | 872 |
| `nuc_vagrant_precise64_nodistcc_j4` | 4 | n/a | make -j4 | 474 |
| `nuc_vagrant_precise64_distcc_localhost_dell760x1_j6` | 8 | 6 | make -j6 | 306 |
| `nuc_vagrant_precise64_distcc_localhost_dell760x2_j10` | 12 | 10 | make -j10 | 233 |
| `nuc_vagrant_precise64_distcc_localhost_dell760x3_j14` | 16 | 14 | make -j14 | 190 |

Of course the more CPU cores, the less build time. 

For offline (standalone) development environment (e.g. on your mac book pro), it's always suggested to build OpenCV 2.4.8 with `make -j$(nproc)` to utilize all CPU cores.

For online development environment such as T2T Mani office, it's suggested to install `distcc` and run following commands to find available distcc servers inside T2T office:

```bash
git clone https://github.com/yagamy4680/it4t2t.git
./it4t2t/dockers/distcc-gcc46-x86/find_members.sh

# typically, the find_members.sh will output following information, that show you the 
# distcc daemon is running on which server and which public port number.
#
download SERF from its website ...
looking for cluster members for <distcc-gcc46-x86> ...
type following commands to build:

  export DISTCC_HOSTS="localhost 10.20.0.20:49155 10.20.0.101:49153 10.20.0.21:49160"
  make -j$(distcc -j)
```

#### Others
![](https://doc-0c-ac-docs.googleusercontent.com/docs/securesc/ha0ro937gcuc7l7deffksulhg5h7mbp1/kljlff3pmk6sprgf7t61hbortp8o2if4/1395230400000/16336832464913243550/*/0ByNrSvgz6ePDXzlwdHB3T3dOX0k?h=16653014193614665626&e=download)

Some obvserations:
  1. (a) v.s. (d), vmware fusion is 21% faster than virtualbox on Mac OS X 10.8.5.
  2. (b) v.s. (d), distcc with only localhost brings ~9% overheads.
  3. (c) v.s. (d), Mac Book Pro (2013) is ~7% faster than Intel NUC 54250.



### Experiment Environment Setup

#### DISTCC Client Machine

Before compiling opencv, the distcc client machine (that downloads opencv and build) needs to install distcc3.2rc1 and make symbolic links:

```bash
apt-get update
apt-get install -y build-essential vim unzip cmake subversion autoconf 
apt-get install -y binutils python-dev
apt-get install -y python-software-properties
add-apt-repository -y ppa:kubuntu-ppa/backports
apt-get update
apt-get install -y libiberty-dev
cd /opt
svn checkout http://distcc.googlecode.com/svn/trunk/ distcc-read-only
cd distcc-read-only/
./autogen.sh
./configure
make
make install
distccd --allow 0.0.0.0/0 --listen 0.0.0.0 --user nobody --verbose 
mkdir -p /usr/lib/distcc/
ln -s $(which distcc) /usr/lib/distcc/c++
ln -s $(which distcc) /usr/lib/distcc/c89-gcc
ln -s $(which distcc) /usr/lib/distcc/c99-gcc
ln -s $(which distcc) /usr/lib/distcc/cc
ln -s $(which distcc) /usr/lib/distcc/g++
ln -s $(which distcc) /usr/lib/distcc/g++-4.8
ln -s $(which distcc) /usr/lib/distcc/gcc
ln -s $(which distcc) /usr/lib/distcc/gcc-4.8
ln -s $(which distcc) /usr/lib/distcc/x86_64-linux-gnu-g++
ln -s $(which distcc) /usr/lib/distcc/x86_64-linux-gnu-g++-4.8
ln -s $(which distcc) /usr/lib/distcc/x86_64-linux-gnu-gcc
ln -s $(which distcc) /usr/lib/distcc/x86_64-linux-gnu-gcc-4.8
```

Note, the client machine is also treated as one distcc daemon for distributing compilation tasks.

#### DISTCC Server Machine

Each distcc server (running `distccd`) is based on Docker. Please refer to `it4t2t/dockers/distcc-gcc46-x86/Dockerfile`.

There are 5 distcc servers (including the client machine):

| Server | Core(s) | CPU Spec | # of servers |
|--------|----------|----------|-------------|
| Mac Boo Pro 2013 |  4 cores | Intel(R) Core(TM) i7-3740QM CPU @ 2.70GHz | 1 |
| Intel NUC 54250 | 4 cores | Intel(R) Core(TM) i5-4250U CPU @ 1.30GHz | 1 |
| Dell OptiPlex760 | 4 cores | Intel(R) Core(TM)2 Quad CPU    Q9400  @ 2.66GHz | 3 |


### Experiment Result Raw Data

```text
mbp (virtualbox + precise64 + 4 cores), make -j1
    mbp_vagrant_precise64_nodistcc_j1
    real    14m20.253s
    user    12m58.209s
    sys 1m3.644s

nuc (virtualbox + precise64 + 4 cores), make -j1
    nuc_vagrant_precise64_nodistcc_j1
    real    14m32.375s
    user    13m22.222s
    sys 0m55.555s

mbp (virtualbox + precise64 + 4 cores), make -j4
    mbp_vagrant_precise64_nodistcc_j4
    real    7m20.206s
    user    19m1.347s
    sys 3m31.005s

nuc (virtualbox + precise64 + 4 cores), make -j4
    nuc_vagrant_precise64_nodistcc_j4
    real    7m54.423s
    user    26m23.895s
    sys 2m29.269s

----
mbp (virtualbox + precise64 + 4 cores), make -j8
    mbp_vagrant_precise64_nodistcc_j8
    real    9m36.996s
    user    20m45.946s
    sys 7m13.111s

mbp (virtualbox + precise64 + 4 cores), distcc=localhost, make -j4
    mbp_vagrant_precise64_distcc_localhost_j4
    real    8m0.673s
    user    3m0.115s
    sys 1m5.592s

mbp (vmware + precise64 + 4 cores), make -j1
    mbp_vagrant_precise64_distcc_localhost_j1
    real    13m37.473s
    user    12m11.724s
    sys 1m4.612s

mbp (vmware + precise64 + 4 cores), make -j4
    mbp_vmware_precise64_nodistcc_j4
    real    6m1.698s
    user    19m44.412s
    sys 2m12.128s

nuc + mbp, make -j6
    nuc_vagrant_precise64_distcc_localhost_mbp_j6
    real    5m39.099s
    user    11m0.977s
    sys 1m31.490s

nuc + mbp, make -j12 (double)
    nuc_vagrant_precise64_distcc_localhost_mbp_j12
    real    5m1.410s
    user    10m17.487s
    sys 1m30.182s

nuc + dell760, make -j$(distcc -j)  => j6
    nuc_vagrant_precise64_distcc_localhost_dell760_j6
    real    5m6.708s
    user    10m27.439s
    sys 1m31.414s

nuc + dell760 + dell760, make -j$(distcc -j)  => j10
    nuc_vagrant_precise64_distcc_localhost_dell760_dell760_j10
    real    3m53.449s
    user    8m32.268s
    sys 1m38.550s

nuc + dell760 + dell760 + dell760, make -j$(distcc -j)  => j14
    nuc_vagrant_precise64_distcc_localhost_dell760_dell760_dell760_j14
    real    3m10.543s
    user    7m23.588s
    sys 1m29.686s

    nuc_vagrant_precise64_distcc_localhost_dell760_dell760_dell760_j16
    real    3m6.913s
    user    7m18.979s
    sys 1m30.746s

nuc + dell760 + dell760, make -j24
    nuc_vagrant_precise64_distcc_localhost_dell760_dell760_j20
    real    3m36.921s
    user    7m57.438s
    sys 1m32.058s
```

Build Server Spec
```text
MBP + VirtualBox-4.3.8
    model name      : Intel(R) Core(TM) i7-3740QM CPU @ 2.70GHz
    cpu MHz         : 2698.714
    cpu cores       : 2

NUC54250 + VirtualBox-4.3.8
    model name      : Intel(R) Core(TM) i5-4250U CPU @ 1.30GHz
    cpu MHz         : 1890.565
    cache size      : 6144 KB
    cpu cores       : 4

Dell760 + PlainUbuntu
    model name      : Intel(R) Core(TM)2 Quad CPU    Q9400  @ 2.66GHz
    cpu MHz         : 2000.000
    cpu cores       : 4
```

### Reference

[Using DistCC to speed up compilation](http://pointclouds.org/documentation/advanced/distcc.php)
![](http://pointclouds.org/documentation/advanced/_images/distcc_plot.png)