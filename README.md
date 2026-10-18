# Loup — official toolchain builds

**Loup** (*Language of Unusual Portability*) is a strongly typed language for
scripting, automation, code generation and embedded templating, with a
capability-based runtime and a portable WebAssembly runtime.

This repository publishes the **official Loup toolchain builds** from
[Code Craft AI](https://code-craft.ai): one archive per platform, the download
installers, and signed checksums. It holds no source code.

## Install

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
administrator rights are needed. A specific version: `sh -s -- --version 1.25.1`
(Unix) or `$env:LOUP_VERSION = "1.25.1"` before the Windows command.

| Platform | Archive | Native builds |
| --- | --- | --- |
| Linux x86-64 (glibc 2.35+) | `loup-<version>-x86_64-unknown-linux-gnu.tar.gz` | `loup build --target native` (clang 14+) |
| Linux arm64 (glibc 2.35+) | `loup-<version>-aarch64-unknown-linux-gnu.tar.gz` | not yet |
| macOS 11+, Apple silicon | `loup-<version>-aarch64-apple-darwin.tar.gz` | `loup-bootstrap build --target native` (preview) |
| macOS 11+, Intel | `loup-<version>-x86_64-apple-darwin.tar.gz` | `loup-bootstrap build --target native` (preview) |
| Windows 10+ x86-64 | `loup-<version>-x86_64-pc-windows-msvc.zip` | not yet |

Every archive holds the Loup toolchain (`loup`, the trusted Stage0 compiler
`loup-bootstrap`) and the Loup Wasm runtime `loup-launcher`, so scripts, Wasm
applications and packaged desktop applications work on every platform.

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
Code Craft AI is the only source of official builds. Contact:
<https://code-craft.ai/contact_us>.
