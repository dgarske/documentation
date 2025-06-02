## Building wolfCLU

### Building on *NIX
To build wolfCLU, start by building wolfSSL with the --enable-wolfclu flag. An example of this would be:

```
cd wolfssl
./configure --enable-wolfclu
make
sudo make install
```
    

Note that if parsing PKCS12 files with RC2 or if using CRL the flags --enable-rc2 and --enable-crl would also need to be used when building wolfSSL.


Then build wolfCLU linking agianst the wolfSSL library created.

```
cd wolfclu
./configure
make
sudo make install

or

cd wolfclu
./configure --with-wolfssl=/path/to/wolfssl/install
make
sudo make install
```

Run `make check` to run unit tests.

## Building on Windows

wolfCLU can also be built with the appropriate Visual Studio solution, wolfclu.sln. The solution provides both Debug and Release builds of Dynamic 32- or 64-bit libraries. The file `user_settings.h` should be used in the wolfSSL build to configure it.

The file `wolfclu\ide\winvs\user_settings.h` contains the settings used to configure wolfSSL with the appropriate settings. This file must be copied from the directory `wolfclu\ide\winvs` to `wolfssl\IDE\WIN`. You can then build wolfSSL with support for wolfCLU. 

Before building wolfCLU, make sure you have the same architecture (Win32 or x64) selected as used in wolfSSL.

This project assumes that the wolfSSH and wolfSSL source directories
are installed side-by-side and do not have the version number in their
names:
```
    Projects\
        wolfclu\
        wolfssl\
```

Building a wolfCLU release configuration will generate `wolfssl.exe` in the 
`Release\Win32` or `Release\x64` directory. 

#### Running Unit Tests

To run the shell-script unit tests, you will need either the `sh` or `bash`
commands, both of which come with a Git installation on Windows (although you
may have to add them to the PATH). 

1. Copy the `wolfssl.exe` to the root directory of wolfclu.
2. Modify the `run` function (as well as `run_fail`, if present) of the desired 
unit test to run `./wolfssl.exe $1` instead of `./wolfssl $1`.
3. In your terminal, run `sh <desired_unit_test>` from the root directory. For 
instance, to run the hash unit tests, run `sh tests\hash\hash-test.sh`.
