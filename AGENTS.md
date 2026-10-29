# naga

This repository contains only `naga` and the crates it needs, trimmed from the wgpu workspace.

- `naga` is our shader translator. It is responsible for consuming and translating untrusted shaders to trusted shader code.
- `naga-types` holds the shader-language types, such as target versions and binding maps, that `naga` uses.
- `naga-cli`, `naga-test`, `naga/hlsl-snapshots`, `naga/fuzz` and `naga/xtask` are the CLI front-end, the test harness, the generated HLSL snapshots, the fuzz targets and the development tasks for `naga`.

`naga` is used by Firefox and Servo to implement WebGPU, and it processes untrusted shader input. Preserve validation at the safe boundary, and do not rely on callers to reject invalid input. Touching this crate requires the most consideration of potential side effects.

## Code Style

- Do not add comments unless the changed code requires an explanation. If an explanation is required, add the shortest complete explanation. The contributor is expected to rewrite it.
- Do not write large explanatory comment blocks.
- Do not use emoji.

## Workflow

- To check the code compiles: `cargo clippy --all-targets --all-features`.
- To format the code: `cargo fmt`
- To run all tests: `cargo test --all-features --workspace`.
- To run the generated outputs through the matching validator: `cargo xtask validate <dot|glsl|hlsl <dxc|fxc>|msl|spv|wgsl|all>`.
- Report every test failure. You may identify a failure as suspected to be unrelated, but do not assume it is unrelated until you validate that conclusion.
- Never commit anything without being asked, ever. If you are asked to make a commit, _never_ co-author yourself.
- Use the WebGPU and WGSL specifications as a reference to determine the correct behavior.

### Windows notes

- The recursion tests need a bigger test-thread stack: set `RUST_MIN_STACK=33554432` before running `cargo test`.
- Every test run rewrites `naga/tests/out`. The working copy is checked out with CRLF (`core.autocrlf=true`) but the tests write LF, so `git status` lists those files as modified while `git diff` is empty. Restore them with `git checkout -- naga/tests/out`.