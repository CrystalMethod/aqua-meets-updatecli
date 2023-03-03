```console
updatecli diff
```

```console
updatecli apply
```

### Version Updates

### You can't touch this - Pinned Versions

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
conditions:
  pinnedVersion:
    name: Verify version is not pinned
    kind: file
    disablesourceinput: true
    spec:
      file: aqua.yaml
      matchpattern: '(- name: helmfile/helmfile@)v\d+.\d+.\d+'

targets:
  updateVersion:
    sourceid: latestVersion
    scmid: default
    name: Update version in aqua.yaml
    kind: file
    spec:
      file: aqua.yaml
      matchpattern: '(- name: helmfile/helmfile@)v\d+.\d+.\d+'
      replacepattern: '${1}{{ source "latestVersion" }}'
```

##### updatecli --config ./updatecli.d

```
...
- Bump helmfile/helmfile version:
        Source:
                ✔ [latestVersion] Get latest version from GitHub releases (kind: githubrelease)
        Condition:
                ✗ [pinnedVersion] Verify version is not pinned (kind: file)
                ✔ [verifyVersion] Verify latest release published on GitHub (kind: githubrelease)
        Target:
                - [updateVersion] Update version in aqua.yaml (kind: file)
```
