# Exposing TCP and UDP services

Ingress does not support TCP or UDP services. For this reason this Ingress controller uses the flags `--tcp-services-configmap` and `--udp-services-configmap` to point to an existing config map where the key is the external port to use and the value indicates the service to expose using the format:
`<namespace/service name>:<service port>:[PROXY]:[PROXY]`

It is also possible to use a number or the name of the port. The two last fields are optional.
Adding `PROXY` in either or both of the two last fields we can use Proxy Protocol decoding (listen) and/or encoding (proxy_pass) in a TCP service (https://www.nginx.com/resources/admin-guide/proxy-protocol/).

The next example shows how to expose the service `example-go` running in the namespace `default` in the port `8080` using the port `9000`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: tcp-configmap-example
data:
  9000: "default/example-go:8080"
```

Since 1.9.13 NGINX provides [UDP Load Balancing](https://www.nginx.com/blog/announcing-udp-load-balancing/).
The next example shows how to expose the service `kube-dns` running in the namespace `kube-system` in the port `53` using the port `53`

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: udp-configmap-example
data:
  53: "kube-system/kube-dns:53"
