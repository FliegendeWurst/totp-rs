# [3.1](https://github.com/constantoine/totp-rs/releases/tag/v3.1) (03/11/2022)
### What's new
- `get_qr()` now returns a `String` as an error.
- `TOTP` now implements `core::fmt::Display`
- `Rfc6238Error` and `TotpUrlError` now implement `std::error::Error`

### CI
- Add better coverage thanks to `llvm-tools-preview` and `grcov`

### Style
- Finally `cargo fmt`'d the whole repo

### Special thanks
* [@tmpfs](https://github.com/tmpfs)  for making me notice #41.

# [3.0.1](https://github.com/constantoine/totp-rs/releases/tag/v3.0.1) (13/08/2022)
### Fixes
* `TotpUrlError` was unexported. This is now fixed. (#29)
* `base32` was reexported instead. It is now private, and will need to be an explicit dependency for the user to encore/decode base32 data.

### Changes
* `Secret` comparison is now done in constant time.

### Special thanks
* [@alexanderkja](https://github.com/alexanderkjall) for discovering #29.

# [3.0](https://github.com/constantoine/totp-rs/releases/tag/v3.0) (09/08/2022)
### New features
* Secret handling is now less error prone thanks to #25
* Totp now implements the `Default` trait, which will generate a strong secret, and have sane default values according to RFC-6238 like #26
* `Rfc6238` struct is exposed for easy Totp building
* `Totp.ttl` convenience method will tell remaining validity time of token (not taking skew into account)

### New dependency
* [gen_secret] uses `rand` to generate a secret

### Breaking
* TotpUrlError now contain a string explaining. Inspired by #23
* Totp fields `issuer` and `account_name` won't be present anymore if feature `otpauth` isn't enabled
* The secret and digits field will now be validated for SecretSize (>= 128 bits)

### Special thanks
* [@sacovo](https://github.com/sacovo)for opening #23, from which the TotpUrlError rework was inspired
* [@steven89](https://github.com/steven89) for the tremendous work and back and forth provided with #24 #25 and #26 

## Note
This has been, I think, the update containing the most work. While a lot of unit testing have been done, and test cases added, coverage seems to have dropped. Please report any issue encountered while updating totp-rs to 3.0.0

# [2.1](https://github.com/constantoine/totp-rs/releases/tag/v2.1) (16/06/2022)
### New dependency
* [otpauth] now uses `urlencoding`, which has no dependencies, to url-encode and url-decode values. Because doing this with the `url` library was kind of awkward.

### Fixes
* Bug where your issuer would be incorrectly prefixed with a /, and comparison with the issuer parameter would fail.
* Bug where the issuer and account name in path would not be correctly url decoded in path, but correctly decoded in url query.

### Special thanks
@wyhaya for discovering the first problem in #21 

# [2.0](https://github.com/constantoine/totp-rs/releases/tag/v2.0) (30/05/2022)
### What changed
- `issuer` and `account_name` are now members of TOTP, and thus are not used anymore as function parameters for methods
- `from_url()` now extracts issuer and label
- Method `get_url()` now needs `otpauth` feature
- Method `get_url()` now produces more correct urls
- Methods `next_step(time: u64)` and `next_step_current` will return the timestamp of the next step's start
- Feature `qr` enables feature `otpauth`

### Special thanks
- @wyhaya for giving ideas and feedback for this release

# [1.4](https://github.com/constantoine/totp-rs/releases/tag/v1.4) (06/05/2022)
## What's changed
* Added url dependency for `otpauth` feature, which adds a `from_url` function to parse a `TOTP` object from url. Thanks to @wyhaya (https://github.com/constantoine/totp-rs/pull/19)

# [1.3](https://github.com/constantoine/totp-rs/releases/tag/v1.3) (06/05/2022)
## What's changed
* Added helper functions `generate_current` and  `check_current`. Thanks to @wyhaya (https://github.com/constantoine/totp-rs/pull/17)
* Clarified output format of get_qr in the docs

# [1.2.1](https://github.com/constantoine/totp-rs/releases/tag/v1.2.1) (05/05/2022)
## What's changed
* Disabled default image features to only enable png

# [1.2](https://github.com/constantoine/totp-rs/releases/tag/v1.2) (05/05/2022)
## What's changed
* Bumped "image" version to 0.24
* Removed "qrcode" library, which was abandoned years ago, to "qrcodegen", which is actively maintained

# [1.1](https://github.com/constantoine/totp-rs/releases/tag/v1.1) (24/04/2022)
## What's changed
* Mitigated possible timing attack as noticed per @gleb-chipiga in https://github.com/constantoine/totp-rs/issues/13
* Added PartialEq support for TOTP<T> and PartialEq + Eq support for Algorithm, suggestion from @gleb-chipiga in https://github.com/constantoine/totp-rs/issues/14

# [1.0](https://github.com/constantoine/totp-rs/releases/tag/v1.0) (15/04/2022)
## What's Changed
* Fixed wrongful results using hmac-256 and hmac-512 thanks to @ironhaven extensive researches within RFC's in https://github.com/constantoine/totp-rs/pull/12

## What's coming next
- The currently used "qrcode" library is abandonned. Preliminary work showed it was not compatible woth newer versions of the "image" library
- I'd like to take that opportunity to rethink the way the "qr" feature is presented