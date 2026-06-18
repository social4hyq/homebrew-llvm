# Homebrew LLVM for HarmonyOS

Homebrew tap providing LLVM toolchains (clang + lld + OHOS multiarch runtime libs) for HarmonyOS native development on `aarch64-linux-ohos`.

## Formulas

| Formula | Version | Bottle | Notes |
|---------|---------|--------|-------|
| `llvm@21` | 21.1.8 | arm64_ohos | keg-only, code-sign patch |
| `llvm@22` | 22.1.7 | — | keg-only, code-sign patch |
| `llvm` | 22.1.7 | — | keg-only, unversioned alias |

All formulas are **keg-only** — they install into isolated prefixes and do not conflict with `ohos-sdk` (LLVM 15).

## Install

```sh
# Add tap (atomgit.com mirror)
brew tap social4hyq/llvm https://atomgit.com/social4hyq/homebrew-llvm.git

# Or from GitHub
brew tap social4hyq/llvm https://github.com/social4hyq/homebrew-llvm.git

# Install LLVM 21 (with pre-built bottle)
brew install llvm@21

# Install LLVM 22 (builds from source)
brew install llvm@22

# Install latest (LLVM 22)
brew install llvm
```

## Usage

```sh
# Get install prefix
LLVM21=$(brew --prefix llvm@21)

# Cross-compile for HarmonyOS (aarch64)
$LLVM21/bin/aarch64-linux-ohos-clang++ -stdlib=libc++ \
  --sysroot=$(brew --prefix ohos-sdk)/native/sysroot \
  hello.cpp -o hello

# Or use the unprefixed clang directly
$LLVM21/bin/clang++ --target=aarch64-linux-ohos -stdlib=libc++ \
  --sysroot=$(brew --prefix ohos-sdk)/native/sysroot \
  hello.cpp -o hello
```

## Dependencies

- [`ohos-sdk`](https://atomgit.com/Harmonybrew/homebrew-ohos-sdk) — provides sysroot, libcxx-ohos headers, and binary-sign-tool
- `cmake`, `ninja` — build only

## Coexistence with ohos-sdk

`ohos-sdk` ships LLVM 15 for bootstrapping. The formulas in this tap install newer LLVM versions into separate prefixes without interference:

```
ohos-sdk      → Cellar/ohos-sdk/26.0.0.18_1/   (LLVM 15, linked)
llvm@21       → Cellar/llvm@21/21.1.8/          (LLVM 21, keg-only)
llvm@22       → Cellar/llvm@22/22.1.7/          (LLVM 22, keg-only)
```

## License

Apache-2.0 with LLVM-exception
