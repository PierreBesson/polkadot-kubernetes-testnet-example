# Polkadot Multi Chain Kubernetes Testnet Example

1. Setup your Kubernetes cluster, eg. locally you can use [Rancher-desktop](https://rancherdesktop.io/) or [Minikube](https://minikube.sigs.k8s.io/)

2. Install [helmfile](https://github.com/roboll/helmfile)

3. Run `helmfile sync`

4. `kubectl port-forward -n rococo validator-alice-node-0 9944` and open [polkadot.js.org/apps/?rpc=ws%3A%2F%2F127.0.0.1%3A9944](https://polkadot.js.org/apps/?rpc=ws%3A%2F%2F127.0.0.1%3A9944#/explorer)

5. `k port-forward -n rococo svc/testnet-manager 8080:80` and open [localhost:8080](http://localhost:8080/)

6. Onboard paras via gui or cmd line:

```
curl -X 'POST' 'http://localhost:8080/api/onboard_parachain/2000' -H 'accept: application/json' -d ''
curl -X 'POST' 'http://localhost:8080/api/onboard_parachain/3000' -H 'accept: application/json' -d ''
```

7. Setup hrmp channels

Install [polkadot-js api-cli](https://github.com/polkadot-js/tools/tree/master/packages/api-cli)

If you have npm:
```
npm -g i @polkadot/api-cli
```

Or with docker/podman:
```
alias polkadot-js-api="docker run --network=host jacogr/polkadot-js-tools api"
```

Test that it works with:
```
polkadot-js-api --ws ws://127.0.0.1:9944 query.system.account 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
```

```
polkadot-js-api --ws ws://127.0.0.1:9944 tx.parasSudoWrapper.sudoEstablishHrmpChannel 2000 3000 8 1024   --seed "//Alice" --sudo
polkadot-js-api --ws ws://127.0.0.1:9944 tx.parasSudoWrapper.sudoEstablishHrmpChannel 3000 2000 8 1024   --seed "//Alice" --sudo
```

8. Transfer asset from parachain 2k to parachain 3k:

```
polkadot-js-api tx.polkadotXcm.limitedReserveTransferAssets '{"v1":{"parents":1,"interior":{"x1":{"parachain":3000}}}}' '{"v1":{"parents":0,"interior":{"x1":{"AccountId32": {"id": "0x90b5ab205c6974c9ea841be688864633dc9ca8a357843eeacf2314649965fe22", "network": "Any"}}}}}' '{"v1": [ {"id": { "Concrete": {"parents":0, "interior":{"x2": [ {"PalletInstance":12}, {"GeneralIndex":0}] }}}, "Fun": { "Fungible": "320008009"}}]}' 0 Unlimited  --ws ws://127.0.0.1:9945 --seed "//Alice"
```

9. Result

See ```assets.Issued``` to ```Charlie``` on parachain 3k. 




Resources:

- [Kubernetes section of the Substrate Devops Guide](https://paritytech.github.io/devops-guide/kubernetes/index.html)
