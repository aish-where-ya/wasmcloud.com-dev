---
title: 2023-10-18 Community Meeting
authors: [brooks]
tags: [community, meeting]
description: Agenda, notes, and recording for the 2023-10-18 community meeting
---

import ReactPlayer from 'react-player/youtube';

### Agenda

- DEMO: wash build and progress to working with wasi-preview-2 components
- DISCUSSION: [wasmCloud security audit](https://ostif.org/ostif-has-completed-a-security-audit-of-wasmcloud/)
- DISCUSSION: wasmcloud/wasmcloud monorepo
- DISCUSSION: Accepting ADRs
- DISCUSSION: Give an update about the community's growth and WASI-virt

<!--truncate-->

### Meeting Notes

#### DEMO: `wash build` & progress to WASI Preview 2

- Brooks demo'd VSCode (in dark mode) with
- If you've built Webassembly while working with wasmCloud before, you've been using `wash build`
- `wash build` started as a light wrapper around toolchains to compile to WebAssembly
- `wash build` enables building and signing actors for running in wasmCloud
  - You *could* build the Wasm module manually (ex. in Rust, using `cargo`) and use the `wash claims sign` CLI to manage the claims on the module.
- `wash build` makes it possible to put that config in `wasmcloud.toml`, and get consistent tooling across languages.
  - **Taylor jumped in**
    - The Webassembly community is working on it, but once they get to that place, then we're going to have one of the easier ways to build a component.
- `wasmcloud.toml` contains configuration (e.x. metadata and build options) that controls how `wash` will configure itself.
- You can find out more by heading to the [docs site][wasmcloud-docs] and [searching for `wash build`][wash-build-config].
- We've had support for `actor.wasm_target` for a while now
  - Originally what we did was compile code to `wasm32-unknown-unknown` (with older tooling we didn't use WASI for communicating across component boundaries)
  - What `actor.wasm_target` lets you do is target `wasm32-wasi` (AKA `wasm_target = wasm32-wasi-preview1`)a
- We've *also* added `wasm_target = wasm32-wasi-preview2`, which does the hard parts of the toolchain for you (no language toolchain supports this right now).
- Compiling to `wasm-wasi32-preview2` means:
  - compiling the code to `wasm32-wasi` (i.e. WASI preview1 WebAssembly module)
  - adapts the preview1 component to preview2 using a [component adapter][wasmtime-component-adapter]
- ⚠ There are unfortunately lots of ways to stub your toes currently on the WebAssembly toolchain while trying to build Preview2 components, so there maybe be errors!
- Once your module is built, we can use `wasm-tools` to see the component model imports and exports.
  - Command: `wasm-tools component wit path/to/file.wasm`
- A great example to follow is [`kvcounter-wasmcon2023` in `wasmCloud/examples`](https://github.com/wasmCloud/examples/tree/main/actor/kvcounter-wasmcon2023)
  - Use the [`just`][just] commands in there to run commands (run `just` to get a list of available commands)
- For stuff like Preview2 to work, *multiple* things have to match up:
  - WASI spec & WIT definitions in the adapter
  - WASI spec & WIT definitions in the component itself
  - Underlying engine that's going to run the module (`wasmtime` version under `wasmCloud`
- The rapid upstream changes makes it hard to make the flow simpler/less error-prone for now, the standards are making great improvements quickly.
- Any time you have a preview1 component, you can adapt it to a Wasm preview2 component -- this workflow is required no matter what language you're using -- Rust, TinyGo, ETC
- We went over the [kvcounter demo](https://github.com/wasmCloud/examples/tree/main/actor/kvcounter-wasmcon2023) Rust code
- After building actors with `wash build`, we can inspect the resulting Wasm module with `wash inspect`
- If we use `wasm-tools component wit` on the web assembly component that is built to see the imports and exports that are expected.
- The host expects the interface that WASI provides here, so components that run on it must provie those.
- You can use the `just run-actor` in the repo to do some of the local things that are possible there (use `just` to see a list of available commands)

##### Question: how does the code look like? (Kevin)

Brooks showed the code, and walked through `lib.rs` and the WIT

##### Question: is `write_http_response` generated by `wit_bindgen` or is that from somewhere else (Kevin)?

It's a custom function that's written but it's necessary to write the HTTP responses using the WIT machinery, according to the WIT contracts.

[wasmcloud-docs]: https://wasmcloud.com/docs/intro
[wash-build-config]: https://wasmcloud.com/docs/fundamentals/workflow/config#wash-build
[wasmtime-component-adapter]: https://github.com/bytecodealliance/wasmtime/tree/main/crates/wasi-preview1-component-adapter
[just]: https://github.com/casey/just

#### ANNOUNCEMENT: wasmCloud Security Audit

- [📖 Trail of Bits security assessment](https://ostif.org/wp-content/uploads/2023/10/wasmcloud-audit-ostif-trail-of-bits-final.pdf)
- WasmCloud received a security audit from the folks at Trail of Bits.
- **TL;DR** is that "WasmCloud is a well reviewed project, with lots of diligence in it's security posture. THis has paid off, as evidenced by this audit, which had no severe or high issues to resolve.
- Kudos to Kevin the founder of wasmCloud who made decisions that paid off with respect to security, making sure to secure access with keys.
- Feel free to peruse the low criticality vulns that *were* found, and note that they have all been resolved and released as patch versions at this point.
- Thanks to CNCF for sponsoring the security audit w/ Trail of Bits.
- This is a great step forward for moving the wasmCloud project along within the CNCF.

#### DISCUSSION: Managing the wasmCloud GitHub organization repos

- We've talked a lot about reorganizing the orgs in the past, and we still have a lot of repos in our org.
- We've noticed some friction working across many repositories in the past.
  - For example, Brooks was working on a change to the [control interface client crate](https://github.com/wasmCloud/control-interface-client). Updates had to be made to capability providers, wasmcloud, and other crates to properly propagate the changes.
- With all the code in a single repo, we would be able to test all of it and push all the changes at the same time.
- There are various pros/cons to monorepos, and we don't want to start a holy war here
- The basic approach is to push crates that have wasmCloud as a primary consumer into the `wasmCloud` repository.
- Connor & Roman just put up PRs that merge in `wash` and `wascap`, for example.
- It should be possible to head into `wasmCloud/wasmCloud` and contribute changes across multiple repos
- Not *everything* will go into `wasmcloud/wasmcloud` -- `wadm` is unlikely to go in, for example, because it is not necessarily released on a cadence with wasmCloud.
- **Connor chimed in**
  - He'd like `wasmcloud-otp` moved into the repo
- This is a heads up to the community that we've started the work to move everything into the monorepo, with the goal to make it easy to contribute to individual components in there.

#### DISCUSSION: RFCs

- Another thing we've talked about in the past -- RFCs.
- Normally we move RFCs through discussion in the past
- The [Host Metrics RFC](https://github.com/wasmCloud/wasmCloud/issues/664) is a great example, thanks to Patrick for putting it up
  - The last time we commented on this was
- While we have a process around RFCS to propose, accept & close -- we don't have an official process for moving something from proposed to accepted.
- It would be a good idea for maintainers in the relevant project to vote on accepted RFCs to move them from proposed to acepted.
  - Once it's been accepted, we could push the RFC through to the roadmap.
- What we're going to ask Patrick to do (like other RFC proposers) is to put a comment tagging relevant maintainers to ask for approval after a couple of rounds for discussion.
  - To somewhat formalize it we can specify something like "2 maintainers have to approve".
- We want to be clear about how we deal with the RFCs, and their status but don't want to add too much formality.
- Any thoughts on things that work really well/don't?
  - **Bailey chimed in**
    - Great idea brooks!
    - Something that worked well here was [Kubernetes's KEPs](https://github.com/kubernetes/enhancements/blob/master/keps/sig-architecture/0000-kep-process/README.md)
    - We do something similar in the ByteCode Alliance (BCA)
    - They also took into account contributors from different companies
    - There are different levels of where you need to drive consensus.
  - **Taylor chimed in**
    - Don't want to overdue anything, but I do think we have a gap between RFCs and ADRs.
    - One thing I worry about closing an RFC is keeping it open and visible.
    - Closing an RFC stops them from being open & visible.
    - Having a place that it turns into the README document is important.
    - We could soften what we call an ADR to allow for in-discussion statuses
  - **Victor chimed in**
    - What about putting it in the repo?
    - Brooks: This works but it needs to be really accessible
    - Bailey: Do RFCs *always* turn into ADRs?
    - Bailey: We want to leave space for people to YOLO and get thoughts
    - Brooks: Jordan brought up GitHub discussions which could work great.
- Not every RFC is an architectural decision
- Let's try a bunch of things, without going too crazy
- Once an RFC is done, we file the ADR and close the original issue.
- We have a status field for a reason, it's a good idea to accept an RFC and turn it into a README.

#### Don't forget: 🗓 WasmDay 2023 on November 6th

[The event is on November 6th!](https://events.linuxfoundation.org/kubecon-cloudnativecon-north-america/co-located-events/cloud-native-wasm-day/)

[Talk: Orchestrating Wasm: Reconciliation loops aren't owned by Kubernetes](https://events.linuxfoundation.org/kubecon-cloudnativecon-north-america/co-located-events/cncf-hosted-co-located-schedule/)

#### Don't forget: Cosmonic @ Kubecon NA 2023

[Talk: Bringing Cloud Native Wasm to the mainstream](https://kccncna2023.sched.com/event/1RQZf/bringing-cloud-native-wasm-to-the-mainstream-with-wasm-working-group-shivay-lamba-meilisearch-kevin-hoffman-cosmonic?iframe=no&w=100%&sidebar=yes&bg=no)

[Talk: Serving backends like frontends](https://kccncna2023.sched.com/event/1R2r8/delivering-backends-like-frontends-with-webassembly-brooks-townsend-cosmonic?iframe=no&w=100%&sidebar=yes&bg=no)

[Talk: Serving backends like frontends](https://kccncna2023.sched.com/event/1R2r8/delivering-backends-like-frontends-with-webassembly-brooks-townsend-cosmonic?iframe=no&w=100%&sidebar=yes&bg=no)

### Recording

<ReactPlayer url='https://www.youtube.com/watch?v=qaqkbAXfKIA' controls />
