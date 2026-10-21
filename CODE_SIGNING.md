# Windows code-signing status

Rows were checked on the dates noted below against the specific release packages.
The release API confirmed the listed versions as latest at the time of each
verification; package hashes matched its SHA-256 digests. Each evidence section
describes the files and verification scope.
Signing claims apply to the versions and artifacts listed here, not every
historical release or development build.

| Product | Verified release | Windows Authenticode status |
| --- | --- | --- |
| PC Tweaker | [1.18.8](https://github.com/AurelioAvila/pc-tweaker-app/releases/tag/v1.18.8) | 2026-10-21: EXE and MSI installers (and both stable download aliases, same bytes) match GitHub API digests. `signtool verify /pa /all /tw` passed on Windows with zero warnings or errors; see the 2026-10-21 evening evidence below. |
| Redaxa | [0.6.3](https://github.com/AurelioAvila/redaxa/releases/tag/v0.6.3) | 2026-10-21: EXE and MSI installers, the `Redaxa-Setup.exe` alias and the `Redaxa.exe` inside the MSI passed `signtool verify /pa /all /tw` on Windows with zero warnings or errors; Tauri updater signatures verified. |
| Redexa Social | [1.11.3](https://github.com/AurelioAvila/redexa-social/releases/tag/v1.11.3) | 2026-10-21: Setup EXE and win64 ZIP match GitHub API digests. `signtool verify /pa /all /tw` passed on the Setup EXE with zero warnings or errors; every PE file in the ZIPs was signed and verified at build time (Certum timestamps). |
| PC Tweaker Uninstaller | [0.13.4](https://github.com/AurelioAvila/pc-tweaker-uninstaller/releases/tag/v0.13.4) | 2026-10-21: EXE and MSI installers and the stable alias match GitHub API digests. `signtool verify /pa /all /tw` passed with zero warnings or errors; the application inside the MSI is signed and timestamped; Tauri updater signatures verified. |
| Tweaky Driver | [0.1.23](https://github.com/AurelioAvila/Tweaky-Driver-Releases/releases/tag/v0.1.23) | 2026-10-21: NSIS installer and the stable alias match GitHub API digests. `signtool verify /pa /all /tw` passed with zero warnings or errors; payload and uninstaller signatures checked by the release build; updater signature verified with minisign. |
| Frame Witness | [0.33.0](https://github.com/AurelioAvila/frame-witness/releases/tag/v0.33.0) | 2026-10-05: Final installer matches the official SHA-256 digest. SignTool verified Aurelio Avila and a trusted Certum timestamp, with zero warnings or errors. |

The verified signed files identify **Aurelio Avila** as publisher and use a
certificate issued by **Certum Code Signing 2021 CA**. Timestamp certificates
are present. A ZIP itself is not an Authenticode-signed executable; extract it
and verify the files inside.

## Verify a download

### Releases of 2026-10-21 evening

| Release asset | SHA-256 |
| --- | --- |
| `pc-tweaker-app_1.18.8_x64-setup.exe` / `PCTweaker-Setup.exe` | `8a92a80ed77f6d56e31e630e1694d85a9497d4ca95ce8b775826a872288a5a13` |
| `pc-tweaker-app_1.18.8_x64_en-US.msi` / `PCTweaker-Setup.msi` | `28890cbd42939fd7c3da9fb71149d9ef552829ea45b4ef45318b77f92a30abbc` |
| `PC.Tweaker.Uninstaller_0.13.4_x64-setup.exe` / `PCTweakerUninstaller-Setup.exe` | `0944fb54ee58c1f7df4e09d5934962cb40ada3edb7e520683c4e8bad712dfcf3` |
| `PC.Tweaker.Uninstaller_0.13.4_x64_en-US.msi` | `4a5182255e37de99c3eb79841d57941428ff774fa332d2625d34b15b4e83e428` |
| `Tweaky-Driver-0.1.23-x64-setup.exe` / `TweakyDriver-Setup.exe` | `f47b31f8c7e98f44f5a481c8dc2ba8fcfdc4e8fd8a65a636f4cf314f1efe6d08` |
| `Redaxa_0.6.3_x64-setup.exe` / `Redaxa-Setup.exe` | `4d66556ce67ea9c4484c647763f8f48e54b75463568c3351f986300d0647e5fc` |
| `Redaxa_0.6.3_x64_en-US.msi` | `a3e31788c22ca18de8dd2419e37f65d633faa2ac81f3db0807469b82939398df` |
| `Redexa-Social-Setup.exe` (1.11.3) | `a2b5d01994446fbfbaf7d4bbd462819429b1e5a2354a45f6730db74b0b3854b9` |
| `Redexa-Social-v1.11.3-win64.zip` | `d3013156420e11b48e9c7b563651fb5c7c40c27215129c10a8f1e344d9ba5570` |

Each file was downloaded again from its published GitHub release (the release API
listed each tag as latest) and matched the API digest. `signtool verify /pa /all /tw /v`
on Windows 11 verified each installer with zero warnings and zero errors, signer
**Aurelio Avila** (`4F8341A74D16077AE1849DC8B8CAC99F22606754`) with a trusted timestamp.
Before publication the release gates verified the application binaries inside the
packages and the Tauri updater signatures (Redexa Social: signed update manifests
from the protected key). Each release was also installed over the previous version on
a Windows 11 PC.

### PC Tweaker 1.18.7 evidence (2026-10-21)

| Release asset | SHA-256 |
| --- | --- |
| `pc-tweaker-app_1.18.7_x64-setup.exe` / `PCTweaker-Setup.exe` | `cf59f368475ac519b3075d5086307b91d8abbc3ffe935e9a38a867e5e628adab` |
| `pc-tweaker-app_1.18.7_x64_en-US.msi` / `PCTweaker-Setup.msi` | `3f81e84bcd28d469424b4997f020eace942440b58f7512b11196a7c8cd807394` |

Downloaded from the published release and the stable aliases; every hash
matches the GitHub release API digest. `signtool verify /pa /all /tw /v` on
Windows 11 (SignTool 10.0.26100) verified one signature on each installer with
zero warnings and zero errors: signer **Aurelio Avila**
(`4F8341A74D16077AE1849DC8B8CAC99F22606754`) through Certum Code Signing 2021 CA
to Certum Trusted Network CA 2, timestamp by DigiCert SHA256 RSA4096 Timestamp
Responder 2026 1. `certutil -urlfetch -verify` on the signer certificate passed
the leaf revocation check (Certum CRL and OCSP). The release build passed the
same `make-latest-json.mjs` gate and CI installer job as 1.18.6.

### PC Tweaker 1.18.6 evidence (2026-10-21)

| Release asset | SHA-256 |
| --- | --- |
| `pc-tweaker-app_1.18.6_x64-setup.exe` / `PCTweaker-Setup.exe` | `ce0ea348fb72c40fa8324c424ce7c773cdfc35b3a3f1334a12584ef95940383d` |
| `pc-tweaker-app_1.18.6_x64_en-US.msi` / `PCTweaker-Setup.msi` | `0fdc3e0d01c45dfe314559f42b636c988db530314628774f0946c8b321974310` |

Both installers and both stable aliases were downloaded from the published
release; each hash matches the SHA-256 digest the GitHub release API reports.
Checked on Windows 11 with SignTool 10.0.26100 (`signtool verify /pa /all /tw /v`):

```
Signing Certificate Chain:
    Issued to: Certum Trusted Network CA 2
        Issued to: Certum Code Signing 2021 CA
            Issued to: Aurelio Avila
            SHA1 hash: 4F8341A74D16077AE1849DC8B8CAC99F22606754
Timestamp Verified by:
    Issued to: DigiCert Assured ID Root CA
        Issued to: DigiCert Trusted Root G4
            Issued to: DigiCert Trusted G4 TimeStamping RSA4096 SHA256 2025 CA1
                Issued to: DigiCert SHA256 RSA4096 Timestamp Responder 2026 1
Successfully verified: pc-tweaker-app_1.18.6_x64-setup.exe
Number of signatures successfully Verified: 1
Number of warnings: 0
Number of errors: 0
```

The MSI gave the same result (one signature, zero warnings, zero errors).

- Revocation: SignTool's chain check passed, and `certutil -urlfetch -verify` on
  the signer certificate fetched Certum's CRLs (`ccsca2021.crl.certum.pl`,
  `crl.certum.pl/ctnca2.crl`) and OCSP responses (`ccsca2021.ocsp-certum.com`,
  `subca.ocsp-certum.com`) and reported the leaf revocation check as passed.
  For the timestamp certificate, DigiCert's intermediate CRLs and OCSP verified,
  but certutil reported the responder certificate's own revocation status as
  `CRYPT_E_REVOCATION_OFFLINE` at the time of the check.
- Before publication the release build passed `scripts/make-latest-json.mjs`
  (Authenticode, publisher, timestamp and Tauri updater signatures on the
  application and both installers), and the CI installer job installed both
  formats, upgraded from 1.15.2, started the app and uninstalled it.

### Redaxa 0.6.2 evidence (2026-10-21)

| Release asset | SHA-256 |
| --- | --- |
| `Redaxa_0.6.2_x64-setup.exe` / `Redaxa-Setup.exe` | `5737c63436a61fb5565d3675ca634c4bca7d1d0fcf729a900968bb4ae4e3e7b1` |
| `Redaxa_0.6.2_x64_en-US.msi` | `d98baa15d027fbad1240e194689f481effba8f106953cd261af5920c8a0c4644` |
| `Redaxa.exe` inside the MSI | `a1a28cc3feb262b6519431e03e3a9e112e2e0cfa78fc74cb52a997d3aef6f640` |

Checked on Windows 11 with SignTool from the Windows SDK 10.0.26100, using
`signtool verify /pa /all /tw /v` on each file, before publication:

- The chain runs Aurelio Avila → Certum Code Signing 2021 CA → Certum Trusted
  Network CA 2 → Certum Trusted Network CA. The signer thumbprint is
  `4F8341A74D16077AE1849DC8B8CAC99F22606754`, and the signature uses SHA-256.
- Each file has an RFC 3161 timestamp from "Certum Timestamp 2026" (Certum
  Timestamping 2021 CA), dated 21 October 2026.
- Each file reported "Number of signatures successfully Verified: 1", 0
  warnings and 0 errors. SignTool checks revocation online by default; for the
  signer certificate, `certutil -urlfetch -verify` reached the Certum CRL
  (`http://ccsca2021.crl.certum.pl/ccsca2021.crl`) and the OCSP responder
  (`http://ccsca2021.ocsp-certum.com`) and passed the leaf revocation check.
- The third-party executables in the MSI, `node.exe` (OpenJS Foundation) and
  `git.exe` / `git-remote-https.exe` (Johannes Schindelin), keep their own
  verified Microsoft-chained signatures.
- The Tauri updater signatures (`.sig`) of both installers verify against the
  public key shipped in the app.

After publication, the SHA-256 digests that the GitHub release API reports for
both installers, their `.sig` files and `latest.json` matched the checked files.
The update feed (`releases/latest/download/latest.json`) serves 0.6.2.
The same build updated a real per-user 0.5.4 installation to
`C:\Program Files\Redaxa\Redaxa.exe`, with settings and sign-in kept. That update
used the setup's updater arguments, not the in-app button.

### Current release evidence (2026-10-20)

| Release asset | SHA-256 |
| --- | --- |
| `pc-tweaker-app_1.18.5_x64-setup.exe` / `PCTweaker-Setup.exe` | `4a61898313e0d717b149572506e41da062307c85d9dcea0b1e08ea86076afed3` |
| `pc-tweaker-app_1.18.5_x64_en-US.msi` / `PCTweaker-Setup.msi` | `53cf456d0d1d0caab34c5503cbce57bcfecf9aa62787bccdc232527850f1df2b` |
| `Redaxa_0.6.0_x64-setup.exe` / `Redaxa-Setup.exe` | `c4f3b84834ae4f945c219e1c89ac6083d39c0dc93762d4290ef284c535816acc` |
| `Redaxa_0.6.0_x64_en-US.msi` | `4c15120d71c82ff4a8032eac34e85c0c8d42bb099531a4f91c074f97b338b15b` |
| `Redexa-Social-Setup.exe` | `d84f050a6c2c65382ad82c9bf9830d6e064d3cfb7cdf0ec6e995b326cf76824f` |
| `Redexa-Social-v1.11.1-win64.zip` | `6d9e9eed19248f3d57fb64b898af659853f1fa26cbe1fbb3347bf6c7bebd8a6b` |

Each file was downloaded from its latest release and matched the SHA-256 digest
the GitHub release API reports. Signatures were checked with `osslsigncode` 2.8
on Linux, not with SignTool:

- The Authenticode message digest recalculated from each file matches the signed
  digest.
- The signer is **Aurelio Avila**, certificate thumbprint
  `4F8341A74D16077AE1849DC8B8CAC99F22606754`, chaining through **Certum Code
  Signing 2021 CA** to Certum Trusted Network CA 2.
- RFC 3161 timestamps chain to DigiCert Trusted Root G4 (PC Tweaker, responder
  "DigiCert SHA256 RSA4096 Timestamp Responder 2026 1") and to Certum Trusted
  Network CA 2 (Redaxa and Redexa Social, "Certum Timestamp 2026").
- Redexa Social's ZIP contains 44 PE files (EXE, DLL and PYD); every one passed
  the same digest, signer and timestamp checks.

Certificate revocation (CRL) was not checked: the CRL servers were unreachable
from the verification environment. Tauri and Ed25519 updater signatures were
not verified in this pass, and the files were not executed or installed. Run
`signtool verify /pa /all /tw` on Windows for a full check including revocation.

### Earlier release evidence (2026-10-05)

| Release asset | SHA-256 |
| --- | --- |
| `pc-tweaker-app_1.15.9_x64-setup.exe` / `PCTweaker-Setup.exe` | `5c8eefe004d278922a6e9da772ff7cd7513add61098c7d0a089969102b4023a3` |
| `pc-tweaker-app_1.15.9_x64_en-US.msi` / `PCTweaker-Setup.msi` | `356650bce853bbd31e54507bf5234cfe785b16486ebc87009966dc5f07c3bf6d` |
| `Redaxa_0.4.7_x64-setup.exe` | `56b3a4c70d0156fb2d56aaf461083d8cc3723a0c26ff8c9e692a3d1c202e6177` |
| `Redaxa_0.4.7_x64_en-US.msi` | `f38b10e90c02c84e4fc9c17babb8d19626a106d736bfbb71ec85779be9093401` |
| `Redexa-Social-v1.10.9-win64.zip` | `64e246c6b90e7919b6f1f2fecea39a688d7a5a691916668b84af895149397ce2` |
| `PC.Tweaker.Uninstaller_0.12.2_x64-setup.exe` | `aef8cc74cea77d886f22cd160c96641248d8cc489c88e7ab1361b81544ad89e0` |
| `Tweaky-Driver-0.1.11-x64-setup.exe` | `badb3ae437c7dcf55998ef6320999905d00220aac088519371164a4c92990592` |
| `FrameWitness-Setup-0.33.0.exe` | `b5001c50182265ab06425b986c913095bcb1c783a84960097d46be71ef59d27c` |

PC Tweaker's four final downloads and Redexa Social's ZIP plus the application,
compatibility launcher and updater executables passed the release checksum and
Windows publisher-signature checks. Update metadata pointed to the listed versions;
metadata consistency is not cryptographic verification of an updater signature.

Redaxa's two downloaded installers, Uninstaller's final NSIS installer, Tweaky's
downloaded NSIS installer and Frame Witness's final installer also passed
`signtool verify /pa /all /tw`. These checks verified publisher identity and trusted
timestamps; the files were not executed or installed during this inspection.

This inspection did not repeat complete payload extraction audits, clean Windows
installation tests, cryptographic updater-signature checks or upgrades from older
clients. Historical evidence below retains its original date and scope and must
not be treated as verification of later releases.

### Redaxa 0.4.5 and Redexa Social 1.10.5 evidence (2026-09-26)

| Release asset | SHA-256 |
| --- | --- |
| `Redaxa_0.4.5_x64-setup.exe` | `002ab86f6159869aa7acd11e9725c66dc967a63df19339a456b83eb0cf4f0ae0` |
| `Redaxa_0.4.5_x64_en-US.msi` | `af8e34b7e805a0c37a0a1f2982040e3204f04bd8428b68481b5875fe05d29e1d` |
| `Redexa-Social-v1.10.5-win64.zip` | `6a69a7e32fbf3a102f3d6f74241441e0d810cf765e53d48743fe83d04e490c0f` |

Redaxa's final EXE and MSI installers and the executable payloads extracted from
both installers passed Windows Authenticode and timestamp verification: 40 files,
all with valid signatures and Certum timestamps. The release source is commit
[`091a85b`](https://github.com/AurelioAvila/redaxa/commit/091a85b11ac112988d676863c7e2f34021696fd7). Public
installer downloads were checked again after release; their hashes matched the
verified files and GitHub asset digests. Both detached Tauri signatures verified
against the existing public key. `latest.json` points to version 0.4.5 and includes
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
