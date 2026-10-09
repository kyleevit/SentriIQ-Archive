# SentriIQ Archive

## Version 1.3.0 — archive tools and Windows multi-user isolation

**Archive tools** adds add/delete/rename in verified copies, ZIP/7z/TAR conversion, batch file compression, batch extraction, duplicate-entry detection, checksum/contents reports, generic file split/join with per-part verification, and full-copy recovery snapshots. Originals are preserved by editing/conversion. Compression settings now include dictionary size, CPU threads and RAR recovery-record percentage; creation adds TGZ/TBZ2/TXZ containers. Installed Rar.exe is detected automatically, but RAR-specific writing/repair still requires a valid license and that engine.

Each Windows account has an independent private AppData profile and unique temporary workspace. Legacy preferences migrate only within the same account. Machine installation and Explorer registration remain shared. Output lock files protect concurrent writers; edits save copies. Installed updates are deferred while another user/session is running the app, installer runs are coordinated globally, and no other user's process is terminated. Preview registration is serialized. Do not share one portable executable across writable untrusted accounts; use the machine installer for shared/RDS deployments.

See [FEATURE-MATRIX.md](FEATURE-MATRIX.md) for the audited capabilities and explicit gaps. This release does not claim all proprietary vendor features, hosted SafeShare/cloud-provider services, PDF editing suites, scheduled unattended jobs, FIPS certification or every Outlook extension model. Tests simulate separate account/profile roots and write contention; production RDS/server deployment still requires environment validation.

A Windows Electron archive manager designed around a large, instant file preview and a layered glass interface. Everything is processed locally.

## Install

Download the [portable executable](https://github.com/kyleevit/SentriIQ-Archive/releases/latest) and run it: **no Electron, Node.js or application installation is needed**. Electron is bundled internally. Keep the EXE in a writable folder for portable updates. Alternatively run `release/SentriIQ-Archive-1.2.1-Setup.exe` for the guided, machine-wide installer (administrator access required). Target: x64 Windows 11 and Windows Server with Desktop Experience. Server Core has no supported graphical desktop. Windows Server compatibility requires validation on your server version; development checks run on Windows 11.

The installer registers archive file associations and an Explorer **Preview with SentriIQ Archive** menu. On Windows 11 this menu may appear under **Show more options**. Choose the application through **Open with** or Windows default apps. Windows retains the user's existing default application until the user changes it. Other programs can launch the executable with an archive path; this is a standalone application, not a plug-in embedded in WinRAR or other archive tools.

## Features

- Open archives through a file dialog, drag and drop, Explorer or the command line.
- Read ZIP, RAR, 7z, LHA/LZH, TAR, GZIP, BZIP2, XZ, ZSTD, CAB, ISO, ARJ, CPIO and other formats supported by the bundled full 7-Zip engine.
- Preview PNG/JPEG/WebP/GIF/BMP/AVIF images, multi-page PDFs, DOCX document text, XLSX cell values, PPTX slide text, text/source files, CSV/TSV text, and Chromium-compatible audio/video.
- Browse folders, search all paths, filter by file type, switch list/grid layouts, expand the preview and keep multiple archives open.
- Browse nested archives; extract a selected file or all contents; test archive integrity.
- Create ZIP, 7z and TAR archives from files or folders, with compatible compression methods, password encryption, split volumes and self-extracting 7z archives.
- Encrypted archive passwords are kept in memory only. Sample workspace is explicitly marked and separate from actual archives.

## ISO Studio and bootable USB

Version 1.2.0 adds **Settings & integration**, with saved archive profiles and a native Windows preview component. See the compatibility section below for host-specific limits.

Open **ISO Studio**, select a source folder, choose an image type and save your ISO outside the source folder. Windows' built-in IMAPI2 engine creates the image; no ISO utility installation is needed. **UDF** supports large Windows installation files; **ISO 9660 + Joliet** is available for smaller general-purpose disc images. Junctions and symbolic links in the source tree are rejected.

**Bootable Windows · BIOS + UEFI** detects `boot/etfsboot.com` and `efi/microsoft/boot/efisys.bin` (or `efisys_noprompt.bin`) in a complete extracted Windows installation source. It creates both El Torito boot entries. **Custom bootable ISO** accepts a BIOS boot image and/or UEFI FAT image. Boot images must be 512 bytes to 64 MB. Booting also requires the operating-system files and loader configuration; selecting bootable mode cannot turn arbitrary documents into an operating system. Custom Linux image creation does not add a hybrid USB partition table, GRUB or Syslinux automatically.

**Make a bootable USB** opens the bundled official, signed **Rufus 4.15** portable companion with the selected ISO preloaded. Rufus runs in its own native window. Choose the device, partition scheme and firmware target, then review and confirm writing in Rufus. Windows requests administrator permission for the writer. **Writing erases the selected USB device.** Rufus supplies Windows/Linux ISO support, GPT/MBR, UEFI/BIOS and its normal image-writing modes; it does not add boot files to a non-bootable data ISO. Hardware boot compatibility and Secure Boot depend on the source OS, boot files and target firmware.

## Automatic updates

The public release channel is [GitHub Releases](https://github.com/kyleevit/SentriIQ-Archive/releases). Packaged builds check on startup and every four hours while open. Automatic checks/downloads can be disabled through **Updates**. The app verifies an Ed25519-signed manifest and each EXE's SHA-512 hash before staging an update. Unpublished or unreachable feeds show an error rather than claiming the app is current.

Ready updates apply when the app closes, or with **Restart & update**. Portable updates replace the original EXE and retain a `.previous` backup. Installed updates run the NSIS installer and may request Windows administrator permission. No update is applied while an ISO is being created. No updater runs when the app is closed; publishing a new version is required for an update to exist. These update signatures protect release contents; they are separate from Windows Authenticode signing. SentriIQ itself remains unsigned.

Release signing uses the local ignored `.release-secrets/update-private.pem`. Back up this private key securely; never publish it. `src/update-config.json` contains only the verification public key and public feed URL. After building a version, run `pnpm release:manifest`, then upload both EXEs and `release/manifest.json` to one GitHub release. Existing clients must retain the same public key across versions.

## Settings, compatibility profiles and email previews (1.2.0)

### Explorer right-click commands (1.2.1)

Right-click a file or folder and choose **Compress with SentriIQ…** to use the saved compression profile and creation dialog with that source already selected. Right-click inside a folder to compress that folder. For archives, use **Uncompress to new folder here**, **Uncompress to…**, or **Preview archive with SentriIQ**. Extraction always creates a separate unique folder. The commands target one selected file/folder; the app's normal creation dialog also supports multiple file selection.

The installer registers these commands and the ZIP/archive preview handler. Portable users enable them through **Settings & integration → Enable Windows previews**, which requires administrator permission. Keep a registered portable EXE in the same location, or refresh integration after moving it. Windows 11 displays these classic shell verbs under **Show more options** (Shift+F10). Enable Explorer's preview pane with **Alt+P**, select a ZIP, then select an entry in the preview. Reopen Explorer after registration changes. Windows policies or downloaded-file restrictions may still block previews; the app does not disable them.

Settings saves per-user defaults for compression format, level/method, encryption, solid compression, encrypted filenames, split-volume size, self-extracting output, verify-after-create, app hover previews and size limits. Passwords are supplied for each operation and never saved in preferences. **Create archive** provides overrides and a file/folder source chooser.

| Profile | Result | Engine |
| --- | --- | --- |
| PKZIP / Windows compatible | Standard Deflate ZIP; optional legacy ZipCrypto | Bundled 7-Zip |
| WinRAR / WinZip secure ZIP | ZIP with AES-256 | Bundled 7-Zip |
| 7-Zip high compression | LZMA2 7z; solid blocks and encrypted filenames available | Bundled 7-Zip |
| WinRAR RAR | RAR creation, encryption and volumes | Your installed, licensed `Rar.exe` |

These provide common compatible settings, not every proprietary PKZIP/WinRAR feature. RAR writing needs your WinRAR engine; RAR reading remains available. Recovery records, archive repair, archive-entry editing, PKZIP certificate encryption and Outlook compose automation are not included. ZIP AES archives need an AES-capable extractor. ZipCrypto is weak encryption and is offered for legacy compatibility.

The installer registers a native Windows **IPreviewHandler** for archive extensions in both 32-bit and 64-bit registry views. Portable users can choose **Enable Windows previews** from Settings. This requests administrator permission and installs a small persistent component under `Program Files\SentriIQ Preview`; the Electron EXE stays portable. Disabling integration restores previous handler values if they are still owned by SentriIQ. Preview registration does not change the default archive-opening app. Reopen Explorer and classic Outlook after registration changes.

The native handler runs in its own Windows preview surrogate, retaining low-integrity isolation. It accepts files or attachment streams and displays a searchable archive list plus selected PNG/JPEG/GIF/BMP, text/code and Office document text. HTML/SVG display as source. It does not execute entries or extract archive-controlled paths. Native limits: 10,000 entries, 8 MB per selected entry, 1 MB text and 512 MB attachment streams. Encrypted, oversized, PDF and media entries need the full app for richer previews.

| Host | Integration |
| --- | --- |
| File Explorer preview pane | Archive contents and selected-entry previews |
| Classic Outlook desktop | Attachment preview when Outlook calls the registered Windows handler |
| Other Windows apps | Available when the host calls Windows preview handlers |
| Explorer hover | Standard filename, size and date information tips |
| Hover within SentriIQ | Optional delayed file preview |
| New Outlook / Outlook on the web | This COM handler is not used; save/open attachments in SentriIQ |
| Attachment hover in every app | Cannot be provided globally; the host controls its UI and events |

Trust Center, enterprise policies, Mark-of-the-Web restrictions and host support can prevent previews. SentriIQ does not disable those protections. Native 32/64-bit rendering, file/stream initialization and COM activation are tested in isolated harnesses; live Outlook mailbox workflows are not tested. This component provides a lightweight subset of the Electron preview.

## Practical limits

Not every compression format or file type can be previewed. RAR and LHA are extraction-only. DOCX and PPTX previews show text rather than exact Office layout. XLSX previews show cached cell values (no formula recalculation, formatting, dates or charts), with up to 12 sheets, 1,000 rows and 50 columns. PPTX previews show the first 50 slides. Legacy XLS, PPT and DOC require extraction and an associated application. HTML and SVG are shown as source to avoid running active archive content. Media playback depends on Chromium codecs. Images and PDFs may need the selected file decoded before appearing.

Default app preview/nested-archive limit: 80 MB per entry, configurable to 8–256 MB. Text shows the first 2 MB. Extraction streams to disk, with configurable limits (default 8 GB per file, 32 GB per operation). Extraction creates a new folder. Compression stages selected sources in a temporary workspace, requiring enough temporary disk space. Split outputs are saved in a new volume folder. No archive-entry editing or password recovery is included.

Unsafe paths, Windows device names, alternate data streams and archive links are rejected. Preview content runs in a sandboxed renderer with context isolation and a restricted preload API. Temporary files are removed on normal app exit; abrupt termination may leave temporary files in the user temp directory.

## Develop and build

Install Node.js and pnpm, then:

```powershell
pnpm install
pnpm start
pnpm test
pnpm smoke
pnpm build
```

The full Windows x64 7-Zip 26.04 engine is included under `vendor/7zip`, sourced from the [official release](https://github.com/ip7z/7zip/releases/tag/26.04). Its license and notices ship beside the executable. The independent Rufus companion ships with its GPL license and matching source archive under `resources/rufus`. Electron and application dependencies are locked by `pnpm-lock.yaml`.

Implementation references: [Microsoft IMAPI2 filesystem imaging](https://learn.microsoft.com/en-us/windows/win32/api/imapi2fs/nn-imapi2fs-ifilesystemimage), [multiple boot images](https://learn.microsoft.com/en-us/windows/win32/api/imapi2fs/nn-imapi2fs-ifilesystemimage2), [Rufus](https://rufus.ie/en/) and [Rufus's `-i` command-line implementation](https://github.com/pbatard/rufus/blob/v4.15/src/rufus.c).

Build outputs are in `release/`. The installer and portable executable are unsigned; production distribution should use your organization's code-signing certificate. Releases are published separately from the build, and no installation onto the development machine is performed.

Keyboard shortcuts: **Ctrl+O** open, **Ctrl+F** search, **Escape** leave expanded preview.



## Published source

[Download the complete 1.3.0 source bundle](https://github.com/kyleevit/SentriIQ-Archive/releases/download/v1.3.0/SentriIQ-Archive-1.3.0-Source.zip). It includes the application, native preview component, build scripts, tests and bundled engines. Use this named asset rather than the automatically generated tag archives for the current source.
