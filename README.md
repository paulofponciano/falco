![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)

# Lab Falco

[![Falco](https://falco.org/img/brand/falco-horizontal-color.svg)](https://falco.org)

### Deploy with Helm

```sh
helm upgrade --install falco -n falco --create-namespace falcosecurity/falco \
  --set driver.kind=ebpf \
  --set tty=true \
  --set falcosidekick.enabled=true \
  --set falcosidekick.webui.enabled=true \
  --set falcosidekick.config.loki.hostport=http://loki-loki-distributed-gateway.o11y.svc.cluster.local:80 \
  --set falcosidekick.config.loki.endpoint=/loki/api/v1/push \
  --set falcosidekick.config.loki.minimumpriority=notice \
  --set falcosidekick.config.customfields="source:falco"
```

### Falco logs

```sh
kubectl logs -l app.kubernetes.io/name=falco -n falco -c falco
```
```sh
kubectl logs -l app.kubernetes.io/name=falco -n falco -c falco | grep Notice
```

### Simulate suspicious activity

```sh
kubectl run alpine --image alpine -- sh -c "sleep infinity"
```
```sh
kubectl exec -it alpine -- sh -c "uptime"
```

