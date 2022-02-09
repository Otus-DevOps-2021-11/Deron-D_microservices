# **Лекция №15: Технология контейнеризации. Введение в Docker**
> _docker-1_
<details>
  <summary>Технология контейнеризации. Введение в Docker</summary>

## **Задание:**
Установка Docker, запуск контейнера на локальной машине, выполнение команд внутри контейнера, создание образа контейнера на основе запущенного.

Цель:
ПРИМЕЧАНИЕ: д/з будет необходимо выполнить после изучения 2-го занятия по docker - «Docker контейнеры. Docker под капотом»

В данном дз студент студент познакомится с контейнеризацией. Поймет ее отличие от виртуализации, узнает что такое Docker и зачем он нужен, создаст образ и запустит контейнер.
В данном задании тренируются навыки: работы с Docker, создания образа, запуск контейнера.

Описание/Пошаговая инструкция выполнения домашнего задания:
Все действия описаны в методическом указании.

Критерии оценки:
0 б. - задание не выполнено
1 б. - задание выполнено
2 б. - выполнены все дополнительные задания

---

## **Выполнено:**

1. Устанавливаем Docker:

 [Install Docker Engine on CentOS](https://docs.docker.com/engine/install/centos/)

~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker version
Client: Docker Engine - Community
 Version:           20.10.7
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        f0df350
 Built:             Wed Jun  2 11:56:24 2021
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.7
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       b0f5bc3
  Built:            Wed Jun  2 11:54:48 2021
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.4.9
  GitCommit:        e25210fe30a0a703442421b0f60afac609f950a3
 runc:
  Version:          1.0.1
  GitCommit:        v1.0.1-0-g4144b63
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
➜  Deron-D_microservices git:(docker-2) ✗ docker-compose version
docker-compose version 1.29.2, build 5becea4c
docker-py version: 5.0.0
CPython version: 3.7.10
OpenSSL version: OpenSSL 1.1.0l  10 Sep 2019
~~~

2. Docker hello-world

Запустим первый контейнер:
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete
Digest: sha256:507ecde44b8eb741278274653120c2bf793b174c06ff4eaa672b713b3263477b
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
~~~

3. Docker ps

Список запущенных контейнеров и список всех контейнеров:
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
➜  Deron-D_microservices git:(docker-2) ✗  docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
cf84f4fc13d5   hello-world   "/hello"   2 minutes ago   Exited (0) 2 minutes ago             awesome_hofstadter
~~~

4. Docker images

Список сохраненных образов:
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker images
REPOSITORY                TAG       IMAGE ID       CREATED        SIZE
hello-world               latest    feb5d9fea6a5   4 months ago   13.3kB
ubuntu                    18.04     39a8cfeef173   6 months ago   63.1MB
ubuntu                    bionic    39a8cfeef173   6 months ago   63.1MB
debian                    stretch   2c3ad12c6ecf   6 months ago   101MB
~~~

5. Docker run

Команда run создает и запускает контейнер из image:
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker run -it ubuntu:18.04 /bin/bash
root@610e2e3a4456:/# echo 'Hello world!' > /tmp/file
root@610e2e3a4456:/# cat /tmp/file
Hello world!
root@610e2e3a4456:/# exit
exit
➜  Deron-D_microservices git:(docker-2) ✗ docker run -it ubuntu:18.04 /bin/bash
root@8d6629686521:/# cat /tmp/file
cat: /tmp/file: No such file or directory
root@8d6629686521:/#
root@8d6629686521:/# exit
~~~

Docker run каждый раз запускает новый контейнер
Если не указывать флаг --rm при запуске docker run, то после остановки контейнер вместе с содержимым остается на диске


6. Docker ps

Найдем ранее созданный контейнер в котором мы создали /tmp/file
Это должен быть предпоследний контейнер запущенный из образа ubuntu:18.04
~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker ps -a --format "table {{.ID}}\t{{.Image}}\t{{.CreatedAt}}\t{{.Names}}"
CONTAINER ID   IMAGE          CREATED AT                      NAMES
8d6629686521   ubuntu:18.04   2022-02-06 23:26:02 +0300 MSK   objective_faraday
610e2e3a4456   ubuntu:18.04   2022-02-06 23:25:28 +0300 MSK   fervent_lewin
cf84f4fc13d5   hello-world    2022-02-06 23:13:57 +0300 MSK   awesome_hofstadter
~~~

7. Docker start & attach

- start запускает остановленный(уже созданный)
- контейнер attach подсоединяет терминал к созданному контейнеру

~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker start fervent_lewin
fervent_lewin
➜  Deron-D_microservices git:(docker-2) ✗ docker attach fervent_lewin
root@610e2e3a4456:/# cat /tmp/file
Hello world!
~~~

`Ctrl+p`, `Ctrl+q`

~~~bash
root@610e2e3a4456:/# read escape sequence
➜  Deron-D_microservices git:(docker-2) ✗ docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED         STATUS         PORTS     NAMES
610e2e3a4456   ubuntu:18.04   "/bin/bash"   7 minutes ago   Up 2 minutes             fervent_lewin
~~~

8. Docker exec

~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker ps
CONTAINER ID   IMAGE          COMMAND       CREATED      STATUS      PORTS     NAMES
610e2e3a4456   ubuntu:18.04   "/bin/bash"   2 days ago   Up 2 days             fervent_lewin
➜  Deron-D_microservices git:(docker-2) ✗ docker exec -it 610e2e3a4456 bash
root@610e2e3a4456:/# ps axf
    PID TTY      STAT   TIME COMMAND
     14 pts/1    Ss     0:00 bash
     25 pts/1    R+     0:00  \_ ps axf
      1 pts/0    Ss+    0:00 /bin/bash
root@610e2e3a4456:/# exit
exit
~~~

9. Docker commit

- Создает image из контейнера
- Контейнер при этом остается запущенным

~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker commit 610e2e3a4456 deron73/ubuntu-tmp-file
sha256:e909dc08bf43bbd66cce08c5c89c37286f56503fa9b32319ca4c909623b60697
➜  Deron-D_microservices git:(docker-2) ✗ docker images
REPOSITORY                TAG       IMAGE ID       CREATED          SIZE
deron73/ubuntu-tmp-file   latest    e909dc08bf43   14 seconds ago   63.1MB
hello-world               latest    feb5d9fea6a5   4 months ago     13.3kB
docker-compose_postgres   latest    27b9045857c9   6 months ago     265MB
docker-compose_pgsql1c    latest    27b9045857c9   6 months ago     265MB
ubuntu                    18.04     39a8cfeef173   6 months ago     63.1MB
ubuntu                    bionic    39a8cfeef173   6 months ago     63.1MB
debian                    stretch   2c3ad12c6ecf   6 months ago     101MB
➜  Deron-D_microservices git:(docker-2) ✗ docker images > dockermonolith/docker-1.log
~~~

### Задание со ⭐

~~~bash
➜  Deron-D_microservices git:(docker-2) ✗ docker inspect 610e2e3a4456
[
    {
        "Id": "610e2e3a4456ca905af8b292da1f4cb276a551c4b79b1dd441183088aa460c6f",
        "Created": "2022-02-06T20:25:28.563953735Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 1671358,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-02-06T20:30:50.19225462Z",
            "FinishedAt": "2022-02-06T20:25:59.575342297Z"
        },
        "Image": "sha256:39a8cfeef17302cb7ce93cefe12368560fe62ef9d517808855f7bda79a1eb697",
        "ResolvConfPath": "/var/lib/docker/containers/610e2e3a4456ca905af8b292da1f4cb276a551c4b79b1dd441183088aa460c6f/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/610e2e3a4456ca905af8b292da1f4cb276a551c4b79b1dd441183088aa460c6f/hostname",
        "HostsPath": "/var/lib/docker/containers/610e2e3a4456ca905af8b292da1f4cb276a551c4b79b1dd441183088aa460c6f/hosts",
        "LogPath": "/var/lib/docker/containers/610e2e3a4456ca905af8b292da1f4cb276a551c4b79b1dd441183088aa460c6f/610e2e3a4456ca905af8b292da1f4cb276a551c4b79b1dd441183088aa460c6f-json.log",
        "Name": "/fervent_lewin",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/4925f794763b8f803d6b76401e34dcdb1bdb7e5f70e6ffe7acb357750c14e8d3-init/diff:/var/lib/docker/overlay2/04e1c7aa28e7582ffe86ca772c26544f5a952c052d5be37e67e5aef63bf5a80f/diff",
                "MergedDir": "/var/lib/docker/overlay2/4925f794763b8f803d6b76401e34dcdb1bdb7e5f70e6ffe7acb357750c14e8d3/merged",
                "UpperDir": "/var/lib/docker/overlay2/4925f794763b8f803d6b76401e34dcdb1bdb7e5f70e6ffe7acb357750c14e8d3/diff",
                "WorkDir": "/var/lib/docker/overlay2/4925f794763b8f803d6b76401e34dcdb1bdb7e5f70e6ffe7acb357750c14e8d3/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "610e2e3a4456",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "ubuntu:18.04",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "3cb67ef189998d29761a502dad22b7c0d8a720f9a87ed3742ad11cc01d4b7490",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/3cb67ef18999",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "850ed7f45b94a55314b0b839485b79601c61c4fecb51a38ce2dbf10ebfa5d85d",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "6d2e447479c6b6e4e141a8281be413a46af2b28c9e182c69230e3ed653f2abcb",
                    "EndpointID": "850ed7f45b94a55314b0b839485b79601c61c4fecb51a38ce2dbf10ebfa5d85d",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
➜  Deron-D_microservices git:(docker-2) ✗ docker inspect 39a8cfeef173
[
    {
        "Id": "sha256:39a8cfeef17302cb7ce93cefe12368560fe62ef9d517808855f7bda79a1eb697",
        "RepoTags": [
            "ubuntu:18.04",
            "ubuntu:bionic"
        ],
        "RepoDigests": [
            "ubuntu@sha256:7bd7a9ca99f868bf69c4b6212f64f2af8e243f97ba13abb3e641e03a7ceb59e8"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2021-07-26T21:21:31.071665434Z",
        "Container": "c92bfb9ad23f9f790e1b9aceecd94e4da4fd21892314d88c8baf1e767d826306",
        "ContainerConfig": {
            "Hostname": "c92bfb9ad23f",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"bash\"]"
            ],
            "Image": "sha256:b8a5122daf391c9b0675a7d9b74c22896be683c3ee0935858ca8166d51756164",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "20.10.7",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "bash"
            ],
            "Image": "sha256:b8a5122daf391c9b0675a7d9b74c22896be683c3ee0935858ca8166d51756164",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 63137486,
        "VirtualSize": 63137486,
        "GraphDriver": {
            "Data": {
                "MergedDir": "/var/lib/docker/overlay2/04e1c7aa28e7582ffe86ca772c26544f5a952c052d5be37e67e5aef63bf5a80f/merged",
                "UpperDir": "/var/lib/docker/overlay2/04e1c7aa28e7582ffe86ca772c26544f5a952c052d5be37e67e5aef63bf5a80f/diff",
                "WorkDir": "/var/lib/docker/overlay2/04e1c7aa28e7582ffe86ca772c26544f5a952c052d5be37e67e5aef63bf5a80f/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:21639b09744fc39b4e1fe31c79cdf54470afe4d7239a517c4060bd181f8e3039"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
~~~
### **Выводы про разницу между контейнером и образом:**
Образ докера являются основой контейнеров.
Образ - это упорядоченная коллекция изменений корневой файловой системы и соответствующих параметров 
выполнения для использования в среде выполнения контейнера.
Образ обычно содержит объединение многоуровневых файловых систем, расположенных друг на друге.
Образ не имеет состояния и никогда не изменяется.

**Контейнер - это исполняемый (остановленный) экземпляр образа docker.**

Контейнер Docker состоит из:
- Docker образа
- Среды выполнения
- Стандартного набора инструкций
Концепция заимствована из морских контейнеров, которые определяют стандарт для доставки товаров по всему миру.
Docker определяет стандарт для отправки программного обеспечения.

(*) Взято из [Docker Glossary](https://docs.docker.com/glossary/)

Хотелось бы еще добавить, что из одного образа можно запустить множество контейнеров.



## **Полезное:**

</details>

# **Лекция №16: Docker контейнеры. Docker под капотом**
> _docker-2_
<details>
  <summary>Docker контейнеры. Docker под капотом</summary>

## **Задание:**
Запуск VM с установленным Docker Engine при помощи Docker Machine. Написание Dockerfile и сборка образа с тестовым приложением. Сохранение образа на DockerHub.

Цель:
В данном дз студент продолжит работать с Docker, создаст образы приложения и загрузит из в DockerHub.
В данном задании тренируются навыки: работы с Docker, DockerHub.

Описание/Пошаговая инструкция выполнения домашнего задания:
Все действия описаны в методическом указании.

Критерии оценки:
0 б. - задание не выполнено
1 б. - задание выполнено
2 б. - выполнены все дополнительные задания

---

## **Выполнено:**


## **Полезное:**

</details>
