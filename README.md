# Loup — official toolchain builds

**Loup** (*Language of Unusual Portability*) is a strongly typed language for
scripting, automation, code generation and embedded templating, with a
capability-based runtime and a portable WebAssembly runtime.

This repository publishes the **official Loup toolchain builds** from
[Code Craft AI](https://code-craft.ai): one archive per platform, the download
installers, and signed checksums. It holds no source code.

> No release has been published yet; the first preview release is in preparation.

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
administrator rights are needed. A specific version: `sh -s -- --version 1.25.0`
(Unix) or `$env:LOUP_VERSION = "1.25.0"` before the Windows command.

| Platform | Archive |
| --- | --- |
| Linux x86-64 (glibc) | `loup-<version>-x86_64-unknown-linux-gnu.tar.gz` |
| macOS, Apple silicon | `loup-<version>-aarch64-apple-darwin.tar.gz` |
| macOS, Intel | `loup-<version>-x86_64-apple-darwin.tar.gz` |
| Windows x86-64 | `loup-<version>-x86_64-pc-windows-msvc.zip` |

To install by hand: download the archive, check it against `SHA256SUMS`, unpack
it anywhere and add its `bin` directory to PATH. Each archive's `README.txt`
has the platform notes.

## First steps

```sh
echo 'println("hello, loup");' > hello.loup
loup run hello.loup            # run a host script

loup-bootstrap init demo       # scaffold an application
cd demo && loup-bootstrap build
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
