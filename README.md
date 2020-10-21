# Dependency Analysis

This repo contains data for exploring project dependencies.

The commands used to generate the data make use of [`jq`](https://stedolan.github.io/jq/) and assume the dependency manifest file in the same directory.

## Nodejs

The JavaScript ecosystem's handling of dependencies is unique because it's the only language that needs to transfer it's dependencies to the client. This constraint has created an ecosystem of many tiny dependencies to be consumed a-la-carte style.

The node.js dependency manifest format is `package.json`.
The `nodejs.csv` was generated from the `package.json` file with the following bash command:


```bash
for pkg in $(cat package.json | jq '.dependencies * .devDependencies | keys[]' | tr -d \"); do $(curl -sL "https://registry.npmjs.org/$pkg" | jq -r --arg  name "$pkg" '.time | to_entries[] | [$name, .key, .value] | @csv' >> nodejs.csv && sleep 1 ); done

```

## PHP

As a server side language, PHP developer don't need to think about the size of their dependencies. Consequently PHP projects have fewer, larger dependencies.
The PHP dependency manifest format is `composer.json`.
The `nodejs.csv` was generated from the `composer.json` file (taken from the Magento project) with the following bash command:

```bash
for pkg in $(cat composer.json | jq '.require * .["require-dev"] | keys[] | select(test("ext|lib-\\w+$") | not) | select(test("php$")| not)' | tr -d \"); do $(curl -sL "https://packagist.org/packages/$pkg.json" | jq --arg name "$pkg" '.package.versions | to_entries[] | [$name, .key, .value.time] | @csv' >> php.csv && sleep 1 ); done
```

## The data

The data is in csv format: package name, version, date
