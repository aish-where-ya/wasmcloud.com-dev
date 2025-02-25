---
title: 2023-09-13 Community Meeting
authors: [brooks]
tags: [community, meeting]
description: Agenda, notes, and recording for the 2023-09-13 community meeting
---

import ReactPlayer from 'react-player/youtube';

### Agenda

- DEMO: Running Rust and TinyGo WebAssembly components in wasmCloud
- DISCUSSION: Check-in on the [witify](https://github.com/wasmCloud/wasmCloud/milestone/6) milestone

<!--truncate-->

### Meeting Notes

- **DEMO: Running Rust and TinyGo WebAssembly Components in wasmCloud**
  - We wanted to demo some of the projects we're most proud of coming out of WasmCon, and this is definitely one of them.
  - During the workshop we had 2 intro wasmCloud actors. The cool thing - these were Wasm Components instead of Wasm modules. One is written in Rust and the other in TinyGo.
  - This is important because these components were built using wit and wit-bindgen and do not include wasmCloud SDKs or dependencies!
  - We still sign the wasm component but we also have the Wasm toolchain and the subcommands component to pull the wit file and World used for this particular component.
  - If you're working with WebAssembly, we recommend you catch a few of the amazing talks given at WasmCon - particularly Taylor and Bailey's Dreamy Fettuccine talk [https://www.youtube.com/watch?v=Hrsi9F7VWHw](https://www.youtube.com/watch?v=Hrsi9F7VWHw) and Luke Wagner's keynote 'What is a component? (and why?)' [https://www.youtube.com/watch?v=tAACYA1Mwv4](https://www.youtube.com/watch?v=tAACYA1Mwv4).
  - You can now see all the wasmCon recordings on the Linux Foundation's [YouTube channel](https://www.youtube.com/playlist?list=PLbzoR-pLrL6pdO2yYg4jAQAW3zd15CYO0).
  - We'll also share some of the highlight talks on Slack.
  - If you have any specific questions around our talks, or would like to learn more, do get in touch.
- **DISCUSSION: Wasifills**
  - Check out the recording for the full context as to where Wasifills fit in the overall roadmap.
  - Progress is being made here - we will be landing a few new features in the next few weeks.
  - We are also waiting for Streams and Resources which will prompt (good!) breaking changes.
  - [Wasifills RFC](https://github.com/wasmCloud/wasmCloud/issues/497) requires update but is a good representation of where we are.
  - Why is this important? We need Wasifills to have custom contracts, which is crucial to the Smithy depreciation plan.
  - We also want to depreciate wasmbus RPC and this is also dependent on Wasifills.
  - More to share soon.
- **DISCUSSION: wasmCloud Roadmap**
  - Currently working on support for wasi-cloud core interface in wasmCloud.
  - As well as KV and HTTP, there are other wasi-cloud core components that are critical to support - messaging, runtime config, blobstore etc.
  - Latest version of wash does launch the latest rust host!
  - Next: tracking [standardized WASI Worlds](https://github.com/wasmCloud/wasmCloud/milestone/8). - Any questions or suggestions regarding the wasmCloud roadmap, get in touch!

### Recording

Check out the recording for the wider community discussion.

<ReactPlayer url='https://www.youtube.com/watch?v=6teMfDOhZqU' controls />
