### Docker DNS Round Robin

A test of Docker DNS Round Robin

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

$ docker-compose scale app=3
Creating and starting dockerdnsroundrobin_app_2 ... done
Creating and starting dockerdnsroundrobin_app_3 ... done

$ docker ps --all
CONTAINER ID        IMAGE                    COMMAND                  CREATED             STATUS              PORTS                              NAMES
ba736c124c4a        ehazlett/docker-demo     "/bin/docker-demo -li"   3 minutes ago       Up 3 minutes        8080/tcp                           dockerdnsroundrobin_app_2
70a2256fc596        ehazlett/docker-demo     "/bin/docker-demo -li"   3 minutes ago       Up 3 minutes        8080/tcp                           dockerdnsroundrobin_app_3
38d6b54d2adb        ehazlett/docker-demo     "/bin/docker-demo -li"   3 minutes ago       Up 3 minutes        8080/tcp                           dockerdnsroundrobin_app_1
226ed826c42e        dockerdnsroundrobin_lb   "nginx -g 'daemon off"   3 minutes ago       Up 3 minutes        192.168.64.3:80->80/tcp, 443/tcp   lb

$ curl -s http://docker.local | grep strong
<h2 class="lsf info">served from <strong>38d6b54d2adb</strong></h2>

$ curl -s http://docker.local | grep strong
<h2 class="lsf info">served from <strong>38d6b54d2adb</strong></h2>

$ curl -s http://docker.local | grep strong
<h2 class="lsf info">served from <strong>38d6b54d2adb</strong></h2>
```

`¯\_(ツ)_/¯`
