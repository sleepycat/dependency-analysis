# Dependency Analysis

This repo contains data for exploring project dependencies.

The data was generated with the following bash command:

```bash
for pkg in $(cat package.json | jq '.dependencies * .devDependencies | keys[]' | tr -d \"); do $(curl -sL "https://registry.npmjs.org/$pkg" | jq -r --arg  name "$pkg" '.time | to_entries[] | [$name, .key, .value] | @csv' >> pkgs.csv && sleep 1 ); done
```

This command uses [`jq`](https://stedolan.github.io/jq/) and assumes it's being run in a directory with a node.js dependency manifest file in it; aka `package.json`.

The data is in csv format: package name, version, date

This repo contains the package.json used to create the data file, but the above command will work on any package.json.
