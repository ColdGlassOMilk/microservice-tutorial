[< Contents](../README.md#contents)

# :wrench: Environment Setup

## Platform

It it assumed that you will be developing on either a Mac, Debian based Linux distro, or on Windows with a debian (preferably Ubuntu) WSL integration enabled. (Windows users, simply run: `wsl --install` and follow the prompts)

Where applicable, commands will be presented for both platforms as such:

```shell
brew install example
# -- or --
apt install example
```

## Required Tools

### [Rancher Desktop](https://rancherdesktop.io/)

Follow the [installation instructions](https://docs.rancherdesktop.io/getting-started/installation/) for your platform.

Once installed, in `Settings -> Kubernetes`, ensure that `Enable Traefik` is unchecked, then restart.

Set rancher-desktop as the default cluster / check that everything is working:

```shell
kubectl config set-context rancher-desktop
kubectl version -o yaml
```

#### [ingress-nginx](https://github.com/kubernetes/ingress-nginx)

Once we have access to our cluster and ensured that traefik is disabled, we can install ingress-nginx, follow the [deployment instructions](https://kubernetes.github.io/ingress-nginx/deploy/):

```shell
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.6.4/deploy/static/provider/cloud/deploy.yaml
```

Deployment may take a few minutes, we can verify it's complete by waiting for the "ingress-nginx-controller" LoadBalancer service to bind to our host address (under External IP):

```shell
kubectl get svc -n ingress-nginx -w
```

### [Skaffold](https://skaffold.dev/)

Lastly, we will need the skaffold CLI tool for local development, follow the [installation instructions](https://skaffold.dev/docs/install/#standalone-binary) for your platform.

```shell
skaffold version
```

## Recommended Tools (Optional)

- [nvm]()
- [rvm]()
- [VSCode]()
- [Yarn]()
- [EditorConfig]()
- [RuboCop]()
- [ESLint]()
- [Prettier]()
