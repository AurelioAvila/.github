# Windows code-signing status

Verified on 2026-09-06 against the release files downloaded from GitHub.
Signing claims apply to the versions and artifacts listed here, not every
historical release or development build.

| Product | Verified release | Windows Authenticode status |
| --- | --- | --- |
| PC Tweaker | [1.9.0](https://github.com/AurelioAvila/pc-tweaker-app/releases/tag/v1.9.0) | Valid signatures and timestamps on EXE and MSI installers, including the stable download aliases. |
| Redaxa | [0.3.3](https://github.com/AurelioAvila/redaxa/releases/tag/v0.3.3) | Valid signatures and timestamps on EXE and MSI installers. |
| Redexa Social | [1.9.3](https://github.com/AurelioAvila/redexa-social/releases/tag/v1.9.3) | Valid signatures and timestamps on the application, updater and compatibility launcher inside the ZIP. |
| PC Tweaker Uninstaller | [0.8.2](https://github.com/AurelioAvila/pc-tweaker-uninstaller/releases/tag/v0.8.2) | EXE and MSI installers, including the EXE alias, are not Authenticode-signed. |

The verified signed files identify **Aurelio Avila** as publisher and use a
certificate issued by **Certum Code Signing 2021 CA**. Timestamp certificates
are present. A ZIP itself is not an Authenticode-signed executable; extract it
and verify the files inside.

## Verify a download

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
