# CI: Build Unsigned iOS IPA on GitHub

This workflow builds your iOS app **without code signing** and packages an **unsigned .ipa** as a downloadable artifact.

## How to use
1. Put this repo on GitHub (include the `project.yml` from the starter and the Sources/… files).
2. Place this file at `.github/workflows/ios-unsigned.yml`.
3. Push to `main` (or run the workflow manually under **Actions**).
4. When the job finishes, download **PS3iOS-unsigned.ipa** from the workflow **Artifacts** section.
5. Install with **AltStore** (it will re-sign it with your Apple ID).

> Notes
> - The build uses **XcodeGen** to generate the Xcode project from `project.yml`.
> - Because `CODE_SIGNING_ALLOWED=NO`, the build cannot run on device directly, but the **IPA** is suitable for AltStore/Sideloadly re-signing.
