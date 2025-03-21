# Docker Proxy settings

For some reason, there are always network issues when accessing docker hub.

There are three levels that need to be set, which we explain one by one blow

## For docker pull

The network environment here is set up by dockerd, so we need to set dockerd proxy paramters, or set daemon.json

This is general set:

```
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo touch /etc/systemd/system/docker.service.d/proxy.conf
```

proxy.conf
```
[Service]
Environment="HTTP_PROXY=http://proxy.example.com:8080/"
Environment="HTTPS_PROXY=http://proxy.example.com:8080/"
Environment="NO_PROXY=localhost,127.0.0.1,.example.com"
```

or

/etc/docker/daemon.json
```
{
    "dns": [
        "223.5.5.5",
        "8.8.8.8"
    ],
    "proxies": {
        "http-proxy": "http://127.0.0.1:10086",
        "https-proxy": "http://127.0.0.1:10086",
        "no-proxy": "127.0.0.0/8,192.168.1.1/24"
    }
}
```


This is snap version:

/etc/systemd/system/snap.docker.dockerd.service
```
...
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:10086"
Environment="HTTPS_PROXY=http://127.0.0.1:10086"
Environment="NO_PROXY=localhost,127.0.0.1"
...
```
or 

/etc/environment
```
HTTP_PROXY=http://127.0.0.1:10086
HTTPS_PROXY=http://127.0.0.1:10086
NO_PROXY=localhost,127.0.0.1
```

/var/snap/docker/[version]/config/daemon.json
```
{
    "dns": [
        "223.5.5.5",
        "8.8.8.8"
    ],
    "proxies": {
        "http-proxy": "http://127.0.0.1:10086",
        "https-proxy": "http://127.0.0.1:10086",
        "no-proxy": "127.0.0.0/8,192.168.1.1/24"
    }
}

Run the following command to make the parameters take effect
```
sudo systemctl daemon-reload
// general
sudo systemctl restart docker
// snap
sudo snap restart docker
```

Run the following command to check
```
sudo systemctl show --property=Environment snap.docker.dockerd.service
```

BWT: You can also configure dockerd's proxy by configuring daemon.json

## For docker build

Since the agents of build and docker pull are isolated from each other, we need to specify the agent when we build the image, which can only be specified in the command at present

```

docker build --network host --build-arg "HTTP_PROXY=http://127.0.0.1:10086/" \
    --build-arg "HTTPS_PROXY=http://127.0.0.1:10086/" \
    --build-arg "NO_PROXY=localhost,127.0.0.1" -t agent -f ./docker/Dockerfile .

```

The host network is used here to ensure that 127.0.0.1 points to the local machine


## For docker run / container

~/.docker/config.json
```
{
   "proxies": {
       "default": {
           "httpProxy": "http://127.0.0.1:10086",
           "httpsProxy": "http://127.0.0.1:10086",
           "noProxy": "localhost,127.0.0.1"
       }
   }
}
```
