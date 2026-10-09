# SentriIQ Archive

A preview-first Windows archive manager with a layered glass interface, ISO Studio and bootable USB tools.

## Download and run

[Download the latest release](https://github.com/kyleevit/SentriIQ-Archive/releases/latest).

Choose **SentriIQ-Archive-1.1.0-Portable.exe** for no-install use. Run the EXE directly. Electron and Node.js do not need to be installed separately. Keep the EXE in a writable folder to allow automatic updates.

The **Setup.exe** alternative installs machine-wide and adds Windows Explorer integration.

## Features

- Browse ZIP, RAR, 7z, LHA/LZH, TAR, GZIP and other formats supported by the bundled 7-Zip engine.
- Preview images, PDFs, Word text, spreadsheet cells, slide text, code and compatible media inside archives.
- Search, browse nested archives, extract files and create ZIP/7z/TAR archives.
- ISO Studio creates UDF or ISO 9660/Joliet disc images from folders.
- Create bootable Windows ISOs with BIOS and UEFI entries from complete Windows installation sources; custom El Torito boot images are also supported.
- Prepare bootable USB drives using the bundled official signed Rufus 4.15 companion, opening in its own native window with the ISO preselected.
- Automatic update checks on startup and every four hours while open. Downloads require a verified Ed25519 signature and matching SHA-512 checksum, then apply on close. Portable updates retain the previous EXE as a backup.

Bootable images require genuine boot loaders and operating-system files. Writing USB media erases the selected device; review and confirm the device in Rufus. SentriIQ is unsigned; the independent Rufus companion is digitally signed by Akeo Consulting. Builds target x64 Windows 11 and Windows Server with Desktop Experience; server and physical boot compatibility require validation on the intended machine.

The release includes a detailed README, validation report and SHA-256 checksums. Rufus's license and matching source archive are bundled with the companion. Signed update metadata is published as `manifest.json`.
