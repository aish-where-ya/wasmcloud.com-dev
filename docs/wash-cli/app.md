---
title: "wash app"
draft: false
sidebar_position: 1
description: "wash app command reference"
---

When deploying apps using [wadm](../fundamentals/wadm), the easiest way to manage these apps is using `wash app`. `wash app` provides us with all the tools needed to add, remove and get the necessary details of your deployed as well as undeployed applications. Following are the subcommands available under `wash app`.

- list
- get
- history
- del
- put
- deploy
- undeploy

### list
This will retrieve a list of all applications that wadm knows about. Application specifications (also referred to in our documentation and code as "models") are stored _without regard to lattice_.

Usage: 

```
wash app list
```

### get
The `get` command is used to retrieve the details for a specific version of a model.You can retrieve both the original text (e.g. YAML) that you submitted and the vetted and normalized model in JSON format.

Usage:

```
wash app get <name> [version]
wash app get petclinic 0.0.1
```

### history
The `history` command is used to retrieve the version history of a given model. You can view all the previous versions available in store and their respective status for a given app.

Usage:

```
wash app history <name>
wash app history petclinic
```

### del
The `del` command is used to delete an application model of a specific version. You can optionally delete _all_ versions of a specification, but make sure you're aware of the consequences when you do this.

Usage:

```
wash app del <name> <version>
wash app del petclinic 0.0.1
wash app del petclinic --delete-all
```

### put

Performs an _idempotent_ put operation for an application specification _version_. Each time you put a model spec YAML to the server, it will either produce a new version or _be ignored_. Versions are _immutable_ and as such cannot be overwritten. If you wish to change anything about your spec, it needs to be done in a new version.

Usage:

```
wash app put <SOURCE>
wash app put petclinic.yaml
```

### deploy
Application specification versions are deployed _to a given lattice_. When a specification is deployed, wadm will start monitoring the state of the relevant lattice. Once it is satisfied it has enough information, it will begin its _control loop_, where it constantly compares the desired state of an application with the actual state and issues the appropriate low-level imperative commands to reconcile the two.

Usage:

```
wash app deploy <name> [version]
wash app deploy petclinic v0.0.1
```

### undeploy
Undeploying an application spec tells wadm to stop monitoring that deployment. For now, wadm does not support _destructive_ undeploys, so all resources originally provisioned for an application will _remain_ after that application is undeployed. In the near future, wadm will support destructive undeploys which can optionally remove previously provisioned resources. This

Usage:

```
wash app undeploy <name>
wash app undeploy petclinic
```


### Options
The following options can be specified with all of the above subcommands for a finer control of your environment.

#### --output 
Alias: `-o`.
        Specify output format (text or json). The default value is text.
        
#### --ctl-host 
Alias: `-r`.
        CTL Host for connection. The default value is 127.0.0.1 for local nats [env: WASMCLOUD_CTL_HOST=]

#### --experimental
Whether or not to enable experimental features [env: WASH_EXPERIMENTAL=]

#### --ctl-port 
Alias: `-p`.
        CTL Port for connections, defaults to 4222 for local nats [env: WASMCLOUD_CTL_PORT=]

#### --ctl-jwt 
JWT file for CTL authentication. Must be supplied with ctl_seed [env: WASMCLOUD_CTL_JWT]

#### --ctl-seed 
Seed file or literal for CTL authentication. Must be supplied with ctl_jwt [env: WASMCLOUD_CTL_SEED]

#### --ctl-credsfile 
Credsfile for CTL authentication. Combines ctl_seed and ctl_jwt. See https://docs.nats.io/using-nats/developer/connecting/creds for details [env: WASH_CTL_CREDS]

#### --js-domain 
JS domain for wasmcloud control interface. Defaults to None [env: WASMCLOUD_JS_DOMAIN]

#### --lattice-prefix 
Alias: `-x`.
        Lattice prefix for wasmcloud control interface. The default value is "default". [env: WASMCLOUD_LATTICE_PREFIX=]

#### --timeout-ms 
Alias: `-t`.
        Timeout length to await a control interface response. The default value is 2000 milliseconds [env: WASMCLOUD_CTL_TIMEOUT_MS=] [default: 2000]

#### --context 
Path to a context with values to use for CTL connection and authentication
