# 64-bit YNAB 4 + modern Adobe AIR runtime installer for macOS

YNAB 4 is a 32-bit application that will not run on macOS beginning with macOS Catalina. Catalina is the first version of macOS to exclusively support 64-bit applications. YNAB made an announcement that they will not update YNAB 4 to the 64-bit architecture.

This shell script will:
- download the final release of YNAB 4 for macOS (v4.3.855)
- convert it from a 32-bit app to a 64-bit app
- upgrade the embedded Adobe AIR runtime to a newer **HARMAN Adobe AIR** runtime (currently pinned to **51.2.2.6**)

You will still need a valid license key to use the app.

This shell script is not affiliated with YNAB in any way and YNAB has not endorsed this at all. YNAB has [offically ended support](https://web.archive.org/web/20191008235951/https://www.youneedabudget.com/ynab-4-support-will-end-october-2019/) for YNAB 4. Do not contact them with support requests of any kind with respect to this shell script or using YNAB 4 on macOS Catalina. You are on your own and you use at your own risk.

This project is based on bradleymiller/Y64, but updated to produce the “final” modern-runtime result without renaming the app.

## Usage

To run the shell script, do the following:

1. Press `Command (⌘) + Space`
2. Type `Terminal` and press Enter
3. Copy and paste the following into the terminal window and press Enter. The script explains what it will do and then pauses before it does it.

   ```/bin/sh -c "$(curl -fsSL https://raw.githubusercontent.com/adamgall/ynab4-macos-modernizer/main/install)"```

4. When the shell script is complete, you must drag the resulting app to your `/Applications` directory to install.

Tip: if you already have an older patched `YNAB 4.app`, rename it first (e.g. `YNAB 4 (old).app`) so you can easily roll back.

Tip: to avoid overwriting an existing install, you can set `OUTPUT_APP_NAME` to build a differently-named app bundle:

```sh
OUTPUT_APP_NAME="YNAB 4 Remix" /bin/sh -c "$(curl -fsSL https://raw.githubusercontent.com/adamgall/ynab4-macos-modernizer/main/install)"
```

Tip: launch the app by double‑clicking it in Finder (or Launchpad). Avoid running `YNAB 4.app/Contents/MacOS/YNAB 4` directly from a terminal — some sandboxed terminal environments can interfere with LaunchServices and cause startup crashes.

## Troubleshooting

If you see either of these errors when trying to open the app:
- “This application is damaged and can’t be opened…”
- “This application requires a version of Adobe AIR which cannot be found…”

Try clearing extended attributes and re-signing (this does **not** give you a trusted developer signature; it just makes the bundle internally consistent after patching):

```sh
xattr -cr "/Applications/YNAB 4.app"
codesign --force --deep --sign - "/Applications/YNAB 4.app"
```

## How It Works

YNAB 4 is not written in a compiled language. It is written in ActionScript and interpreted by the Adobe AIR runtime when you run the program. As a result, it is the Adobe AIR runtime that needs to be updated, not the ActionScript. The final release of YNAB 4 is packaged with a 32-bit AIR runtime and 32-bit native extensions; this script patches the app to `x86_64` and then embeds a modern HARMAN Adobe AIR runtime in the resulting bundle.

On modern macOS versions, the older embedded AIR runtime can cause degraded behavior (e.g. sluggishness and broken keyboard shortcuts). This script upgrades the embedded runtime to a newer HARMAN Adobe AIR build after the 64‑bit conversion.

## Limitations
- Will not work with App Store purchased licenses unless you have received a separate license key. Refer to YNAB's [Mac App Store FAQ](https://web.archive.org/web/20170817002804/https://classic.youneedabudget.com/support/article/mac-app-store-faq).
- The `Check for Updates` function does not work.
- Apple Silicon: this build is still `x86_64` and will run under Rosetta 2 (because YNAB 4’s native extensions are not available as `arm64`).

## Other Methods

You may be able to run macOS Mojave or Windows in a virtual machine to run the 32-bit app on Catalina.

## Legal Stuff
**IMPORTANT NOTE:** This shell script is not affiliated with YNAB in any way and YNAB has not endorsed this at all. You Need a Budget and YNAB are registered trademarks of You Need A Budget LLC and/or one of its subsidiaries.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
