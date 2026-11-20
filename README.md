# HTML Export for DBeaver Community

[Español](README.es.md)

HTML Export adds an **HTML Export** button to DBeaver's SQL result viewer, next to *Export data*. One click turns
the rows on screen into a standalone HTML report that you can filter, sort and chart in Edge or Chrome, and copy
into Outlook or Teams. The arrow next to the button also offers **Export complete result**: after you confirm, it
runs the query once more and exports every row, within fixed limits.

This repository is the extension's **update site**. DBeaver installs and updates HTML Export from this address:

```
https://monosabio.github.io/html-export-update-site/
```

The same address, opened in a browser, shows the available version, its installable ZIP (no GitHub account needed),
its SHA-256 and the signing key. Use the ZIP only when DBeaver cannot reach the site
([Install without network access](#install-without-network-access)).

## Requirements

HTML Export is **only the DBeaver extension**. It does not include, download or install Python or the report
generator: both must already be installed on your computer before HTML Export can produce a report.

| Component | Required version | Notes |
| --- | --- | --- |
| DBeaver Community | **26.2.5** | Other DBeaver versions are not supported; DBeaver refuses the installation, saying that required items could not be found |
| Java | 21 or newer | The Java bundled with DBeaver 26.2.5 is enough; nothing else to install |
| Python | **3.10 or newer** | On Windows, the real `python.exe` (not the Microsoft Store alias or a `.bat` launcher) |
| DBeaver Export generator | **26.19.1**, unmodified | A separate project, installed separately. HTML Export checks 9 of its files against fixed SHA-256 values and refuses any other version or any modified copy, including line-ending changes |
| Operating system | Windows 11 x64 or Linux x86_64 | Other systems have not been tested |

This site does not distribute the DBeaver Export generator. Without it you can install HTML Export, but you cannot
generate reports.

## Install

1. Open DBeaver Community 26.2.5.
2. Choose **Help > Install New Software...**.
3. Click **Add...**.
4. Enter **Name** `HTML Export` and **Location** `https://monosabio.github.io/html-export-update-site/`, then
   click **Add**.
5. Check **HTML Export for DBeaver Community** in the list and click **Next >** twice.
6. On **Review Licenses**, read the Apache License 2.0, choose **I accept the terms of the license agreement** and click
   **Finish**.
7. DBeaver asks two security questions (see [Security questions](#security-questions)):
   - **Trust Authorities**: check only the row `https://monosabio.github.io` and click **Trust Selected**;
   - **Trust Artifacts** ("Do you trust these signers?"): check the **PGP** row only if its fingerprint is
     `BBA987AAB8D738A0548DB03A88A1DE796ADEF15A` (the [signing key](#signing-key)), then click **Trust Selected**.
8. When DBeaver asks *Restart DBeaver to apply the software update?*, click **Restart Now**.
9. Run a query in a SQL editor: the **HTML Export** button appears in the result toolbar, next to *Export data*.
10. Set the paths of Python and the DBeaver Export generator ([Configure](#configure)).
11. Click **Verify configuration**.

### Install without network access

1. On a computer with Internet access, open https://monosabio.github.io/html-export-update-site/ and click
   **Download ZIP**.
2. Optionally, check the file: its SHA-256 must be the one shown on the page and in
   [downloads/SHA256SUMS](https://monosabio.github.io/html-export-update-site/downloads/SHA256SUMS). For example,
   `Get-FileHash <file>.zip` (Windows PowerShell) or `sha256sum <file>.zip` (Linux).
3. In DBeaver, choose **Help > Install New Software...**, click **Add...**, then **Archive...**, select the ZIP and click
   **Add**. Do not unzip it.
4. Continue from step 5 of [Install](#install): the same license and the same security questions, with the same
   fingerprint.

DBeaver does not look for updates in a ZIP. To update later, add the site URL as in
[Migrating from a ZIP installation](#migrating-from-a-zip-installation), or install the next ZIP the same way.

## Configure

Open **Window > Preferences > Editors > Data Editor > HTML Export**:

- **Python executable**: the full path, for example `C:\Program Files\Python313\python.exe` or `/usr/bin/python3`.
- **DBeaver Export engine folder**: the folder of the installed DBeaver Export 26.19.1 generator, which contains
  `formatear_html_dbeaver_v7.py` and `report/`.
- **Output folder for HTML reports**: an existing folder of yours.
- Optionally, **Open the generated file automatically** and **Show a notification when finished**.

Click **Verify configuration**, then **Apply and Close**. The settings are kept when DBeaver restarts and when
HTML Export is updated.

## What the two export options do

- **Export loaded rows** (a click on the button): exports the rows DBeaver has already loaded, with the visible
  columns in their order. It never runs SQL again.
- **Export complete result** (the button's arrow): asks for confirmation, then **runs the result's query once
  more** and exports all of its rows. It accepts only a single read-only `SELECT` (or `WITH ... SELECT`) that it can
  verify; anything else is refused before anything runs. If the data changed so that the rows you already had are
  no longer the first rows of the new result, nothing is exported.
- Both options respect the same limits: at most **10,000 rows**, **256 columns**, **50,000 cells** and 2,000,000
  characters. Beyond a limit no report is written, not even a partial one.
- Re-running a query costs time on the database server and may be recorded in the database's audit or DBeaver's
  query history. Use *Export loaded rows* when that matters.

## Update

In DBeaver, **Help > Check for Updates** only looks for a new version of DBeaver itself; it does not update
extensions, and DBeaver does not check extensions on its own. To update HTML Export:

1. Choose **Help > Installation Information**, tab **Installed Software**.
2. Select **HTML Export for DBeaver Community** and click **Update...**.
3. When the newer version is listed, click **Next >**. If **Review Licenses** appears (the first time the Apache License 2.0
   is shown), accept it. Then click **Finish**.
4. Answer the security questions as when you installed it, then click **Restart Now**.

Your settings and your reports are not changed by an update. Older versions remain on this site: in
*Install New Software*, clear **Show only the latest versions of available software** and **Hide items that are
already installed** to see them.

## Migrating from a ZIP installation

If you installed an earlier HTML Export from a ZIP file (*Add > Archive*), DBeaver does not know this site yet, and
*Update...* would say *No updates found*. Do this once; there is no need to reinstall DBeaver or HTML Export:

1. Choose **Help > Install New Software...**, click **Add...**, enter **Name** `HTML Export` and **Location**
   `https://monosabio.github.io/html-export-update-site/`, then click **Add**.
2. When the list shows HTML Export, click **Cancel**. The site stays registered.
3. Choose **Help > Installation Information**, tab **Installed Software**, select HTML Export and click **Update...**.
4. Keep the new version checked, click **Next >**, accept the Apache License 2.0 on **Review Licenses**, click
   **Finish**, answer the security questions and click **Restart Now**.
5. Open **Window > Preferences > Editors > Data Editor > HTML Export**: your paths are still there. Click
   **Verify configuration**.

## Check the installed version

Choose **Help > Installation Information**, tab **Installed Software**. The row *HTML Export for DBeaver Community*
shows the version, for example `0.2.5.202611200208-5a52e82`. The [site page](https://monosabio.github.io/html-export-update-site/)
lists every published version with its ZIP and the ZIP's SHA-256.

## Uninstall

1. Choose **Help > Installation Information**, tab **Installed Software**.
2. Select **HTML Export for DBeaver Community** and click **Uninstall...**, then **Finish**.
3. Restart DBeaver.

DBeaver keeps working as before. Your reports and your settings are not deleted. To remove the update site too,
open **Window > Preferences > Install/Update > Available Software Sites** and remove *HTML Export*.

## Security questions

Installing over HTTPS guarantees that the files come unchanged from this site. It does not prove who published them,
so every version on this site is **signed with PGP**, and DBeaver asks before installing it:

- **Trust Authorities**: whether you trust content from `https://monosabio.github.io`.
- **Trust Artifacts**: *Do you trust these signers?*, with a **PGP** row that shows the key's fingerprint and name.
  Trust it only if the fingerprint is exactly the one below. DBeaver remembers the key (*Remember selected signers*),
  so later updates signed with it do not ask again.

If DBeaver shows *Do you trust unsigned content of unknown origin?* or a different fingerprint for HTML Export, click
**Cancel**: that is not a version published here.

**Never check "Always trust all content" or "Always trust all authorities"**: they turn these questions off for any
software from any site, now and in the future. The [site page](https://monosabio.github.io/html-export-update-site/)
also lists the SHA-256 of each published ZIP.

### Signing key

| | |
| --- | --- |
| Fingerprint | `BBA9 87AA B8D7 38A0 548D  B03A 88A1 DE79 6ADE F15A` |
| Without spaces | `BBA987AAB8D738A0548DB03A88A1DE796ADEF15A` |

DBeaver shows the fingerprint in lowercase without spaces, for example `bba987aab8d738a0548db03a88a1de796adef15a`.

## If the HTML Export button does not appear

1. Restart DBeaver once more.
2. Check **Help > Installation Information > Installed Software**. If HTML Export is not listed, install it again.
3. The button is in the toolbar of a **result** (SQL editor results or a table's data), not in the main toolbar.
4. Only DBeaver Community **26.2.5** is supported.
5. If DBeaver was started with `-clean` after installing, HTML Export may not load. Uninstall it, restart, and install
   it again.
6. If DBeaver cannot connect to the site, check **Window > Preferences > General > Network Connections** (proxy).

## If Verify configuration or an export fails

HTML Export shows a technical code and a short explanation, never your data. The most common ones:

- **Python not found / not executable / incompatible**: point *Python executable* to a real Python 3.10+ executable.
- **Engine folder not found / engine file missing**: point *DBeaver Export engine folder* to the folder that contains
  `formatear_html_dbeaver_v7.py` and `report/`.
- **Engine file modified**: a generator file differs from version 26.19.1. Restore an unmodified copy; on Windows,
  make sure Git did not convert line endings (`core.autocrlf`). Do not edit files to get around the check.
- **Limit exceeded**: reduce the loaded rows or the visible columns.

## Report a problem

Open an issue in this repository. Please include:

- the HTML Export version (see [Check the installed version](#check-the-installed-version));
- your DBeaver version and operating system;
- the error code HTML Export showed, if any.

**Do not attach real data, reports or screenshots with confidential information.**

## License

HTML Export is licensed under the [Apache License 2.0](LICENSE) (see also [NOTICE](NOTICE)). Copyright 2026 Gaston
Wierny. It does not include DBeaver or the DBeaver Export generator. DBeaver is a trademark of its owner; HTML Export is
an independent project.

## Privacy

HTML Export runs locally. It sends no telemetry and makes no network calls; reports are written only to the folder
you choose.
