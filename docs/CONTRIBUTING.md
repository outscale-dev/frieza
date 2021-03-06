# How to test

You will need:
- golang
- Gnu Make
- docker (to run reuse tests)

Just run `make test` to run all available tests.

# How to open a bug

Please provide `frieza version` output and steps to reproduce the bug.

# How to implement a new provider

- Create package in `internal/providers/` folder. You can start by copying `provider_example`.
- Provider's package must implement:
  - Provider interface (see `internal/common/provider.go`)
  - On package level:
    - `Name string`
    - `New(config ProviderConfig, debug bool) (*YourProvider, error)`
    - `Types() []ObjectType`
    - `Cli() cli.Command`
- Add provider to `cmd/frieza/providers.go`
- Complete README.md file
- Test and Pull Request :)

Note about resource implementation:
- Try to minimize API calls by reading all resources at once when possible
- If some resource cannot be deleted (like a default resource), filter them on read
- Try to store a cache of some objects at reading-time so you can use it at deletion time. This limit the number of API calls.
- When adding new resource, remember to run `./docs/providers.sh` to update [providers.md](providers.md)

# How to release

0. Make sur that [providers.md](providers.md) is up-to-date by running `./docs/providers.sh` and commit if needed.
1. Edit `cmd/frieza/version` following [semantic versioning rules](https://semver.org/).
2. Run tests with `make test`
3. Commit changes with title `Frieza v0.0.1` (adapt version)
4. Create and push tag `v0.0.1` (adapt version)

At this point, github action should have created a new release with changelog and binaries. If not:
5. Generate binaries using `make release`
6. Create release on Github named `Frieza version v0.0.1` (adapt version)
7. Write changelog with details
8. Upload binaries in release page
