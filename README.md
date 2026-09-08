# @aihu/plugin

The build-time plugin contract for aihu applications and opt-in extensions.
It provides the typed contribution and hook surface used by the aihu compiler,
server configuration, and plugin authors. It has no runtime dependencies and
must be explicitly registered by an application; plugins are never discovered
implicitly.

## Install

```bash
npm install @aihu/plugin
# or
bun add @aihu/plugin
```

## Define a plugin

```ts
import { definePlugin } from '@aihu/plugin'

export default definePlugin({
  name: 'forms',
  version: '0.1.0',
  namespace: 'forms',
  contributes: {
    blocks: ['fields'],
    macros: [
      {
        name: '$field',
        validIn: ['@forms.fields'],
        lowering: () => 'createField()',
      },
    ],
  },
})
```

`definePlugin` adds the package brand. The compiler or framework integration
must call `validatePlugin` during registration so required fields, reserved
namespaces, duplicate namespaces, and the declared `aihuVersion` range are
checked at the build boundary.

## Contract surface

The public entry point exports build and SFC contexts, block parsers, macros,
transforms, server-only runtime and middleware contributions, lifecycle hooks,
`definePlugin`, `validatePlugin`, and `AIHU_VERSION`.

The ratified contract references are retained in [`docs/superpowers/specs`](docs/superpowers/specs):

- [Plugin Contract](docs/superpowers/specs/2026-05-02-spec-plugin-contract.md)
- [Live Binding](docs/superpowers/specs/2026-05-05-spec-live-binding.md)
- [Live Binding implementation notes](docs/superpowers/specs/live-binding-impl.md)

## Development

This repository uses Bun 1.3.14 and Node 22.14.0 in CI.

```bash
bun install --frozen-lockfile
bun run check:ci
```

The release workflow only publishes an exact `v<package-version>` tag, requires
the repository `NPM_TOKEN` secret, verifies npm authentication, and requests
npm provenance through GitHub's OIDC token.

## License

MIT — see [LICENSE](LICENSE).
