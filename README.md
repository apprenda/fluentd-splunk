# Fluentd-Splunk on Kubernetes

Send [kubernetes](https://github.com/kubernetes/kubernetes) logs to [splunk](https://github.com/splunk) using [fluentd](https://github.com/fluent/fluentd)

## Before start

**Download the project**

```sh
curl -L https://github.com/dkoshkin/fluentd-splunk/archive/v0.1.tar.gz | tar -xz
```

**Create `config-map` for `fluentd.conf`**

Modify `fluentd.conf` with custom `<source>` definitions if needed.

```sh
kubectl create configmap fluentd-config --from-file=fluentd.conf --namespace=kube-system
```

**Create `secret` with the Splunk HEC configuration for `fluentd.conf`**

```sh
kubectl --namespace kube-system create secret generic splunk-config \
 --from-literal=hec-token=$HEC_TOKEN \
 --from-literal=hec-address=$HEC_ADDRESS \
 --from-literal=hec-protocol=$HEC_PROTOCOL \
 --from-literal=hec-verify-tls=$HEC_VERIFY_TLS \
 --from-literal=hec-index=$HEC_INDEX
```

* `HEC_TOKEN`: a unique token generated by Splunk
* `HEC_ADDRESS`: **Do not** include the protocol. ie `10.0.0.5:8088`
* `HEC_PROTOCOL`: `https` or `http`
* `HEC_VERIFY_TLS`: set to `false` if using self-signed cert
* `HEC_INDEX`: index configured in HEC

## Deploy Daemonset

```sh
kubectl apply -f kubernetes/fluentd-splunk.yaml
```