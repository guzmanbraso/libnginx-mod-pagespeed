
Dockerfiles for building libnginx-mod-pagespeed for Debian / Ubuntu

[![Build Status](https://travis-ci.org/darylounet/libnginx-mod-pagespeed.svg?branch=master)](https://travis-ci.org/darylounet/libnginx-mod-pagespeed)

If you're just interested in installing built packages, go there :
https://packagecloud.io/DaryL/libnginx-mod-pagespeed

Instructions : https://packagecloud.io/DaryL/libnginx-mod-pagespeed/install#manual-deb


If you want to build packages by yourself, this is for you :

DCH Dockerfile usage (always use stretch as it is replaced before build) :

```bash
docker build -t deb-dch -f Dockerfile-deb-dch .
docker run -it -v $PWD:/local deb-dch bash -c '\
dch -M -v 1.12.34.3+nginx-1.12.2-1~stretch --distribution "stretch" "Updated upstream."'
```

Build Dockerfile usage :

```bash
docker build -t build-nginx-pagespeed -f Dockerfile-deb \
--build-arg DISTRIB=debian --build-arg RELEASE=stretch \
--build-arg NGINX_VERSION=1.12.2 --build-arg NPS_VERSION=1.12.34.3 .
```

Or for Ubuntu :
```bash
docker build -t build-nginx-pagespeed -f Dockerfile-deb \
--build-arg DISTRIB=ubuntu --build-arg RELEASE=xenial \
--build-arg NGINX_VERSION=1.12.2 --build-arg NPS_VERSION=1.12.34.3 .
```

Then :
```bash
docker run build-nginx-pagespeed
docker cp $(docker ps -l -q):/src ~/Downloads/
```

And once you don't need it anymore :
```bash
docker rm $(docker ps -l -q)
```

Get latest ngx_pagespeed version : https://github.com/pagespeed/ngx_pagespeed/releases
Or :
```bash
curl -s https://api.github.com/repos/pagespeed/ngx_pagespeed/tags |grep "name" |grep "stable" |head -1 |sed -n "s/^.*v\(.*\)-stable.*$/\1/p"
```

Get latest nginx version : https://nginx.org/en/download.html
Or :
```bash
curl -s https://nginx.org/packages/ubuntu/dists/xenial/nginx/binary-amd64/Packages.gz |zcat |php -r 'preg_match_all("#Package: nginx\nVersion: (.*?)-\d~.*?\nArch#", file_get_contents("php://stdin"), $m);echo implode($m[1], "\n")."\n";' |sort -r |head -1
```