# Polkadot Multi Chain Kubernetes Testnet Example

1. Setup your Kubernetes cluster, eg. locally you can use [Rancher-desktop](https://rancherdesktop.io/) or [Minikube](https://minikube.sigs.k8s.io/)
2. Install [helmfile](https://github.com/roboll/helmfile)
3. Run `helmfile sync`
4. `kubectl port-forward -n rococo validator-alice-node-0 9944` and open [polkadot.js.org/apps/?rpc=ws%3A%2F%2F127.0.0.1%3A9944](https://polkadot.js.org/apps/?rpc=ws%3A%2F%2F127.0.0.1%3A9944#/explorer)
5. `k port-forward -n rococo svc/testnet-manager 8080:80` and open [localhost:8080](http://localhost:8080/)

