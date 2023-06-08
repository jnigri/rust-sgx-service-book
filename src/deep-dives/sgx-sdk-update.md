# SGX SDK Update

If you follow the instructions in this book, after some time, you might get an error like this one:

```
SDK version is not correct. The same SDK should be used for enclave building and signing.
The SDK version for building enclave could be obtained by below command:

           $ strings {Enclave.so} | grep SGX_TSTDC_VERSION 

Error happened while signing the enclave.
make: *** [Makefile:153: bin/enclave.signed.so] Error 255
```

As [this github issue](https://github.com/apache/incubator-teaclave-sgx-sdk/issues/387) suggests, this is due to the Teaclave SGX SDK being updated to a more recent version. This requires you to update the SGX SDK to the same version. To do so, you need:

1. The version of the SGX SDK that Teaclave is using. You can find this by looking into the Dockerfiles, or in [the commit logs](https://github.com/apache/incubator-teaclave-sgx-sdk/commit/6309bca65ee80b2f6879cbaae1d8082bb8cb821d#diff-3118d6b85963430da57eb38871d66aa4d52f7f8e03c32e4c8181e75d6d2aa5c2).
2. The linux distribution and version. On ubuntu you can run `lsb_release -a`.

Then you will need to find the corresponding link of the SDK here: [https://download.01.org/intel-sgx/sgx-linux](https://download.01.org/intel-sgx/sgx-linux).

For example, for ubuntu 20.04 (focal) and the v2.17 of the sdk, you will need to download this binary: [https://download.01.org/intel-sgx/sgx-linux/2.17/distro/ubuntu20.04-server/sgx_linux_x64_sdk_2.17.100.3.bin](https://download.01.org/intel-sgx/sgx-linux/2.17/distro/ubuntu20.04-server/sgx_linux_x64_sdk_2.17.100.3.bin).

Once downloaded, you need to execute it (`chmod +x [...].bin`) using sudo: `sudo ./[...].bin`.

If you try to make the file again, it should now work and output:

```
Succeed.
SIGN => bin/enclave.signed.so
```