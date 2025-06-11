# Alpaca PAC Finder Darwin Fix

This repository contains a fix for the PAC (Proxy Auto-Configuration) detection issue on macOS when PAC is set via JAMF or other Managed Profile / MDM solutions.

## The Issue

Alpaca may fail to detect PAC URLs on macOS systems where the PAC configuration is set by JAMF or other MDM solutions through the `scutil --proxy` mechanism. This results in Alpaca reporting:

```
No PAC URL specified or detected; all requests will be made directly
```

Even though the PAC URL is properly configured in the system.

## The Solution

This fork adds a redundant PAC URL detection method for macOS that uses `SCDynamicStoreCopyProxies` as a fallback when the original method using `SCDynamicStoreCopyValue` fails. This ensures that PAC URLs are properly detected across various macOS environments and configurations.

![PAC Detection Fix](PR%20pacfinder_darwin%20fix.png)

## Usage Instructions

### For macOS Users with PAC Detection Issues

If you're experiencing issues with Alpaca not detecting your PAC URL on macOS, especially when it's set by JAMF or another MDM solution, you have two options:

#### Option 1: Use the Pre-built Binary

1. Download the pre-built `alpaca` binary from this repository
2. Make it executable: `chmod +x alpaca`
3. Run it: `./alpaca`

#### Option 2: Build from Source

1. Clone this repository:
   ```
   git clone https://github.com/enelass/alpaca_pacfinder_darwin_fix.git
   cd alpaca_pacfinder_darwin_fix
   ```

2. Build the binary:
   ```
   go build
   ```

3. Run the resulting binary:
   ```
   ./alpaca
   ```

## Important Notes

- This fork is solely focused on fixing the PAC detection issue on macOS. It does not address similar issues on Windows or other UNIX systems.
- Once [PR #147](https://github.com/samuong/alpaca/pull/147) is approved and merged into the official repository, we recommend using the official Alpaca repository to benefit from other bug fixes and feature improvements.

## Technical Details

The fix implements two separate methods to retrieve the PAC URL:

1. Original method using `SCDynamicStoreCopyValue` with a specific key
2. New method using `SCDynamicStoreCopyProxies`

If the first method fails, the second method is tried as a fallback. This ensures that PAC URLs are properly detected regardless of how they were configured in the system.

## Original Alpaca Documentation

For more information about Alpaca and its features, please refer to the [official Alpaca repository](https://github.com/samuong/alpaca).

## License

This project is licensed under the Apache License 2.0 - see the LICENSE file for details.
