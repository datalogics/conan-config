# conan-config

Contains a standard Conan 2 configuration for Datalogics, including:

- profiles for various platforms
- `settings.yml` file containing definitions of compilers and platforms
- remote repositories
- config variable overrides

This configuration can be installed with `conan config install`. 

## Using

```bash
$ conan config install git@github.com:datalogics/conan-config.git
```

## Sanitizer Profiles

Conan 2.22 introduces `compiler.sanitizer`, so this repo now ships mix-in profiles in `profiles/sanitizer-*`. Layer them after a platform profile, for example:

```bash
$ conan install . -pr apple-clang-16.4-arm -pr sanitizer-address
```

The overlay sets `compiler.sanitizer` plus the recommended `-fsanitize` compile and link flags from the [Sanitizers guide](https://docs.conan.io/2/security/sanitizers.html). Available overlays: `sanitizer-address`, `sanitizer-thread`, `sanitizer-undefined`.

## Tagging

Tags with version numbers aren't used on this repo.

This config should be **forward compatible**; only add items to the config. It's
possible to create new named profiles, and add remotes.

Projects will use the tip of the default (`develop`) branch.
