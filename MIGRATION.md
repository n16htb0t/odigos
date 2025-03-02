# Migration

Odigos is composed of few parts:
1. The Odigos cli.
1. Odigos UI which is started by the cli.
1. The Odigos deployment in a k8s cluster.

## Check your Versions

Use Odigos cli to test for your versions of odigos:
```bash
➜  ~ odigos version
Odigos Cli Version: version.Info{Version:'v1.0.0', GitCommit:'6977d54', BuildDate:'2023-10-31T12:44:18Z'}
Odigos Version (in cluster): version.Info{Version:'v1.0.0'}
```

This cli output (at the time of writing) shows that the cli version is `v1.0.0` and the version of Odigos in the cluster is also `v1.0.0`. 

## Upgrade Odigos CLI

### Brew (MacOS only)

```sh
brew install keyval-dev/homebrew-odigos-cli/odigos
```

Will install the latest version of the Odigos cli from brew.

### GitHub Releases

Go to the [Releases Page](https://github.com/keyval-dev/odigos/releases), download the latest version for your arch and os, and install it by coping the executable to a directory in your `PATH`. You can execute `odigos version` in your shell to verify that odigos is installed correctly.

## Upgrade Odigos in the Cluster

** New from Odigos v1.0.0 **

```sh
$ odigos upgrade
```

This command will upgrade the Odigos deployment in the cluster to the version of the CLI.

## Development

### Odigos Manifests

Any change that is made in code to a manifest of odigos k8s object is automatically applied to the cluster when `odigos upgrade` is run. Any k8s object which was removed from the code will be removed from the cluster.

### Odigos CRD

Odigos CRDs are versioned. For example the `odigosconfigurations.odigos.io` resource version is `v1alpha1`.

When a change is made to a CRD, use the following guidelines:
- Removing a field from a CRD is not a breaking change and is ok without version change or migration.
- Adding a new optional field to a CRD (with a default value) is not a breaking change and is ok without version change or migration.
- Adding a new required field to a CRD is a breaking change and requires a version change and migration.
- Any change to the CRD semantics (e.g. changing the type of a field, changing the way values are used) is a breaking change and requires a new CRD version change and migration of the objects from old version to the new one.
