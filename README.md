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

# Sample output
```
$ curl -sL http://localhost:8080


 Hello backend-111111111    TCP-8081 


$ curl -sL http://localhost:8080


 Hello backend-222222222    TCP-8082 


$ 
```