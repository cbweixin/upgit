# ![upgit](https://cdn.jsdelivr.net/gh/pluveto/upgit/logo.png)

<img align="right" src="https://img.shields.io/github/workflow/status/pluveto/upgit/Release?logo=go&style=flat-square" />

<img src="https://img.shields.io/badge/Windows-0078D6?style=for-the-badge&logo=windows&logoColor=white" /> <img src="https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white" /> <img src="https://img.shields.io/badge/mac%20os-000000?style=for-the-badge&logo=apple&logoColor=F0F0F0" />



**Languages**: English / [简体中文](docs/README.zh-CN.md)



*Upgit* is a native & lightweight tool to helps you upload any file to your Github repository and then get a raw URL for it.

This is also useful with [Typora](https://support.typora.io/Upload-Image/#image-uploaders) as an image uploader.

## Feature

+ Support for Linux, Windows and macOS
+ Upload any file to given remote github repo folder
+ Upload from **clipboard**
+ Custom auto **renaming** rules
+ **CDN** via replacing rules
+ Config via **Environment Variable**
+ Output URL to stdout/clipboard, supports markdown image format

## Get started

### Download

Download it from [Release](https://github.com/pluveto/upgit/releases).

> If you have no idea which to download:
>
> + For most Windows users, choose `upgit_win_amd64.exe`
> + For most macOS users, choose `upgit_macos_arm64`

Rename it to `upgit` (For Windows users, `upgit.exe`), save it to somewhere you like. To access it from anywhere, add its directory to the `PATH` environment variable.

**Warning:** this program doesn't contain an auto-updater. If you need to keep updated, just give *upgit* a ⭐star.

### Config

Create `config.toml` in the same directory of *upgit*, and fill it in following [this sample config file](https://github.com/pluveto/upgit/blob/main/config.sample.toml).

### Use it

To upload file `logo.png` with rename rules, execute:

```shell
./upgit logo.png
# for windows: .\upgit.exe logo.png
```

Then you'll see a link to `logo.png`.

To upload file `logo.png` to remote folder `/my_images/demo`, execute:

```shell
./upgit logo.png -t /my_images/demo
# for Windows: .\upgit.exe logo.png -t /my_images/demo
```

---

For more help, type `-h` argument

```

Upload anything to github repo and then get its link.
For more information: https://github.com/pluveto/upgit

Usage: upgit.exe [--target-dir TARGET-DIR] [--verbose] [--size-limit SIZE-LIMIT] [--wait] [--clean] [--raw] [--no-log] [--output-type OUTPUT-TYPE] [--output-format OUTPUT-FORMAT] FILE [FILE ...]

Positional arguments:
  FILE                   local file path to upload. :clipboard for uploading clipboard image

Options:
  --target-dir TARGET-DIR, -t TARGET-DIR
                         upload file with original name to given directory. if not set, will use renaming rules
  --verbose, -V          when set, output more details to help developers
  --size-limit SIZE-LIMIT, -s SIZE-LIMIT
                         in bytes. overwrite default size limit (5MiB). 0 means no limit
  --wait, -w             when set, not exit after upload, util user press any key
  --clean, -c            when set, remove local file after upload
  --raw, -r              when set, output non-replaced raw url
  --no-log, -n           when set, disable logging
  --output-type OUTPUT-TYPE, -o OUTPUT-TYPE
                         output type, supports stdout, clipboard [default: stdout]
  --output-format OUTPUT-FORMAT, -f OUTPUT-FORMAT
                         output format, supports url, markdown and your customs [default: url]
  --help, -h             display this help and exit
```

### Use it for Typora

> Assuming your *upgit* program is saved at `"C:\repo\upgit\upgit.exe"`.

Select *File > Preferences...*

![image-20220128204217802](https://cdn.jsdelivr.net/gh/pluveto/0images@master/2022/01/upgit_20220128_1643373863.png)

Move to *Image*. Choose *Custom Command* as your *Image Uploader*.

Input *upgit* program location into *Command* textbox.

> You can click *Test Uploader* button to make sure it works.

![image-20220128204418723](https://cdn.jsdelivr.net/gh/pluveto/0images@master/2022/01/upgit_20220128_1643373868.png)

Now enjoy it!

### Upload Clipboard

Use `:clipboard` place holder for clipboard image. (Only supports **png** format)

```shell
./upgit :clipboard
```

Shortcuts for screenshot:

- On macOS, use `Ctrl+Shift+Cmd+4`
- On Linux/Ubuntu, use `Ctrl+Shift+PrintScreen`
- On Windows, use `Shift+Win+s`

### Save URL to Clipboard

Use `--output-type clipboard`:

```shell
./upgit logo.png --output-type clipboard
# or .\upgit.exe :clipboard -o clipboard
```

#### Copy as Markdown format

Add argument `--output-format markdown`:

```shell
./upgit logo.png --output-type clipboard --output-format markdown
# or .\upgit.exe :clipboard -o clipboard -f markdown
```

Then you'll get a markdown image link in your clipboard like:

```
![logo.png](!https://cdn.jsdelivr.net/gh/pluveto/upgit/logo.png)
```

### Best practice with AHK

For Windows user:

1. Install AHK

2. Create this script `upload_clipboard.ahk` and run:

   ```ahk
   ; Press Ctrl + F9 to upload clipboard image
   ^F9::
   RunWait, "upgit.exe" :clipboard --output-type clipboard --output-format markdown
   return
   ```

3. Then press <kbd>Win</kbd><kbd>Shift</kbd><kbd>S</kbd> to take screenshot. <kbd>Ctrl</kbd><kbd>F9</kbd> to upload it and get its link to your clipboard!

**Compatible with Snipaste**

(Windows Only, from v0.1.5) We recently added support for Snipaste bitmap format. Just copy screenshot and upload!
```

## Config Instructions

| Key                   | Desc                                                         |
| --------------------- | ------------------------------------------------------------ |
| username              | Your Github username, like `pluveto`                         |
| repo                  | Your Github repository name, like `upgit`                    |
| branch                | The branch for saving files, like `master` or `main`         |
| pat                   | Personal Access Token. Visit [GitHub Docs](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) for more info |
| rename                | Renaming rule. Path separator `/` will create directories if not exists. Supporting: |
| -- `{year}`           | -- Year like `2006`                                          |
| -- `{month}`          | -- Month like `01`                                           |
| -- `{day}`            | -- Day like `02`                                             |
| -- `{unix_ts}`        | -- Unix timestamp in second. Like `1643373370`.              |
| --- `{ext}`           | -- Extension like `.png`, and empty when the original file has no extension |
| -- `{file_name}`      | -- Original file base name like `demo` (without extension)   |
| -- `{file_name_hash}` | -- MD5 Hash in hex of `{file_name}`                          |

Here is a simplist sample config file:

```toml
pat = "ghp_XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX"
rename = "{year}/{month}/upgit_{year}{month}{day}_{unix_ts}{ext}"
repo = "repo-name"
username = "username"
```

### Config via Environment Variables

+ `UPGIT_TOKEN`
+ `UPGIT_RENAME`
+ `UPGIT_USERNAME`
+ `UPGIT_REPO`
+ `UPGIT_BRANCH`

### Custome output format

In follwing way:

```toml
[output_formats]
"bbcode" = "[img]{url}[/img]"
"html" = '<img src="{url}" />'
"markdown-simple" = "![]({url})"
```

Placeholder:

+ `{url}`: URL to image
+ `{fname}`: Original file basename
+ `{url_fname}`: File basename from url

Example usage:

```
# Upload clipboard and save link to clipboard as bbcode format
upgit :clipboard -o clipboard -f bbcode
```

## Todo

+ [x] Upload to specific folder
+ [x] Upload and get raw URL that is not replaced.
+ [x] Upload clipboard image
+ [x] Save uploaded image link to clipboard
+ [ ] Upload from link
+ [x] Ignore uploaded file (link input)
+ [ ] Upload history
