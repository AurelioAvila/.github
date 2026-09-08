# Windows code-signing status

All rows were independently checked again on 2026-09-08 against release files
downloaded from GitHub. The release API confirmed the listed versions as latest
at the time of verification; downloaded asset hashes matched its SHA-256 digests.
Signing claims apply to the versions and artifacts listed here, not every
historical release or development build.

| Product | Verified release | Windows Authenticode status |
| --- | --- | --- |
| PC Tweaker | [1.10.3](https://github.com/AurelioAvila/pc-tweaker-app/releases/tag/v1.10.3) | Valid signatures and trusted timestamps on EXE and MSI installers, including both stable download aliases. SHA-256 values match GitHub API asset digests. |
| Redaxa | [0.3.3](https://github.com/AurelioAvila/redaxa/releases/tag/v0.3.3) | Valid signatures and timestamps on EXE and MSI installers. |
| Redexa Social | [1.9.3](https://github.com/AurelioAvila/redexa-social/releases/tag/v1.9.3) | Valid signatures and timestamps on the application, updater and compatibility launcher inside the ZIP. |
| PC Tweaker Uninstaller | [0.8.2](https://github.com/AurelioAvila/pc-tweaker-uninstaller/releases/tag/v0.8.2) | EXE and MSI installers, including the EXE alias, are not Authenticode-signed. |

The verified signed files identify **Aurelio Avila** as publisher and use a
certificate issued by **Certum Code Signing 2021 CA**. Timestamp certificates
are present. A ZIP itself is not an Authenticode-signed executable; extract it
and verify the files inside.

## Verify a download

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
This check covers the installers and aliases; it does not claim a separate
cryptographic verification of their Tauri updater `.sig` files.

### Redaxa 0.3.3 and Redexa Social 1.9.3 evidence (2026-09-08)

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
