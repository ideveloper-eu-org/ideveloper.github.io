# Docker Proxy settings

For some reason, there are always network issues when accessing docker hub.

There are three levels that need to be set, which we explain one by one blow

## For docker pull

The network environment here is set up by dockerd, so we need to set dockerd proxy paramters

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

/etc/environment
```
HTTP_PROXY=http://127.0.0.1:10086
HTTPS_PROXY=http://127.0.0.1:10086
NO_PROXY=localhost,127.0.0.1
```

Run the following command to make the parameters take effect
```
sudo systemctl daemon-reload
// general
sudo systemctl restart docker
// snap
sudo snap restart docker
```

## For docker build

TODO:

## For docker run / container

TODO: