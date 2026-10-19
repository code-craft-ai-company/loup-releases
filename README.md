# Loup Lite — official toolchain builds

**Loup** (*Language of Unusual Portability | Power*) is a strongly typed language for
scripting, automation, code generation and embedded templating, with a
capability-based runtime and a portable WebAssembly runtime.

> **This is Loup Lite**, the free public build of Loup, intended for
> **educational and training purposes**. It contains the Loup toolchain and
> standard library, but **not** Loup's cryptography APIs — the cryptography
> library with its encryption, signatures, key exchange and TLS — and **not**
> the full set of Loup modules and packages. Its only cryptographic function is
> SHA-256 hashing.
>
> **Customers: get the full release from the Code Craft AI website,
> <https://code-craft.ai/loup/download>.** Commercial use of Loup needs a seat:
> <https://code-craft.ai/loup/license>.

This repository publishes the **official Loup Lite builds** from
[Code Craft AI](https://code-craft.ai): one archive per platform, the download
installers, and signed checksums. It holds no source code.

To learn Loup, read the Loup books at <https://code-craft.ai/references/loup>.

## Install Loup Lite

Linux and macOS:

```sh
curl -fsSL https://github.com/code-craft-ai-company/loup-releases/releases/latest/download/loup-install.sh | sh
```

Windows (PowerShell):

```powershell
irm https://github.com/code-craft-ai-company/loup-releases/releases/latest/download/loup-install.ps1 | iex
```

The installers pick the archive for your machine, verify it against the
release's `SHA256SUMS`, unpack it into one directory (`~/.loup`, or
`%LOCALAPPDATA%\Programs\Loup` on Windows) and put its `bin` on your PATH. No
administrator rights are needed. A specific version: `sh -s -- --version 1.25.2`
(Unix) or `$env:LOUP_VERSION = "1.25.2"` before the Windows command.

| Platform | Archive | Native builds |
| --- | --- | --- |
| Linux x86-64 (glibc 2.35+) | `loup-<version>-x86_64-unknown-linux-gnu.tar.gz` | `loup build --target native` (clang 14+) |
| Linux arm64 (glibc 2.35+) | `loup-<version>-aarch64-unknown-linux-gnu.tar.gz` | `loup build --target native` (clang 17+) |
| macOS 11+, Apple silicon | `loup-<version>-aarch64-apple-darwin.tar.gz` | `loup-bootstrap build --target native` (preview) |
| macOS 11+, Intel | `loup-<version>-x86_64-apple-darwin.tar.gz` | `loup-bootstrap build --target native` (preview) |
| Windows 10+ x86-64 | `loup-<version>-x86_64-pc-windows-msvc.zip` | `loup build --target native` (preview; LLVM 17+ and Visual Studio Build Tools) |

Every archive holds the Loup toolchain (`loup`, the trusted Stage0 compiler
`loup-bootstrap`) and the Loup Wasm runtime `loup-launcher`, so scripts, Wasm
applications and packaged desktop applications work on every platform. The
cryptography library and the rest of the Loup packages are in the full release
only (<https://code-craft.ai/loup/download>).

To install by hand: download the archive, check it against `SHA256SUMS`, unpack
it anywhere and add its `bin` directory to PATH. Each archive's `README.txt`
has the platform notes.

## First steps

```sh
echo 'println("hello, loup");' > hello.loup
loup run hello.loup            # run a host script

loup-bootstrap init demo       # scaffold an application
cd demo
loup-bootstrap build           # -> build/demo.wasm
loup-launcher --run-bare build/demo.wasm          # prints: hello, demo
loup-bootstrap build --target desktop -o pkg      # a packaged app
./pkg/bin/main                 # Windows: pkg\bin\main.exe
```

## Verify a download

`SHA256SUMS` is signed keylessly with [Sigstore](https://www.sigstore.dev/) by
the release workflow:

```sh
cosign verify-blob --bundle SHA256SUMS.sigstore.json \
  --certificate-identity-regexp '^https://github\.com/code-craft-ai-company/loup/\.github/workflows/release\.yml@refs/tags/v' \
  --certificate-oidc-issuer https://token.actions.githubusercontent.com SHA256SUMS
sha256sum --ignore-missing -c SHA256SUMS
```

## License

Loup is licensed under the [Loup Source-Available License 1.0](LICENSE).
Code Craft AI is the only source of official builds: Loup Lite here, and the
full release at <https://code-craft.ai/loup/download>. Contact:
<https://code-craft.ai/contact>.
