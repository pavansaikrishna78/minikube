## istio Addon (istioctl-based)
[Istio](https://istio.io/docs/setup/getting-started/) - Connect, secure, control, and observe services. This addon now uses istioctl for installation instead of the Istio operator.

### Enable istio on minikube
Make sure to start minikube with at least 8192 MB of memory and 4 CPUs.
See official [Platform Setup](https://istio.io/docs/setup/platform-setup/) documentation.

```shell script
minikube start --memory=8192mb --cpus=4
```

To enable this addon, simply run:
```shell script
minikube addons enable istio
```

**Note**: The new istioctl-based installation no longer requires the `istio-provisioner` addon. The installation is now handled by a single Kubernetes Job that downloads and uses istioctl directly.

In a minute or so istio default components will be installed into your cluster. You could run `kubectl get po -n istio-system` to see the progress for istio installation.

### What's installed

The addon installs:
- Istio control plane components (istiod) in the `istio-system` namespace
- Istio gateway components
- Automatic sidecar injection for the `default` namespace

### Testing installation

```shell script
# Check Istio pods
kubectl get po -n istio-system

# Check istioctl version and verify installation (if you have istioctl installed locally)
istioctl version
istioctl verify-install
```

### Features

- **istioctl-based**: Uses official istioctl binary for installation
- **Resource-optimized**: Reduced memory and CPU requests for minikube
- **Default profile**: Uses Istio's default configuration suitable for development
- **Automatic injection**: Default namespace is labeled for sidecar injection

### Troubleshooting

If the installation fails, you can check the installer job logs:
```shell script
kubectl logs job/istio-installer -n istio-system
```

### Disable istio
To disable this addon, simply run:
```shell script
minikube addons disable istio
```

This will remove all Istio components including CRDs and configurations.