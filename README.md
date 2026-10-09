# SentriIQ Archive

A Windows archive manager with in-archive previews, ISO Studio, compatibility profiles and Windows attachment preview integration.

[Download the latest release](https://github.com/kyleevit/SentriIQ-Archive/releases/latest).

The portable EXE includes Electron internally: no separate Electron/Node.js installation is required. Keep it in a writable folder for verified automatic updates. The Setup EXE adds Explorer integration and native Windows archive previews.

## Archive settings

Saved profiles provide common PKZIP/Windows ZIP, WinRAR/WinZip AES ZIP and 7-Zip LZMA2 settings. Configure compression level/method, passwords, encryption, encrypted filenames, solid blocks, split volumes, self-extracting 7z output, verification, hover previews and size limits. Passwords are never stored in preferences. RAR creation requires your own installed, licensed `Rar.exe`; reading RAR files uses the bundled engine.

## Windows and Outlook previews

Use Setup for native preview registration, or Portable → Settings & integration → Enable Windows previews. Registration requests administrator permission and installs a small persistent preview component. Previous handler values are backed up for restoration.

The native 32/64-bit component supports Explorer preview panes, classic Outlook attachment previews and other hosts that call Windows preview handlers. It shows archive contents plus selected images, text/code and Office document text. The full app offers richer PDF/media previews. Shell previews have separate limits: 8 MB selected entries and 512 MB attachment streams.

Hover previews are available inside SentriIQ. Explorer hover shows normal file information. Outlook and other programs control their own hover UI; no universal attachment-hover hook is provided. New Outlook and Outlook on the web do not use this COM integration: save/open the attachment in SentriIQ. Host policies, Trust Center and Mark-of-the-Web restrictions are respected.

## More features

Search archive contents; browse nested archives; extract files; create ZIP/7z/TAR archives; create data or BIOS/UEFI bootable ISOs from valid OS source files; prepare bootable USB media through the bundled official Rufus companion. Update downloads require a signed Ed25519 manifest and matching SHA-512 checksum before applying on close.

These are common compatible settings, not every proprietary PKZIP/WinRAR feature. Windows x64 builds are unsigned; the independent Rufus companion is signed. Native rendering, file/stream input and COM activation are tested in 32/64-bit isolated harnesses. Live Outlook workflows, physical boot and Windows Server require validation on the intended system.

Each release includes detailed usage and validation reports, checksums and signed update metadata. Third-party licenses and Rufus corresponding source are bundled in their resources.
