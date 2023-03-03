```console
updatecli diff
```

```console
updatecli apply
```

### Aqua and Pinned Versions

##### aqua.yaml

```yaml
registries:
- type: standard
  ref: v3.139.0 # renovate: depName=aquaproj/aqua-registry

packages:
- name: helmfile/helmfile
  version: v0.149.0 # pinned to this version
```

##### updatecli.d/helmfile.yaml

```yaml
targets:
  updateVersion:
    sourceid: latestVersion
    scmid: default
    name: Update version in aqua.yaml
    kind: file
    spec:
      file: aqua.yaml
      matchpattern: '(- name: helmfile/helmfile@)v\d+.\d+.\d+'
      replacepattern: >-
        ${1}{{ source "latestVersion" }}
```

##### Error

The implementation of `matchpattern` of kind `file` in step `targets` fails if no matching entry is to
be found in the target file.

```console
ERROR: something went wrong in target "updateVersion" : "No line matched in the file \"/tmp/xxx/yyy/CrystalMethod/aqua-meets-updatecli/aqua.yaml\" for the pattern \"(- name: helmfile/helmfile@)v\\\\d+.\\\\d+.\\\\d+\""

Pipeline "Bump helmfile/helmfile version" failed
Skipping due to:
        targets stage:  "something went wrong during target execution"
```
