---
title: 2023-10-25 Community Meeting
authors: [brooks]
tags: [community, meeting]
description: Agenda, notes, and recording for the 2023-10-25 community meeting
---

import ReactPlayer from 'react-player/youtube';

### Agenda

- DEMO: Globby! (Blobby, but Go!) from Jordan
- UPDATE: wasmcloud/wasmcloud MonoRepo
- DISCUSSION: CNCF Incubation Proposal

<!--truncate-->

### Meeting Notes

**DEMO: Globby Demo from Jordan**

- Lachlan has helped us take Blobby, KV Counter actor and echo actor - well-known wasmCloud apps - into TinyGo! In the demo you can see that wasmCloud instance is being pulled from a separate Cosmonic application.
- Why are we showing this? All the TinyGo examples have been rewritten to use WASI contracts. No more Smithy and no more wasmCloud dependencies!
- When we open them and see we have no dependencies on the echo actor, Blobby actor and KV-counter actor.
- As it stands right now, Roman is bringing a ton of Wasm capabilities to WASI. By the time Wasmtime 15 comes out we will have moved our examples to WASI.
- Workflow: `wash build` does do all this work for you but all we’re doing is running a `go-generate` - a wrapper around `wit-bindgen`.
- As we edit our globby.wit / echo.wit / kvcountser.wit we regenerate the bindings every time. We create the component, sign it and it’s up.
- Wash will front all this work for us before long - take a look at the recording for all the build steps.
- The code itself is nothing too spectacular - there has been a ton of work in the bindgen space to make the naming a little more intuitive.
- Jordan pays particular attention to WASI streams - everything is a u32 in the background, when we’re streaming in data we are doing it at a low level.
- Many of the existing libraries would hide this from us. We have to ask he stream how much data it will let us write. We take our full payload and chop it up and only send as much as it will allow at one time.
- This was the biggest challenge - loops every time you do a WASI streams. Other than that, this was a relatively easy process.
- We used the functions that bindgen gives us. `go-generate` is a simple wrapper around `wit-bindgen`.
- Best thing we can do is make sure we’re using the same version of the contract.
- If you open the wit file you can get a better idea of what pointers correspond to what types. Manual checking and knowing what you’re checking really helps.
- Check out the recording for the details and discussion.

**UPDATE: wasmcloud/wasmcloud MonoRepo**

- We are moving a ton of materials, examples and repos over into a new wasmCloud/wasmCloud monorepo. It will be great to release a version of wasmCloud and bump the refresh across our architecture.
- The only thing left is the CI so we need to review our GitHub actions before this can happen.
- wascap and provider-archive are two lower-level repos that are now available in the crates directory.
- More are on their way. Stay tuned, we will have public archives for a while - least friction possible plus ease of use for newcomers.

**DISCUSSION: wasmCloud Going for CNCF Incubation Status**

- We are officially applying for [incubation status in the CNCF](https://github.com/cncf/toc/pull/1198)!
- Guidelines and criteria are a robust community, that we’re showing traction, contributions, PRs - that it is an active project ad will continue to grow in the coming years.
- We will share the PR in wasmCloud Slack but we wanted to show off some of the things we are most proud of.
    - Stars 2,300+?
    - 441 contributors
    - Nearly 10,000+ commits across the wasmCloud ecosystem
- The numbers just continue to grow - especially since we became a CNCF Sandbox Project.
- Contributions have boomed - and a ton of companies have contributed which is great to see.
- The PR for this will shortly be available in the repo - CNCF does not act on anything within 6 weeks of KubeCon - we expect more movement after Chicago.
- Give us your support - stars, commits, contributions all welcome!
- A huge thank you to the community for getting us to where we are. We’re truly excited to achieve this next milestone in our development.

**Upcoming Events**
Don't forget: 🗓 **[Cloud Native Wasm Day 2023](https://events.linuxfoundation.org/kubecon-cloudnativecon-north-america/co-located-events/cloud-native-wasm-day/)** takes place on November 6. Catch our talks:

- **[WebAssembly Component Model: Enhancing Security, Productivity, and Green Computing](https://sched.co/1Rj12)** - Bailey Hayes (ByteCode Alliance TSC director and Cosmonic CTO) & Kate Goldenring (Fermyon Technologies, Inc).
- **[Orchestrating Wasm: Reconciliation Loops Aren't Owned by Kubernetes](https://sched.co/1Rj5v)** - Taylor Thomas and Dan Norris, Cosmonic

Don’t forget: we’ll also be at 🗓 **[CiliumCon on November 6](https://colocatedeventsna2023.sched.com/overview/area/CiliumCon)**. Catch our talk:

- **[Netreap: Bridging the Gap Between Cilium and Nomad - Dan Norris, Cosmonic](https://colocatedeventsna2023.sched.com/#)**

Don't forget: 🗓 **[KubeCon NA 2023](https://events.linuxfoundation.org/kubecon-cloudnativecon-north-america/)** takes place 6 - 9 November. Come visit us at booth **`M21`** and at the **[CNCF wasmCloud](https://wasmcloud.com/)** in the Projects Pavilion. Catch our talk:

- **[Serving Backends like Frontends](https://sched.co/1R2r8)** - Brooks Townsend, Cosmonic.

### Recording

<ReactPlayer url='https://www.youtube.com/watch?v=idQYnXqvAhU' controls />
