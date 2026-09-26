# Windows code-signing status

Rows were checked on the dates noted below against the specific release packages.
The release API confirmed the listed versions as latest at the time of each
verification; package hashes matched its SHA-256 digests. Each evidence section
describes the files and verification scope.
Signing claims apply to the versions and artifacts listed here, not every
historical release or development build.

| Product | Verified release | Windows Authenticode status |
| --- | --- | --- |
| PC Tweaker | [1.10.3](https://github.com/AurelioAvila/pc-tweaker-app/releases/tag/v1.10.3) | Valid signatures and trusted timestamps on EXE and MSI installers, including both stable download aliases. SHA-256 values match GitHub API asset digests. |
| Redaxa | [0.4.4](https://github.com/AurelioAvila/redaxa/releases/tag/v0.4.4) | Valid publisher signatures and trusted timestamps on both installers and their executable payloads (2026-09-26). Both Tauri updater signatures verified; the manifest covers NSIS, MSI and the legacy Windows target. |
| Redexa Social | [1.10.5](https://github.com/AurelioAvila/redexa-social/releases/tag/v1.10.5) | All 184 Windows PE files in the ZIP passed Authenticode and timestamp verification (2026-09-26). The Ed25519 update manifest verified against the application's existing public key. |
| PC Tweaker Uninstaller | [0.8.3](https://github.com/AurelioAvila/pc-tweaker-uninstaller/releases/tag/v0.8.3) | Valid signatures and timestamps on EXE and MSI installers, the EXE alias and extracted executable payloads. Both installer updater signatures verify. Historical 0.8.2 installers remain unsigned. |

The verified signed files identify **Aurelio Avila** as publisher and use a
certificate issued by **Certum Code Signing 2021 CA**. Timestamp certificates
are present. A ZIP itself is not an Authenticode-signed executable; extract it
and verify the files inside.

## Verify a download

### Redaxa 0.4.4 and Redexa Social 1.10.5 evidence (2026-09-26)

| Release asset | SHA-256 |
| --- | --- |
| `Redaxa_0.4.4_x64-setup.exe` | `3696db5f07add74a2e6c01586aeb2d90bff3b86fb84e784708863a2946bc739c` |
| `Redaxa_0.4.4_x64_en-US.msi` | `61129a22f0e579a54f07736ff68486396dcdc566611b753c953fd9af58b84a69` |
| `Redexa-Social-v1.10.5-win64.zip` | `6a69a7e32fbf3a102f3d6f74241441e0d810cf765e53d48743fe83d04e490c0f` |

Redaxa's final EXE and MSI installers and the executable payloads extracted from
both installers passed Windows Authenticode and timestamp verification. Public
installer downloads were checked again after release; their hashes matched the
verified files and GitHub asset digests. Both detached Tauri signatures verified
against the existing public key. `latest.json` points to version 0.4.4 and includes
`windows-x86_64`, `windows-x86_64-nsis` and `windows-x86_64-msi` targets.

Redexa Social's final ZIP contains 184 Windows PE files. All passed Authenticode
and `signtool verify /pa /all /tw` verification, including the application,
updater, compatibility launcher and bundled native libraries. The released ZIP
hash matches the verified archive and GitHub asset digest. Its Ed25519 update
manifest passed verification against the application's existing public key.

Both releases identify **Aurelio Avila** as publisher, using certificate thumbprint
`4F8341A74D16077AE1849DC8B8CAC99F22606754`, issued by **Certum Code Signing 2021 CA**,
with trusted timestamps. A valid update manifest proves its signature and package
binding; it does not by itself prove an end-to-end update on every older client.
The ZIP is an archive, not an Authenticode-signed executable. Chrome Web Store
extension signing is a separate distribution channel and is not covered here.

### PC Tweaker 1.10.3 evidence (2026-09-08)

| Downloaded asset | SHA-256 |
| --- | --- |
| `pc-tweaker-app_1.10.3_x64-setup.exe` and `PCTweaker-Setup.exe` | `114ec41c291f28df91278769ae8519f1b2a6aee2efa68a0378daddb38ed9fef2` |
| `pc-tweaker-app_1.10.3_x64_en-US.msi` and `PCTweaker-Setup.msi` | `6fc3efd06a140974ef3fd643f4d01c112cbaf31ffbd679d17119a6b707d2bd40` |

Each of these four downloads passed Windows Authenticode verification and
`signtool verify /pa /all /v /tw` with no errors or warnings. The signer is
**Aurelio Avila**, certificate thumbprint
`4F8341A74D16077AE1849DC8B8CAC99F22606754`, issued by Certum Code Signing
2021 CA. The timestamps chain to DigiCert; the timestamp responder is
**DigiCert SHA256 RSA4096 Timestamp Responder 2026 1**.
The downloaded files also match the local published-artifact evidence.
The EXE and MSI updater `.sig` files also passed cryptographic minisign
verification against the current PC Tweaker repository public key on 2026-09-08
(configuration blob `4f92e5cee70a278dd781ca7ed2c9050317197ead`). This is a
separate check from Windows publisher and timestamp verification.

### Redaxa 0.4.1 evidence (2026-09-23)

| Downloaded asset | SHA-256 |
| --- | --- |
| `Redaxa_0.4.1_x64-setup.exe` | `234d896db504deeaf2b3918f9d82918db3e1df43f01f91094f05d693fb6184f1` |
| `Redaxa_0.4.1_x64_en-US.msi` | `f75daf5377590666141a001b77fd67586a3a5889fe0a0ca2299da7caee1aeab9` |

Both sampled installers were downloaded from the v0.4.1 release and matched
GitHub's asset digests. Windows Authenticode reported `Valid`, publisher
**Aurelio Avila**, the Certum code-signing certificate above, and a trusted
Certum timestamp. The detached Tauri updater signature verified against the
source public key; `latest.json` contained the same signature and installer URL.
This is evidence for these exact release files, not for later builds or mirrors.

### Redaxa 0.3.3 and Redexa Social 1.9.3 historical evidence (2026-09-08)

| Downloaded asset | SHA-256 |
| --- | --- |
| `Redaxa_0.3.3_x64-setup.exe` | `e3c21449aceb7aaa8d63a493301b223b02cea98a7959ed9ae40f59530eff972e` |
| `Redaxa_0.3.3_x64_en-US.msi` | `431dcbe48aed9a7a977481a6c8fd0102533d7ffab8b40be684647aff29a65cc1` |
| `Redexa-Social-v1.9.3-win64.zip` | `82d4729822b6d5ba8abda18fd88f19c44258bd306df76bfa3c2cb8d458e170a2` |

Both Redaxa installers and the extracted Social files `Redexa Social.exe`,
`Social Dashboard.exe` and `updater.exe` passed Authenticode and SignTool
verification with no warnings or errors. They identify Aurelio Avila with the
same Certum code-signing certificate listed above and trusted timestamps from
**Certum Timestamp 2026**. The ZIP hash identifies the downloaded archive;
the Authenticode result applies to those three contained executables.
Redaxa's EXE and MSI updater `.sig` files also passed minisign verification
against its current repository public key on 2026-09-08 (configuration blob
`68a7368ba44d40af951f10552d656b178da392ba`). No signing session was used.

### New signed maintenance releases (2026-09-08)

| Downloaded asset | SHA-256 |
| --- | --- |
| `PC.Tweaker.Uninstaller_0.8.3_x64-setup.exe` and `PCTweakerUninstaller-Setup.exe` | `34eb17dab7b1acc87a2440ced7286726a45d4095f1db4b59305931b5776471d1` |
| `PC.Tweaker.Uninstaller_0.8.3_x64_en-US.msi` | `60ca4ad8f8cde0587438f4c45c52bcc1c2820c366599cb3da8143831dd79d3be` |
| `Redexa-Social-v1.9.4-win64.zip` | `9ea98c8117f7238e39a3855bd5ca538fa37ec52801dc8222e035881112d276cc` |

These releases were signed locally using the owner's existing Certum certificate
and DigiCert RFC 3161 timestamps, then downloaded and verified again after
publication. Uninstaller's EXE and MSI updater signatures and Redexa Social's
Ed25519 update manifest were independently verified. The Windows publisher
remains **Aurelio Avila**. No historical release assets were replaced.

### Check your file

In Windows, open the file's **Properties → Digital Signatures**, or use:

```powershell
Get-AuthenticodeSignature -LiteralPath '.\Downloaded-Setup.exe' |
    Format-List Status, StatusMessage, SignerCertificate, TimeStamperCertificate
```

Expect `Valid` and the publisher **Aurelio Avila** for a file listed as signed.
If verification fails, stop and investigate the exact download and version.
Do not disable antivirus protection to install it.

An updater `.sig` file or signed JSON manifest is separate from Windows
Authenticode. A checksum checks file consistency; it does not establish
publisher identity by itself. Neither a valid signature nor WinGet distribution
guarantees that SmartScreen will show no warning or that software has no
vulnerabilities.

## Scope across repositories

This inventory covers distributed Windows binaries. Web services, browser
extensions, security labs, source-only repositories and temporary media hosting
do not inherit a Windows code-signing claim from another product. Historical
release notes describe their original release and are not current signing status.
