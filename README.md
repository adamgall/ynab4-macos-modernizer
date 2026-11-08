# Universal YNAB 4 for macOS

Build YNAB 4.3.855 with Adobe AIR **51.3.1.2** and native extensions for both **Intel (`x86_64`) and Apple Silicon (`arm64`)**. This replaces the previous Intel-only conversion; the rebuilt launcher, AIR runtime, and extensions do not require Rosetta.

You still need a valid YNAB 4 license. This project is unofficial, unsupported by YNAB, and provided at your own risk. **Back up your budgets and keep your existing app before trying this build.** Removing the Rosetta dependency is not a guarantee of compatibility with future macOS releases.

## Usage

Install the Xcode Command Line Tools if needed (`xcode-select --install`). The build also requires `python3`; `clang`, `curl`, `shasum`, and the macOS bundle tools must be available.

Download or clone the **entire repository**, then run from its directory:

```sh
git clone https://github.com/adamgall/ynab4-macos-modernizer.git
cd ynab4-macos-modernizer
/bin/bash ./install
```

If you already have a checkout, run `/bin/bash ./install` from that checkout after updating it. **The old curl-to-shell command is no longer supported:** the installer now compiles the companion files in `src/air51/`.

The installer explains its actions and waits for Enter. It creates a new app in a temporary output directory, prints that directory, and optionally reveals it in Finder. It does not replace your installed app automatically. Drag the resulting app into `/Applications` when ready; keep the old version for rollback.

For a side-by-side build:

```sh
OUTPUT_APP_NAME="YNAB 4 Universal" /bin/bash ./install
```

Launch through Finder, not by directly executing `Contents/MacOS/YNAB 4`. Differently named builds still share YNAB's application identity and settings; they are not isolated budget environments. Test with a disposable budget copy before using your working budget, and do not run both versions against the same budget concurrently.

## Downloads and verification

The installer reuses these files from the repository root only when their SHA-256 checksums match; otherwise it downloads them:

- `YNAB4_LiveCaptive_4.3.855.dmg` from the Internet Archive.
- `AdobeAIR.dmg`, pinned to AIR **51.3.1.2**, from [airsdk.wxlift.com](https://airsdk.wxlift.com/assets/downloads/51.3.1.2/AdobeAIR.dmg).

The former `airsdk.harman.com` download URL now returns an HTML migration page. That page is not an AIR installer and fails checksum verification. The new URL serves the expected DMG; checksums remain enforced.

Do not bypass a checksum mismatch or substitute a newer AIR version. The runtime patches use offsets specific to the verified AIR release. Updating AIR requires reviewing those patches and testing both architectures.

## How it works

YNAB's ActionScript application is retained. The installer:

1. Verifies and extracts the original YNAB and AIR disk images.
2. Replaces the embedded AIR framework and launcher with AIR 51.3.1.2 universal binaries.
3. Patches the launcher into captive-runtime mode and disables AIR's obsolete privileged-helper installation path.
4. Builds the compatibility `A2712Enabler` executable expected by AIR. It exits successfully without installing a privileged helper.
5. Compiles replacement MacUtils and Sparkle native extensions from `src/air51/` for Intel and Apple Silicon.
6. Ad-hoc signs the app, verifies its signature, and checks both architectures in the launcher, runtime, helper, and replacement extensions. Verification failures stop the build.

Mounted input images are tracked in the main shell and detached during cleanup, including failed builds. Temporary build files are removed; the successful output app is retained separately.

## Limitations and testing

- MacUtils is a compatibility reimplementation, not the original extension source. It supplies file dialogs, directory listing, write-access checks, receipt presence checks, and relaunch support.
- The legacy fullscreen-enabling hook does nothing. Fullscreen behavior is not guaranteed.
- Sparkle update checks are intentionally disabled; YNAB 4's old update service is not restored.
- Mac App Store purchases require a separate license key; receipt presence checking does not restore App Store licensing.
- Ad-hoc signing is not Apple notarization.
- Before switching, test opening a copied budget, saving and reopening it, importing transactions, file/folder selection, and any sync workflow you rely on. Launch success alone does not establish data integrity or sync compatibility.

## Provenance

The original 32-bit-to-64-bit converter was based on [bradleymiller/Y64](https://gitlab.com/bradleymiller/Y64).

The universal conversion, AIR patches, and source files in `src/air51/` are adapted from [newkind/ynab4-universal-binary-converter](https://github.com/newkind/ynab4-universal-binary-converter), revision `b8736448b03a24affb51578d3a66a0a707991d8a`. That project discloses that its AIR 51 / Apple Silicon rewrite was AI-generated. This project updates its download host, preserves custom output naming, fixes mount tracking, and requires successful signing and architecture verification.

## Legal Stuff

This shell script is not affiliated with YNAB in any way and YNAB has not endorsed it. YNAB has [officially ended support](https://web.archive.org/web/20191008235951/https://www.youneedabudget.com/ynab-4-support-will-end-october-2019/) for YNAB 4. Do not contact them for support with this converter. You Need a Budget and YNAB are registered trademarks of You Need A Budget LLC and/or one of its subsidiaries.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
