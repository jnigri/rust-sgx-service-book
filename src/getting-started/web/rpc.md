# RPC

[Remote Procedure Call](https://en.wikipedia.org/wiki/Remote_procedure_call) (RPC) requires 3 steps to be set up:

1. We need to define the structure and procedures in [protocol-buffers](https://developers.google.com/protocol-buffers/docs/proto3) (protobuf) files.
2. We need to set up a [build script](https://doc.rust-lang.org/cargo/reference/build-scripts.html) (i.e. a `build.rs` file) to generate the code thanks to the protobuf files using [tonic-build](https://crates.io/crates/tonic-build) and [prost](https://crates.io/crates/prost).
3. We need to implement the code of our server using this generated code and [tonic](https://crates.io/crates/tonic).

### Improving compilation time

As indicated on the prost [crate](https://crates.io/crates/prost): "It's recommended to install protoc locally in your path to improve build times.". Run the following command to improve your compilation time:

```bash
sudo apt-get install protobuf-compiler -y
```
