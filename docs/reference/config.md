---
title: "wasmcloud.toml"
date: 2023-09-29T11:02:05+06:00
sidebar_position: 4
draft: false
---

Even with all of wasmCloud's benefits, it still has every developers' least favorite file(s) - the config file. Luckily, we've cut it down to just one file and made it as simple as possible to use.

You can think of `wasmcloud.toml` as the `package.json` or `Cargo.toml` of the wasmCloud ecosystem. It's the main config file for any of your wasmCloud projects. Let's look at an example:

```toml
name = "echo"
language = "rust"
type = "actor"
version = "0.1.0"

[actor]
claims = ["wasmcloud:httpserver"]

[rust]
cargo_path = "/tmp/cargo"
```

This is a fairly minimal configuration for a wasmCloud Rust `echo` actor. It defines everything needed to build and sign the actor, such as which [capabilities the actor claims](https://wasmcloud.com/docs/fundamentals/capabilities/) and what language toolchain to use.

> Note that `cargo_path` isn't required. If you're using a standard `cargo` toolchain you could omit the `[rust]` section from this file entirely.

## `wash build`

The partner-in-crime to `wasmcloud.toml` is `wash build`. This command takes all the options set in the `wasmcloud.toml` file to build and sign the actor/provider/interface.

See the `build` docs in the [wash repo](https://github.com/wasmCloud/wash#build) for more info.

## `wasmcloud.toml` Spec

### Common Config
| Setting       | Type   | Default                       | Description                                                                            |
| ------------- | ------ | ----------------------------- | -------------------------------------------------------------------------------------- |
| name          | string |                               | Name of the project                                                                    | 
| version       | string |                               | Semantic version of the project                                                        |
| path          | string | `{pwd}`                       | Path to the project directory to determine where built and signed artifacts are output | 
| wasm_bin_name | string | "name" setting                | Expected name of the wasm module binary that will be generated                         |
| language      | enum   | [rust, tinygo]                | Language that actor or provider is written in                                          |
| type          | enum   | [actor, provider, interface ] | Type of wasmcloud artifact that is being generated                                     |


### Language Config - [tinygo]
| Setting     | Type   | Default        | Description                   |
| ----------- | ------ | -------------- | ----------------------------- |
| tinygo_path | string | `which tinygo` | The path to the tinygo binary |

### Language Config - [rust]
| Setting     | Type   | Default       | Description                             |
| ----------- | ------ | ------------- | --------------------------------------- |
| cargo_path  | string | `which cargo` | The path to the cargo binary            |
| target_path | string | ./target      | Path to cargo/rust's `target` directory |

### Type Config - [actor]
| Setting | Type | Default | Description |
| ------- | ---- | ------- | ----------- |
| claims        | list    | []                     | The list of provider claims that this actor requires. eg. ["wasmcloud:httpserver", "wasmcloud:blobstore"] |
| registry      | string  | localhost:8080         | The registry to push to. eg. "localhost:8080"                                                                              |
| push_insecure | boolean | false | Whether to push to the registry insecurely                                                                                                  |
| key_directory | string  | `~/.wash/keys`         | The directory to store the private signing keys in                                                                        |
| filename      | string  | <build_output>_s.wasm  | The filename of the signed wasm actor                                                                                      |
| wasm_target   | string  | wasm32-unknown-unknown | Compile target                                                                                                            | 
| call_alias    | string  |                        |  The call alias of the actor |

### Type Config - [provider]
| Setting       | Type   | Default | Description                       |
| ------------- | ------ | ------- | --------------------------------- |
| capability_id | string |         | The capability ID of the provider |
| vendor        | string |         | The vendor name of the provider   |

### Type Config - [interface]
| Setting        | Type   | Default | Description               |
| -------------- | ------ | ------- | ------------------------- |
| html_target    | string | ./html  | Directory to output HTML  |
| codegen_config | string | .       | Path to codegen.toml file |
