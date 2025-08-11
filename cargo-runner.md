
## Cargo Runner

Requirements
- [cargo-runner](https://github.com/cargo-runner/cargo-runner) cli


## Why use Cargo Runner
Claude is not well verse in running the correct Command or even able to tell what kind of configuration to use to override those

Cargo Runner is a replacement for Rust Runnables in Rust Analyzer
It has small memory footprint , and doesn't require Rust Analyzer to fetch the Runnables
Detecting the command alone on a file is easy but making sure that command applies to Scope Range and Follow Hierarchy
Meaning even if the line is both contain on different Scope Range the one with the Highest Priority Get to be executed.

Cargo Runner also when paired with Overrides can be powerful. Cargo runner has a concept of Different Runner Framework
- Test Framework 
- Binary Framework

Which allows you to swap cargo run to any framework e.g. dioxus cli or cargo leptos
also it can be use to swap cargo test to any test framework e.g. cargo miri test or cargo nextest run or even combination of both cargo miri nextest run

Those Config can be override on different Level 
- Root
- Workspace (if Available / Optional)
- Package
- File
- Module
- Per Function

This is crucial when you dont want your AI/LLM to figure out what command to use, assuming you have already configure your project.

Cargo Runner also support Template which can be use to quickly scaffold those configuration 
We have the following templates:
- leptos csr
- leptos ssr
- leptos with env
- dioxus
- dioxus ios
- dioxus android
- dioxus web
- dioxus liveview
- dioxus macos
- dioxus windows
- dioxus linux
- tauri 
- shuttle
- many more...

You can use the template to quickly set e.g. main.rs file to run those test framework or binary framework instead of cargo

## How to use it

Running whole file 
```
# this would run the binary framework
cargo runner run src/main.rs
# this would run the test framework
cargo runner run src/lib.rs
```

Cargo runner is intelligent enough to run what specific Runnable should be on specific line

Given example code
```rust
fn main() {
println!("hello world");
}

#[cfg(test)]
mod tests {
  #[test] //<!-- line 7
  fn it_works(){
    assert!(true);
  }
}
```

If we would try to run

```
cargo runner run src/main.rs:7
```

It would run e.g.

```
cargo test --package project-a --bin project-a -- tests::it_works --exact
```

## Pros
- No Rust Analyzer, You will not be block when Rust analyze is busy
- Config Persistency
- Granular Config
- Use Test / Binary Framework to override default cargo run/test/bench
- Can Also uses the Rust Analyze Build Target to avoid Extra Artifacts by setting `extra_env`: `{ "CARGO_TARGET_DIR" : "target/rust-analyze" }`

If you are using LLM like Claude , the LLM will not need to think about what command to run, also it AI can trigger very specific command for the specific line number 
without having to think about what feature to use or what target to add , it just works.

If you pair it with `check` scripts where you have errors or warning at specific line e.g. on test, that would be very easy for any LLM to run only those test for that specific file and line number.












