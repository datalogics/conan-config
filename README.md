# conan-config

Contains a standard Conan 2 configuration for Datalogics, including:

- profiles for various platforms
- `settings.yml` file containing definitions of compilers and platforms
- remote repositories
- config variable overrides

This configuration can be installed with `conan config install`. 

## Contributing

See [AGENTS.md](AGENTS.md) for contributor guidelines covering profile layout, validation commands, and PR expectations. Share the verification checklist with your team so everyone runs the required `conan install`/`conan profile show` smoke tests before opening changes.

## Using

```bash
$ conan config install git@github.com:datalogics/conan-config.git
```

## Sanitizer Profiles

Conan 2.22 introduces `compiler.sanitizer`, so this repo now ships mix-in profiles in `profiles/sanitizer-*`. Layer them after a platform profile, for example:

```bash
$ conan install . -pr apple-clang-16.4-arm -pr sanitizer-address
```

The overlay applies the recommended `-fsanitize` compile and link flags from the [Sanitizers guide](https://docs.conan.io/2/security/sanitizers.html). Only the memory overlays (`sanitizer-memory`, `sanitizer-memory-20`) set `compiler.sanitizer`, because MSan requires a fully instrumented dependency graph and must keep its package IDs distinct. The other overlays (`sanitizer-address`, `sanitizer-hwaddress`, `sanitizer-thread`, `sanitizer-undefined`) leave `compiler.sanitizer` at its default so dependencies build cleanly.

## Tagging

Tags with version numbers aren't used on this repo.

This config should be **forward compatible**; only add items to the config. It's
possible to create new named profiles, and add remotes.

Projects will use the tip of the default (`develop`) branch.
