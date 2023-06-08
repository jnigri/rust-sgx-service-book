# Creating your first service

This section provides the instructions to create a web server inside the enclave.
Two approaches are described.
A first approach uses [hyper](https://crates.io/crates/hyper).
It allows to do a REST-like API.
A second approach uses Google approach for Remote Procedure Calls: [gRPC](https://grpc.io/).

## Common changes

In both cases we need to make some changes in our code.

We started with the helloworld code sample.
Our project is composed of two parts.
A first project in the `/app` folder corresponds to the code that is executed outside of the enclave.
The `main.rs` file contains the code that calls the enclave.
At the moment, that code calls a function named `say_something`.
We will change that to `start_server`.
We won't need to pass the input string but `start_server` will take the port number as parameter.

After our changes, `main.rs` will look like this:

```rust
extern crate sgx_types;
extern crate sgx_urts;

use sgx_types::error::SgxStatus;
use sgx_types::types::*;
use sgx_urts::enclave::SgxEnclave;

static ENCLAVE_FILE: &str = "enclave.signed.so";

extern "C" {
    fn start_server(eid: EnclaveId, retval: *mut SgxStatus, port_number: u16) -> SgxStatus;
}

fn main() {
    let enclave = match SgxEnclave::create(ENCLAVE_FILE, true) {
        Ok(enclave) => {
            println!("[+] Init Enclave Successful {}!", enclave.eid());
            enclave
        }
        Err(err) => {
            println!("[-] Init Enclave Failed {}!", err.as_str());
            return;
        }
    };

    let mut retval = SgxStatus::Success;
    let port_number: u16 = 3000;

    let result = unsafe { start_server(enclave.eid(), &mut retval, port_number) };
    match result {
        SgxStatus::Success => println!("[+] ECall Success..."),
        _ => println!("[-] ECall Enclave Failed {}!", result.as_str()),
    }
}
```

We now need to change the code that will run inside the enclave.
This code is located in the folder `enclave/`.
We first need to change the interface of the enclave by changing the `enclave.edl` file.
(`.edl` file contains the interface between the untrusted application and the enclave.)

```cpp
enclave {
    from "sgx_stdio.edl" import *;
    from "sgx_tstd.edl" import *;

    trusted {
        public sgx_status_t start_server(uint16_t port);
    };
};
```

Then we need to change the actual enclave code, situated in the `lib.rs` file in `src`:
```rust
#![cfg_attr(not(target_vendor = "teaclave"), no_std)]
#![cfg_attr(target_vendor = "teaclave", feature(rustc_private))]

#[cfg(not(target_vendor = "teaclave"))]
#[macro_use]
extern crate sgx_tstd as std;
extern crate sgx_types;

use sgx_types::error::SgxStatus;

/// # Safety
#[no_mangle]
pub unsafe extern "C" fn start_server(port_number: u16) -> SgxStatus {
    // Ocall to normal world for output
    println!("Starting server on port {}", port_number);

    SgxStatus::Success
}
```

You should now see the following when running `make run`:

```bash
===== Run Enclave =====

[+] Init Enclave Successful 2!
Starting server on port 3000
[+] ECall Success..
```

## Rest vs gRPC

### Benefits of HTTP/REST

Using REST API will be familiar to most developers. Most languages will have clients that supports it as it's the simplest approach.

### Benefits of gRPC

Assuming that you are using a language that supports gRPC, it will generate boilerplate code for you. It uses protobuffer to send data over the network, which means that data payload will be smaller than JSON object sent in a HTTP/REST service.