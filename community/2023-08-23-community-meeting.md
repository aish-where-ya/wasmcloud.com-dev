---
title: 2023-08-23 Community Meeting
authors: [brooks]
tags: [community, meeting]
description: Agenda, notes, and recording for the 2023-08-23 community meeting
---

import ReactPlayer from 'react-player/youtube';

### Agenda

- DEMO: wadm 0.5.0 manifest validation
- DISCUSSION: wadm 0.5.0 official release, wash 0.20.0 coming soon
- DISCUSSION: Roadmap update on Rust host OTP parity

<!--truncate-->

### Meeting Notes

- **New Community Member - Dave Tessman 👋 Hi, Dave!**
- **Discussion - wadm 0.5.0 official release, wash 0.20.0 coming soon**
  - Many alphas for wadm means lots of testing and lots of new features: manifest status, manifest validation, multitenant prefixes.
  - What is Wadm? Tool to assist with deploying applications in a declarative manner.
  - Demo - wadm 0.5.0 manifest validation.
  - validation built on top of OAM spec.
  - define components as custom types.
  - specify links values and spreadscaler conditions with custom traits on components.
  - The Demo:
    - valid demo: kvcounter app should access redis and use http server.
    - invalid dupe actor: duplicate actor with same image ref. Prefer not to run multiple components, use spread to control distribution.
    - invalid link manifest: two versions of the same link results in a validation error because they provide conflicting values.
    - invalid dupe provider: two components with the same provider component names, prefer spread scaler.
    - invalid duplicate link name: two providers with the same contract and link name, links can be used to configure providers.
- **Discussion - Roadmap**
  - Deamonscaler: adds ability to scale a component (actor/provider) to run X copies on every host that matches the label spec.
  - WASI Preview 2 components is in progress.
  - witify - changing interfaces to use wit IDL - only a few issues to complete.
  - OTP/Rust Host feature parity - more closed than open, lots and lots of progress (s/o to Roman and Connor and Victor).
  - New contributors can help out with good-first-issue label on wash, wasmCloud, wadm repos.
- **Questions**
  - What big areas are left in OTP > Rust parity? CJ.
    - [implement get operations for link defs and claims](https://github.com/wasmCloud/wasmCloud/issues/480).
    - [validate actor and provider claims upon start](https://github.com/wasmCloud/wasmCloud/issues/554).
    - [implement policy checking and management](https://github.com/wasmCloud/wasmCloud/issues/480).
    - [chunking for large payloads](https://github.com/wasmCloud/wasmCloud/issues/482).
  - Where to ask questions? Dave Tessman.
    - [https://slack.wasmcloud.com](https://slack.wasmcloud.com)
    - #wasmcloud in [CNCF slack](https://communityinviter.com/apps/cloud-native/cncf).
- [**WASMCON**](https://events.linuxfoundation.org/wasmcon/)
  - Hosted by LinuxFoundation.
  - Workshop by Cosmonic.
  - Talks from Bailey, Liam, Taylor, and Brooks.

### Recording

<ReactPlayer url='https://www.youtube.com/watch?v=Jem3QadAPqE' controls />
