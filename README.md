# envoy-tcp-proxy
envoy tcp proxy load balancer

# How to use this repo
Clone and cd into repo
```
git clone git@github.com:ion-training/envoy-tcp-proxy-hello.git && cd envoy-tcp-proxy-hello
```

Install envoy via func-e
```
curl -fsSL https://func-e.io/install.sh | sudo bash -s -- -b /usr/local/bin
```
CP envoy to /usr/local/bin
```
cp `func-e run --version | grep starting | awk '{print $2}'` /usr/local/bin
```

TCP-PROXY start
```
envoy -c tcp-proxy-LISTEN-8080.yml &
```

Backend start
```
envoy -c backend-1-TCP-8081.yml &
```
```
envoy -c backend-2-TCP-8082.yml &
```

Check envoy is running
```
ps -ef | grep envoy
```

Kill envoy
```
pkill envoy
```

# Query
```
curl -sL http://localhost:8080
```

# TCP proxy config
```
admin:
  address: {socket_address: {address: 0.0.0.0, port_value: 8000}}

static_resources:
  listeners:
  - address: {socket_address: {address: 0.0.0.0, port_value: 8080}}
    filter_chains:
    - filters:
      - name: envoy.filters.network.tcp_proxy
        typed_config:
          '@type': type.googleapis.com/envoy.extensions.filters.network.tcp_proxy.v3.TcpProxy
          stat_prefix: ingress_tcp
          cluster: targetCluster
  clusters:
  - name: targetCluster
    connect_timeout: 1s
    type: static
    lb_policy: round_robin
    load_assignment:
      cluster_name: targetCluster
      endpoints:
        - lb_endpoints:
          - endpoint:
              address: {socket_address: {address: 0.0.0.0, port_value: 8081}}
          - endpoint:
              address: {socket_address: {address: 0.0.0.0, port_value: 8082}}
```

# Sample output
```
$ curl -sL http://localhost:8080


 Hello backend-111111111    TCP-8081 


$ curl -sL http://localhost:8080


 Hello backend-222222222    TCP-8082 


$ 
```
