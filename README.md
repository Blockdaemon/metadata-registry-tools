# Oasis Metadata Registry Tools

[![CI test status][github-ci-tests-badge]][github-ci-tests-link]
[![CI lint status][github-ci-lint-badge]][github-ci-lint-link]

<!-- markdownlint-disable line-length -->
[github-ci-tests-badge]: https://github.com/oasisprotocol/oasis-core-rosetta-gateway/workflows/ci-tests/badge.svg
[github-ci-tests-link]: https://github.com/oasisprotocol/oasis-core-rosetta-gateway/actions?query=workflow:ci-tests+branch:master
[github-ci-lint-badge]: https://github.com/oasisprotocol/oasis-core-rosetta-gateway/workflows/ci-lint/badge.svg
[github-ci-lint-link]: https://github.com/oasisprotocol/oasis-core-rosetta-gateway/actions?query=workflow:ci-lint+branch:master
<!-- markdownlint-enable line-length -->

This repository contains tools for working with the [Oasis Metadata Registry].

[Oasis Metadata Registry]: https://github.com/oasisprotocol/metadata-registry

## Building

To build the `oasis-registry` tool, run:

```sh
make build
```

## Usage

_NOTE: Currently, you will need to build the `oasis-registry` tool yourself._

**NOTE: Support for signing entity metadata statements with the Ledger-based
signer is in development and not available yet.**

To sign an entity metadata statement, e.g.

```json
{
  "v": 1,
  "serial": 1,
  "name": "My entity name",
  "url": "https://my.entity/url",
  "email": "my@entity.org",
  "keybase": "my_keybase_handle",
  "twitter": "my_twitter_handle"
}
```

save it as a JSON file, e.g. `entity-metadata.json`, and run:

```sh
./oasis-registry/oasis-registry entity update \
  <SIGNER-FLAGS> \
  entity-metadata.json
```

where `<SIGNER-FLAGS>` are replaced by the appropriate signer CLI flags for your
signer (e.g. Ledger-based signer, File-based signer).

For more details, run:

```sh
./oasis-registry/oasis-registry entity update --help
```

_NOTE: The same signer flags as used by the Oasis Node CLI are supported.
See [Oasis CLI Tools' documentation on Signer Flags][oasis-cli-flags] for more
details._

The `oasis-registry entity update` command will output a preview of the entity
metadata statement you are about to sign:

```text
You are about to sign the following entity metadata descriptor:
  Version: 1
  Serial:  1
  Name:    My entity name
  URL:     https://my.entity/url
  Email:   my@entity.org
  Keybase: my_keybase_handle
  Twitter: my_twitter_handle
```

and ask you for confirmation.

It will store the signed entity metadata statement to the
`registry/entity/<HEX-ENCODED-ENTITY-PUBLIC-KEY>.json` file, where
`<HEX-ENCODED-ENTITY-PUBLIC-KEY>` corresponds to your hex-encoded entity's
public key, e.g.
`918cfe60b903e9d2c3003eaa78997f4fd95d66597f20cea8693e447b6637604c.json`.

[oasis-cli-flags]:
  https://docs.oasis.dev/general/manage-tokens/oasis-cli-tools/setup#signer-flags

### Contributing Entity Metadata Statement to Production Oasis Metadata Registry

See the [Contributing New Statements guide][contrib-guide] at the
[Oasis Metadata Registry]'s web site.

[contrib-guide]:
  https://github.com/oasisprotocol/metadata-registry#contributing-new-statements

## Development

### Examples

For some examples of using this Go library, check the [`examples/`] directory.

To build all examples, run:

```sh
make build-examples
```

To run the `lookup` example that lists the entity metadata statements in the
production Oasis Metadata Registry, run:

```sh
./examples/lookup/lookup
```

It should give an output similar to:

```text
[ms7M1v8HfItCnNNJ0tfE/PsYQsmeD+XpfGF1v0zR2Xo=]
  Name:    Everstake
  URL:     https://everstake.one
  Email:   inbox@everstake.one
  Keybase: everstake
  Twitter: everstake_pool

[gb8SHLeDc69Elk7OTfqhtVgE2sqxrBCDQI84xKR+Bjg=]
  Name:    Bi23 Labs
  URL:     https://bi23.com
  Email:   support@bi23.com
  Keybase: sunxmldapp
  Twitter: bi23com

... output trimmed ...
```

[`examples/`]: examples/

### Test Vectors

To generate the entity metadata test vectors, run:

```sh
make gen_vectors
```
