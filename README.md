### Docker DNS Round Robin

A test of Docker DNS Round Robin

The important part is the using a network alias on the command line or in the compose file and using that alias in your nginx.conf file as the `proxy_pass` or `upstream`.

#### Docker

```
$ docker build -t lb lb
Sending build context to Docker daemon 3.072 kB
Step 1 : FROM nginx:1.9
 ---> eb4a127a1188
...
Successfully built b9976ea1d078

$ docker network create backend
$ docker run -d --name lb --net backend -p 80:80 lb
$ docker run -d --net backend --net-alias apps ehazlett/docker-demo
$ docker run -d --net backend --net-alias apps ehazlett/docker-demo

$ curl -s http://$(docker-machine ip default) | grep strong
<h2 class="lsf info">served from <strong>2a445e8df911</strong></h2>

$ curl -s http://$(docker-machine ip default) | grep strong
<h2 class="lsf info">served from <strong>7043b0a71d39</strong></h2>

$ docker run -d --net backend --net-alias apps ehazlett/docker-demo
$ docker run -d --net backend --net-alias apps ehazlett/docker-demo

$ curl -s http://$(docker-machine ip default) | grep
<h2 class="lsf info">served from <strong>ca99a8442dfb</strong></h2>

$ curl -s http://$(docker-machine ip default) | grep
<h2 class="lsf info">served from <strong>28b6a528bde7</strong></h2>
```

The value between the `<strong>` tag is the hostname of the host serving the request.

#### Docker Compose

```
$ docker-compose up -d
Creating network "dockerdnsroundrobin_backend" with the default driver
Building lb
Step 1 : FROM nginx:1.9
 ---> eb4a127a1188
...
Successfully built 16b9c4a4ba32
Creating lb
Creating dockerdnsroundrobin_app_1

$ curl -s http://$(docker-machine ip default) | grep strong
<h2 class="lsf info">served from <strong>a24fd74b9383</strong></h2>

$ docker-compose scale app=3
Creating and starting dockerdnsroundrobin_app_2 ... done
Creating and starting dockerdnsroundrobin_app_3 ... done

$ curl -s http://$(docker-machine ip default) | grep strong
<h2 class="lsf info">served from <strong>0d0e21b95b1c</strong></h2>

$ curl -s http://$(docker-machine ip default) | grep strong
<h2 class="lsf info">served from <strong>be393718dd7d</strong></h2>
```

The value between the `<strong>` tag is the hostname of the host serving the request.
