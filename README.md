# Dependency Analysis

This repo contains data for exploring project dependencies.

The data was generated with the following bash command:

```bash
for pkg in $(cat package.json | jq '.dependencies | keys[]' | tr -d \"); do $(curl -sL "https://registry.npmjs.org/$pkg" | jq '{ versions: .time, package: .name }' >> pkgs.json && sleep 1 ); done
```

This command uses [`jq`](https://stedolan.github.io/jq/) and assumes it's being run in a directory with a node.js dependency manifest file in it; aka `package.json`.
