# Download and update security

Official release repository: https://github.com/infinitimeilia/-infinitime

- APKs must be signed with the owner's existing production signing key. The repository and automation never need the private key or its password.
- The publishing tool verifies the APK signature structure, production package (`com.ilia.advanceclock`), non-debug build and four-part version, then computes the SHA-256 and exact byte size.
- The publishing workflow cannot independently know the owner's correct certificate. The owner must sign correctly. The installed app independently compares the downloaded APK's certificate against its own installed certificate before exporting the APK.
- The app also checks the package, increasing version code, version name, size and hash. Transfer uses HTTPS and restricted release-host redirects.
- The owner should protect the GitHub account with a passkey or two-factor authentication and keep the signing key offline or securely backed up.
- No automatic installation and no installer permission are required by this update channel. The user installs the verified APK through their file manager.
- These checks do not constitute a malware audit or guarantee absolute security. Android and the file manager govern a file after it is exported to Downloads.

Do not upload keystores, passwords, tokens, environment files, local Gradle configuration or private application data to this repository or to release assets.
