[< Contents](../README.md#contents)

# :wood: Project Setup

## Initialization

1.  Create our project root, we'll call our app `chirper`, then initialize git

    ```shell
    mkdir chirper
    cd chirper
    git init
    touch .gitignore
    ```

2.  Create the base directory structure for our kubernetes (k8s) configurations

    ```shell
    mkdir -p infra/k8s/{base/{deployments,ingress,jobs,namespaces,services},overlays/{development,production}}
    ```

    The folder structure looks like this:

         .
         ├── infra
         │   └── k8s
         │       ├── base
         │       │   ├── deployments
         │       │   ├── ingress
         │       │   ├── jobs
         │       │   ├── namespaces
         │       │   └── services
         │       └── overlays
         │           ├── development
         │           └── production

3.  Create a namespace for our project

    ```shell
    k create ns chirper --dry-run=client -o yaml > infra/k8s/base/namespaces/chirper.yml
    ```

    `infra/k8s/base/namespaces/chirper.yml`:

    ```yaml
    apiVersion: v1
    kind: Namespace
    metadata:
      creationTimestamp: null
      name: chirper
    spec: {}
    status: {}
    ```

4.  Create a kustomize base configuration file at `infra/k8s/base/kustomization.yml`:

    ```yml
    resources:
      - namespaces/chirper.yml
    ```

5.  Create another kustomize config at `infra/k8s/overlays/development/kustomization.yml`:

    ```yml
    bases:
      - ../../base
    ```

6.  Initialize skaffold:

    ```shell
    skaffold init --skip-build --default-kustomization=infra/k8s/overlays/development/kustomization.yml
    ```

    Edit the generated `skaffold.yml` file to support kustomize configs:

    Update `rawYaml` to `kustomize / paths`

    ```yaml
    apiVersion: skaffold/v4beta3
    kind: Config
    metadata:
      name: chirper
    manifests:
      kustomize:
        paths:
          - infra/k8s/overlays/development
    ```

## Optional Setup

Initialize EditorConfig

```shell
ec init
```

Output of `.editorconfig`:

```ini
root = true

[*]
charset = utf-8
indent_style = space
indent_size = 2
end_of_line = lf
trim_trailing_whitespace = true
insert_final_newline = true
```
