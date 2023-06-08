## Hello World Template

In this setup we are using the [version 2.0.0-preview](https://github.com/apache/incubator-teaclave-sgx-sdk/tree/v2.0.0-preview) of the Rust SGX SDK.

Our starting point is the [helloworld codesample](https://github.com/apache/incubator-teaclave-sgx-sdk/tree/v2.0.0-preview/samplecode/helloworld).  
*Note: You can use [Download Directory](https://download-directory.github.io/) to download the content of this specific folder only.*

## RUST SGX SDK references

If you now run `make` it complains that a file doesn't exist.
This is because the code requires the rust SGX SDK sources.
We can use git submodules to add these sources inside our package:

```
git submodule add -b v2.0.0-preview \
 https://github.com/apache/incubator-teaclave-sgx-sdk.git sgx-sdk
```

We then need to update the references to the `sgx-sdk` in the code:

1. In the `Makefile` we need to update `TOP_DIR := ../..` to point to `sgx-sdk/`.
2. In `Cargo.toml` both in `app/` and `enclave/` you will need to change the paths, e.g. `sgx_types = { path = "../../../sgx_types" }` will become `sgx_types = { path = "../sgx-sdk/sgx_types" }`.

Make sure that `SGX_SDK` in the `Makefile` points to where you installed the SGX SDK from intel.
If you followed the instructions on [Installation](./sgx-install.md) you should have `SGX_SDK ?= /opt/sgxsdk`.
You also need to change the file `build.rs` in the `app` folder:
```rust
    let sdk_dir = env::var("SGX_SDK").unwrap_or_else(|_| "/opt/sgxsdk".to_string());
```

Finally, you will also need the nightly version of rust. To do so, create `rust-toolchain` file with `nightly-2022-02-23` as its content.

Running `make` should now output a succesful message ending with the following lines:
```
Succeed.
SIGN => bin/enclave.signed.so
```

### Rust-Toolchain

You might still have issues when trying to run `make`.
It is possible that `sgx-sdk` requires a specific version of the rust toolchain.
Look into the `sgx-sdk` folder and check if there's a `rust-toolchain` file.
If so, copy it to the root of your repository and rerun `make`.

### Running HelloWorld

If you run `make run`, the project should output something similar to:
```
===== Run Enclave =====

[+] Init Enclave Successful 2!
This is a normal world string passed into Enclave!
This is a in-Enclave Rust string!
[+] ECall Success...
```
Here it says "Successful 2!" but you could have another number as this is the enclave id.
At this point we can confirm that code is executed inside the enclave.

## Naming

You can rename the enclave name `helloworld` by changing the name in the `Cargo.toml` file in the `enclave` folder and renaming the path in the `Makefile` for `RustEnclave_Lib_Name`.

For example, changing the name in the `Cargo.toml` to `enclave`, we'll need to change the `Makefile` as the following:

```
RustEnclave_Lib_Name := $(RustEnclave_Out_Path)/libenclave.a
```

## gitignore

Some of the files generated don't need to be in the git sources.
You can add the following in a `.gitingore` file:
```
bin/
lib/
app/target/
enclave/target/
app/enclave_u*
enclave/enclave_t*
```
