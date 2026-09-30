# CroceRossa_PDF_Encrypt
# PdfEncryptor

Encrypts PDF files so that they **open only with a password** (AES-256 or AES-128), one file or a
whole folder at a time, and records every encryption in a SQLite database. Windows desktop
application in C++20 with wxWidgets 3.3, SQLite and PoDoFo; x64, Debug / Release, static / DLL.

Copyright (C) 2026 Fation Coga - proprietary (LICENSE.md). User manuals: `docs\PdfEncryptor-Manual-EN.pdf`,
`docs\PdfEncryptor-Manuale-IT.pdf` (copied next to the exe by the build).

## What it does

- **What**: one PDF, or every PDF of a folder (optionally with its subfolders). Drop a file or a
  folder on the window to choose it. PDFs already encrypted are recognised and skipped.
- **Where**: the encrypted copies go to `encrypted_files\` next to the exe, with the same file name
  (and the same subfolders, if chosen). An existing file is never overwritten (`name_2.pdf`). The
  originals are never changed.
- **Password to open** (the "user" password - without it the file can't be opened at all):
  - fixed; or
  - fixed text + variable parts: *text before* + *part from the file name* + *separator* +
    *part from the creation date* + *text after* (date part first, optionally).
  - Parts from the name: whole name, first N / last N characters, only the digits, initials of the
    words, reversed, without vowels, a code of N hex digits from SHA-256 of the name (looks random,
    can be recomputed), number of characters; as written / lower / UPPER case.
  - Parts from the date: YYYYMMDD, YYYYMMDDhhmm, YYYYMMDDhhmmss, DDMMYYYY, DDMMYYYYhhmm, YYMMDD,
    hhmm, hhmmss, YYYY-MM-DD.
  - The date comes from the PDF's own CreationDate (as written in the file, the same on every PC),
    falling back to the file's creation time; or only from the PDF; or from the file's creation /
    modification time.
  - A live example shows the password the first file will get.
- **Protection**: AES-256 (recommended) or AES-128; permissions password random per file, the same
  as the open password, or fixed; allow printing / copying / editing; verification of every file
  written (it must refuse to open without the password and open with it, with the same pages).
- **History** (`<exe>\PdfEncryptor data\encryptions.db`, table `encryptions`): original file name
  and path, creation date and where it came from, date and time of the operation, open password,
  permissions password, encrypted file, algorithm, pages, verified, SHA-256 of the original, user,
  computer. The History tab searches it, copies passwords (double-click a row), shows the encrypted
  file and exports CSV (opens in Excel). Passwords are masked until "Show passwords" is ticked.
- Light / dark theme (Settings > Theme, or Ctrl-D), English / Italian (Settings > Language), window
  size remembered, application log at the bottom and in `PdfEncryptor data\log.txt`.
- **License**: per-PC licenses issued with LicGen (LICENSING.md); 14-day trial. Without a license
  the settings, the preview and the history stay available; encrypting needs the feature `encrypt`
  (single files) and `folder` (whole folders).


