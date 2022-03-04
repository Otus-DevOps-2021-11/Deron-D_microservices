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

- Интеграция с GitHub
Наберем в своем канале Slack команду-сообщение:
```
/github subscribe Otus-DevOps-2021-11/Deron-D_microservices commits:*
```

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

1. Docker machine


Ставим Docker machine [https://github.com/docker/machine/releases](https://github.com/docker/machine/releases)
~~~bash
curl -L https://github.com/docker/machine/releases/download/v0.16.2/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine &&
~~~

Создадим Docker хост в Yandex Cloud и настроим локальное окружение на работу с ним
~~~bash
➜  Deron-D_infra git:(main) yc compute instance create \
  --name docker-host \
  --zone ru-central1-a \
  --network-interface subnet-name=reddit-app-net-ru-central1-a,nat-ip-version=ipv4 \
  --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=15 \
  --ssh-key ~/.ssh/appuser.pub
done (19s)
id: fhm7srocib0tiak4n3og
folder_id: b1glujc915djb9lako8f
created_at: "2022-02-14T18:30:28Z"
name: docker-host
zone_id: ru-central1-a
platform_id: standard-v2
resources:
  memory: "2147483648"
  cores: "2"
  core_fraction: "100"
status: RUNNING
boot_disk:
  mode: READ_WRITE
  device_name: fhm0r8po6uisnmtgbepe
  auto_delete: true
  disk_id: fhm0r8po6uisnmtgbepe
network_interfaces:
- index: "0"
  mac_address: d0:0d:7e:6f:0c:92
  subnet_id: e9bcqv136grugc8pqv6k
  primary_v4_address:
    address: 10.128.0.10
    one_to_one_nat:
      address: 51.250.15.142
      ip_version: IPV4
fqdn: fhm7srocib0tiak4n3og.auto.internal
scheduling_policy: {}
network_settings:
  type: STANDARD
placement_policy: {}
~~~

После чего можно инициализировать окружение Docker.
~~~bash
➜  Deron-D_infra git:(main) docker-machine create \
  --driver generic \
  --generic-ip-address=51.250.15.142 \
  --generic-ssh-user yc-user \
  --generic-ssh-key ~/.ssh/appuser \
docker-host
Running pre-create checks...
~~~

Проверяем, что наш Docker-хост успешно создан
~~~bash
➜  Deron-D_infra git:(main) docker-machine ls
NAME          ACTIVE   DRIVER    STATE     URL                        SWARM   DOCKER      ERRORS
docker-host   -        generic   Running   tcp://51.250.15.142:2376           v20.10.12
~~~

И начинаем с ним работу
~~~bash
$ eval $(docker-machine env docker-host)
~~~
> переключиться обратно на локальный хост `eval $(docker-machine env --unset)`


2. Сборка образа

~~~bash
➜  docker-monolith git:(docker-2) ✗ docker build -t reddit:latest .
Sending build context to Docker daemon  8.192kB
Step 1/11 : FROM ubuntu:18.04
 ---> dcf4d4bef137
Step 2/11 : RUN apt-get update
 ---> Using cache
 ---> cb0cee1bbe0d
Step 3/11 : RUN apt-get install -y mongodb-server ruby-full ruby-dev build-essential git
 ---> Using cache
 ---> c99964f40c4f
Step 4/11 : RUN gem install bundler
 ---> Using cache
 ---> df14a7870f89
Step 5/11 : RUN git clone -b monolith https://github.com/express42/reddit.git
 ---> Using cache
 ---> efd55a5d1952
Step 6/11 : COPY mongod.conf /etc/mongod.conf
 ---> Using cache
 ---> 3829a455e7a6
Step 7/11 : COPY db_config /reddit/db_config
 ---> Using cache
 ---> a36862cab24c
Step 8/11 : COPY start.sh /start.sh
 ---> Using cache
 ---> bec898124297
Step 9/11 : RUN cd /reddit && rm Gemfile.lock && bundle install
 ---> Using cache
 ---> 865adfa813d2
Step 10/11 : RUN chmod 0777 /start.sh
 ---> Using cache
 ---> 5a162930ae1f
Step 11/11 : CMD ["/start.sh"]
 ---> Using cache
 ---> 924d4cfbfd6e
Successfully built 924d4cfbfd6e
Successfully tagged reddit:latest
~~~

Посмотрим на все образы (в том числе промежуточные):
~~~bash
➜  docker-monolith git:(docker-2) ✗ docker images -a
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
reddit       latest    924d4cfbfd6e   24 seconds ago       647MB
<none>       <none>    5a162930ae1f   About a minute ago   647MB
<none>       <none>    865adfa813d2   About a minute ago   647MB
<none>       <none>    bec898124297   2 minutes ago        616MB
<none>       <none>    a36862cab24c   2 minutes ago        616MB
<none>       <none>    3829a455e7a6   2 minutes ago        616MB
<none>       <none>    efd55a5d1952   2 minutes ago        616MB
<none>       <none>    df14a7870f89   3 minutes ago        616MB
<none>       <none>    c99964f40c4f   3 minutes ago        614MB
<none>       <none>    cb0cee1bbe0d   6 minutes ago        102MB
ubuntu       18.04     dcf4d4bef137   11 days ago          63.2MB
~~~


3. Запуск контейнера

~~~bash
➜  docker-monolith git:(docker-2) ✗ docker run --name reddit -d --network=host reddit:latest
fc1e14519ad2e1b50d80ea866c9f6f11028dab5039093f7402292af2c6db4994
~~~

Проверим результат:
~~~bash
➜  docker-monolith git:(docker-2) ✗ docker-machine ls
NAME          ACTIVE   DRIVER    STATE     URL                        SWARM   DOCKER      ERRORS
docker-host   *        generic   Running   tcp://51.250.15.142:2376           v20.10.12

➜  docker-monolith git:(docker-2) ✗ curl 51.250.15.142:9292
<!DOCTYPE html>
<html lang='en'>
<head>
<meta charset='utf-8'>
<meta content='IE=Edge,chrome=1' http-equiv='X-UA-Compatible'>
<meta content='width=device-width, initial-scale=1.0' name='viewport'>
<title>Monolith Reddit :: All posts</title>q
....
~~~

4. Оптимизация.

Попробуем уменьшить размер образа и количество слоев. Создадим файл `Dockerfile.optimized` со следующим содержимым:
~~~Dockerfile
FROM ubuntu:18.04

RUN apt-get update && \
    apt-get install -y mongodb-server ruby-full ruby-bundler ruby-dev build-essential git && \
    git clone -b monolith https://github.com/express42/reddit.git && \
    apt-get autoremove -y &&  \
    apt-get autoclean && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm -rf /tmp/* /var/tmp/*

COPY mongod.conf /etc/mongod.conf
COPY db_config /reddit/db_config
COPY start.sh /start.sh

RUN cd /reddit && rm Gemfile.lock && bundle install
RUN chmod 0777 /start.sh

CMD ["/start.sh"]
~~~

~~~bash
docker-monolith git:(docker-2) ✗ docker build -t reddit:1.1 . -f Dockerfile.optimized
~~~

~~~bash
➜  docker-monolith git:(docker-2) ✗ docker images -a
REPOSITORY   TAG       IMAGE ID       CREATED          SIZE
reddit       1.1       ce470d456105   29 minutes ago   616MB
...
reddit       latest    675ad42ddbea   51 minutes ago   655MB
...
ubuntu       18.04     dcf4d4bef137   12 days ago      63.2MB
~~~

5. Docker hub

~~~bash
➜  docker-monolith git:(docker-2) ✗ docker login
Login with your Docker ID to push and pull images from Docker Hub. If you don't have a Docker ID, head over to https://hub.docker.com to create one.
Username: deron73
Password:
WARNING! Your password will be stored unencrypted in /home/dpp/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store
Login Succeeded
➜  docker-monolith git:(docker-2) ✗ docker tag reddit:1.1 deron73/otus-reddit:1.0
➜  docker-monolith git:(docker-2) ✗ docker push deron73/otus-reddit:1.0
The push refers to repository [docker.io/deron73/otus-reddit]
5ba9b12c850a: Pushed
0bf01b51819b: Pushed
325283f40c08: Pushed
a844f1d825e6: Pushed
36b33cb1d4ff: Pushed
8f9e73e9156d: Pushed
1dc52a6b4de8: Mounted from library/ubuntu
1.0: digest: sha256:688012a2de6158c20ee3dda784352baa403f59440aae8e394c49e1ace80b25ae size: 1782
~~~

Проверяем:

~~~bash
➜  docker-monolith git:(docker-2) ✗ eval $(docker-machine env --unset)
➜  docker-monolith git:(docker-2) ✗ docker images
REPOSITORY                TAG       IMAGE ID       CREATED        SIZE
deron73/ubuntu-tmp-file   latest    e909dc08bf43   5 days ago     63.1MB
hello-world               latest    feb5d9fea6a5   4 months ago   13.3kB
docker-compose_pgsql1c    latest    27b9045857c9   6 months ago   265MB
docker-compose_postgres   latest    27b9045857c9   6 months ago   265MB
ubuntu                    18.04     39a8cfeef173   6 months ago   63.1MB
ubuntu                    bionic    39a8cfeef173   6 months ago   63.1MB
debian                    stretch   2c3ad12c6ecf   6 months ago   101MB
➜  docker-monolith git:(docker-2) ✗ docker run --name reddit -d -p 9292:9292 deron73/otus-reddit:1.0
Unable to find image 'deron73/otus-reddit:1.0' locally
1.0: Pulling from deron73/otus-reddit
68e7bb398b9f: Pull complete
5ae5dc3e4ca1: Pull complete
5ce2afa0215b: Pull complete
a9820bbe8aa3: Pull complete
5a338a82d69c: Pull complete
7e2f2e908650: Pull complete
ee04fcf40e7a: Pull complete
Digest: sha256:688012a2de6158c20ee3dda784352baa403f59440aae8e394c49e1ace80b25ae
Status: Downloaded newer image for deron73/otus-reddit:1.0
445b15da1a649ea59c8bf217a2cf697ed2f23f50518a1038dd272c4ef751d2bd

➜  docker-monolith git:(docker-2) ✗ docker ps
CONTAINER ID   IMAGE                     COMMAND       CREATED         STATUS         PORTS                                       NAMES
445b15da1a64   deron73/otus-reddit:1.0   "/start.sh"   4 minutes ago   Up 4 minutes   0.0.0.0:9292->9292/tcp, :::9292->9292/tcp   reddit
➜  docker-monolith git:(docker-2) ✗ curl localhost:9292
<!DOCTYPE html>
<html lang='en'>
<head>
<meta charset='utf-8'>
<meta content='IE=Edge,chrome=1' http-equiv='X-UA-Compatible'>
<meta content='width=device-width, initial-scale=1.0' name='viewport'>
<title>Monolith Reddit :: All posts</title>
...
~~~

6. Еще проверяем:

~~~bash
➜  docker-monolith git:(docker-2) ✗ docker logs reddit -f
about to fork child process, waiting until server is ready for connections.
forked process: 9
child process started successfully, parent exiting
Puma starting in single mode...
* Puma version: 5.6.2 (ruby 2.5.1-p57) ("Birdie's Version")
*  Min threads: 0
*  Max threads: 5
*  Environment: development
*          PID: 32
/reddit/helpers.rb:4: warning: redefining `object_id' may cause serious problems
* Listening on http://0.0.0.0:9292
Use Ctrl-C to stop
172.17.0.1 - - [14/Feb/2022:19:47:43 +0000] "GET / HTTP/1.1" 200 1861 0.0190

➜  docker-monolith git:(docker-2) ✗ docker exec -it reddit bash
root@445b15da1a64:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0  18384  3108 ?        Ss   19:42   0:00 /bin/bash /start.sh
root           9  0.1  0.4 1020780 66740 ?       Sl   19:42   0:00 /usr/bin/mongod --fork --logpath /var/log/mongod.log --config /etc/mongodb.conf
root          32  0.0  0.2 662180 37692 ?        Sl   19:43   0:00 puma 5.6.2 (tcp://0.0.0.0:9292) [reddit]
root          45  0.2  0.0  18512  3448 pts/0    Ss   19:52   0:00 bash
root          61  0.0  0.0  34412  2904 pts/0    R+   19:52   0:00 ps aux
root@445b15da1a64:/# killall5 1
root@445b15da1a64:/# %

➜  docker-monolith git:(docker-2) ✗ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

➜  docker-monolith git:(docker-2) ✗ docker stop reddit && docker rm reddit
reddit
reddit

➜  docker-monolith git:(docker-2) ✗ docker run --name reddit --rm -it deron73/otus-reddit:1.0 bash
root@348139cf6af3:/# ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0  18512  3316 pts/0    Ss   19:54   0:00 bash
root          16  0.0  0.0  34412  2804 pts/0    R+   19:54   0:00 ps aux
root@348139cf6af3:/# exit
exit
~~~


~~~bash
➜  docker-monolith git:(docker-2) ✗ docker inspect deron73/otus-reddit:1.0
[
    {
        "Id": "sha256:ce470d456105f7339ff61104c70a0c5eccd43e33b271cd88f393e52cd24ff527",
        "RepoTags": [
            "deron73/otus-reddit:1.0"
        ],
        "RepoDigests": [
            "deron73/otus-reddit@sha256:688012a2de6158c20ee3dda784352baa403f59440aae8e394c49e1ace80b25ae"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2022-02-14T19:03:24.148992844Z",
        "Container": "ca59dcfeacd9b9d9b0894b01d36837643f732777c66dd7c26d46092b744b5119",
        "ContainerConfig": {
            "Hostname": "ca59dcfeacd9",
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
                "CMD [\"/start.sh\"]"
            ],
            "Image": "sha256:9cdb1e2dfef3577af22d96bfbbd44c4bafb1fe0cd78065faeae5131bff2fa93e",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "20.10.12",
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
                "/start.sh"
            ],
            "Image": "sha256:9cdb1e2dfef3577af22d96bfbbd44c4bafb1fe0cd78065faeae5131bff2fa93e",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 615930791,
        "VirtualSize": 615930791,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/4b2a50183a0ef47f7007028a644651f3b98a32ff7e28c7a66b06ea33ceb2b7a4/diff:/var/lib/docker/overlay2/de269dbb24d65aac8b0c6becc07ed0f3f263673fe15bfb13d0c53deb4bcd675e/diff:/var/lib/docker/overlay2/d38c83d66e09b521e78d9ff1648e87339e3243939efb185e1bf1b158a46f35f4/diff:/var/lib/docker/overlay2/4b066fe029bca4727ee7fe6d6666db9a470b80b8f552879e331ce65242fe70d7/diff:/var/lib/docker/overlay2/1fc92dcf6859745850284d5b64917e9569726f1d8cd884a7f510cb799d91965e/diff:/var/lib/docker/overlay2/e2679a7f1ab3a6420c58db16ff044cb2a1930c2047840c353b7739a15c10b28a/diff",
                "MergedDir": "/var/lib/docker/overlay2/f15dfb4fc82a549ecba2e2a404436c45585e37f28d31dd009d9876db8b2fa124/merged",
                "UpperDir": "/var/lib/docker/overlay2/f15dfb4fc82a549ecba2e2a404436c45585e37f28d31dd009d9876db8b2fa124/diff",
                "WorkDir": "/var/lib/docker/overlay2/f15dfb4fc82a549ecba2e2a404436c45585e37f28d31dd009d9876db8b2fa124/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:1dc52a6b4de8561423dd3ec5a1f7f77f5309fd8cb340f80b8bc3d87fa112003e",
                "sha256:8f9e73e9156d454cf191074f3d2cf2e5e549998554675260663bdb84bb83ba96",
                "sha256:36b33cb1d4ff2170784ed4d69175eb2615c5b18205b1bada6e021872a5124de0",
                "sha256:a844f1d825e607f83ec3f00f3df4b3b7b54d34cac8bb3973a202d6872453a767",
                "sha256:325283f40c08a4ec45f039b0436e33e60deaf2d89aeccc0291bb2a04c0e943d6",
                "sha256:0bf01b51819b4ba1a0ab3013cd7b69bdf60efdec790c9dbd36cb699c5328c847",
                "sha256:5ba9b12c850a6e7d1aed0ce3aeb982d0313035588dee9ddc2b19826ecb3495e1"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
~~~

~~~bash
➜  docker-monolith git:(docker-2) ✗ docker inspect deron73/otus-reddit:1.0 -f '{{.ContainerConfig.Cmd}}'
[/bin/sh -c #(nop)  CMD ["/start.sh"]]
➜  docker-monolith git:(docker-2) ✗ docker run --name reddit -d -p 9292:9292 deron73/otus-reddit:1.0
f028497bab3abfded0a2f41f888324411eb4bfe69e59fedd716796e219519acc
~~~


7. Освобождение ресурсов:

~~~bash
➜  docker-monolith git:(docker-2) ✗ docker-machine rm docker-host
About to remove docker-host
WARNING: This action will delete both local reference and remote instance.
Are you sure? (y/n): y
Successfully removed docker-host
~~~

~~~bash
➜  docker-monolith git:(docker-2) ✗ yc compute instances list
+----------------------+-------------+---------------+---------+--------------+-------------+
|          ID          |    NAME     |    ZONE ID    | STATUS  | EXTERNAL IP  | INTERNAL IP |
+----------------------+-------------+---------------+---------+--------------+-------------+
| fhmd1na8srgn4q5top8l | docker-host | ru-central1-a | RUNNING | 51.250.5.184 | 10.128.0.34 |
+----------------------+-------------+---------------+---------+--------------+-------------+

➜  docker-monolith git:(docker-2) ✗ yc compute instance delete docker-host
done (21s)
~~~

Задание со ⭐

1.  Создаем прототип в директории /docker-monolith/infra/ для автоматизации поднятия нескольких инстансов в Yandex Cloud, установки на них докера и запуска там образа deron73/otus-reddit:1.0

~~~bash
➜  docker-monolith git:(docker-2) ✗ tree infra
infra
├── ansible
│   ├── ansible.cfg
│   └── docker_install.yml
├── packer
│   ├── docker.json
│   ├── key.json
│   ├── variables.json
│   └── variables.json.example
└── terraform
    ├── inventory.tpl
    ├── main.tf
    ├── terraform.tfstate
    ├── terraform.tfvars
    ├── terraform.tfvars.example
    └── variables.tf
~~~

2. Жарим образ:

~~~bash
➜  docker-monolith git:(docker-2) ✗ cd infra/packer
➜  packer git:(docker-2) ✗ packer validate -var-file=./variables.json ./docker.json
The configuration is valid.

➜  packer git:(docker-2) ✗ packer build -var-file=./variables.json ./docker.json
yandex: output will be in this color.

...
--> yandex: A disk image was created: docker-base (id: fd8ekbe042d70fehmd54) with family name docker-base
~~~

3. Поднимаем инфрастурктуру, задав количество инстансов `count_of_instance` в `terraform.tfvars`:

~~~bash
➜  packer git:(docker-2) ✗ cd ../terraform
➜  terraform git:(docker-2) ✗ terraform init

Initializing the backend...

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "local" (hashicorp/local) 2.1.0...
- Downloading plugin for provider "yandex" (terraform-providers/yandex) 0.35.0...

➜  terraform git:(docker-2) ✗ terraform apply
Plan: 3 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes
...
~~~

4. Запускаем контейнеры в инстансах:
~~~bash
➜  terraform git:(docker-2) ✗ cd ../ansible
➜  ansible git:(docker-2) ✗ ansible-playbook run_otus_reddit.yml

PLAY [Pull & run docker container] *****
...
PLAY RECAP **************************************************************************************************************************************************
62.84.119.234              : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
62.84.124.98               : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
~~~

5. Проверяем:

~~~bash
➜  ansible git:(docker-2) ✗ curl 62.84.119.234 | head -7
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1861  100  1861    0     0  50297      0 --:--:-- --:--:-- --:--:-- 50297
<!DOCTYPE html>
<html lang='en'>
<head>
<meta charset='utf-8'>
<meta content='IE=Edge,chrome=1' http-equiv='X-UA-Compatible'>
<meta content='width=device-width, initial-scale=1.0' name='viewport'>
<title>Monolith Reddit :: All posts</title>
➜  ansible git:(docker-2) ✗ curl 62.84.124.98 | head -7
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1861  100  1861    0     0  56393      0 --:--:-- --:--:-- --:--:-- 56393
<!DOCTYPE html>
<html lang='en'>
<head>
<meta charset='utf-8'>
<meta content='IE=Edge,chrome=1' http-equiv='X-UA-Compatible'>
<meta content='width=device-width, initial-scale=1.0' name='viewport'>
<title>Monolith Reddit :: All posts</title>
~~~

## **Полезное:**

</details>


# **Лекция №17: Docker образы. Микросервисы**
> _docker-3_
<details>
  <summary>Docker образы. Микросервисы</summary>

## **Задание:**
Разбиение приложения на несколько микросервисов. Выбор базового образа. Подключение volume к контейнеру.

Цель:
В данном дз студент продолжит работы с Docker, разобьет приложение на отдельные микросервисы, соберет для каждого приложения отдельный образ, выберет базовый образ.
В данном задании тренируются навыки: создания образов Docker, написания Dockerfile.

Описание/Пошаговая инструкция выполнения домашнего задания:
Все действия описаны в методическом указании.

Критерии оценки:
0 б. - задание не выполнено
1 б. - задание выполнено
2 б. - выполнены все дополнительные задания


## **План**
- **Разбить наше приложение на несколько компонентов**
- **Запустить наше микросервисное приложение**

---

## **Выполнено:**

1. Поднимаем Docker хост в Yandex Cloud, аналогично предыдущему ДЗ:

~~~bash
➜  Deron-D_microservices git:(docker-3) ✗ yc compute instance create \
  --name docker-host \
  --zone ru-central1-a \
  --network-interface subnet-name=docker-net-ru-central1-a,nat-ip-version=ipv4 \
  --create-boot-disk image-folder-id=standard-images,image-family=ubuntu-1804-lts,size=15 \
  --ssh-key ~/.ssh/appuser.pub
...
      address: 51.250.5.113
...

~~~

~~~bash
docker-machine create \
  --driver generic \
  --generic-ip-address=51.250.5.113 \
  --generic-ssh-user yc-user \
  --generic-ssh-key ~/.ssh/appuser \
docker-host
~~~

~~~bash
➜  Deron-D_microservices git:(docker-3) ✗  eval $(docker-machine env docker-host)
➜  Deron-D_microservices git:(docker-3) ✗  docker-machine ls
NAME          ACTIVE   DRIVER    STATE     URL                       SWARM   DOCKER      ERRORS
docker-host   *        generic   Running   tcp://51.250.5.113:2376           v20.10.12
~~~

2. Скачиваем и распаковываем архив:

~~~bash
➜  Deron-D_microservices git:(docker-3) ✗ wget -q https://github.com/express42/reddit/archive/microservices.zip
➜  Deron-D_microservices git:(docker-3) ✗ unzip microservices.zip
Archive:  microservices.zip
...
➜  Deron-D_microservices git:(docker-3) ✗ rm microservices.zip
➜  Deron-D_microservices git:(docker-3) ✗ mv reddit-microservices src
➜  Deron-D_microservices git:(docker-3) ✗ tree src
src
├── comment
│   ├── comment_app.rb
│   ├── config.ru
│   ├── docker_build.sh
│   ├── Gemfile
│   ├── Gemfile.lock
│   ├── helpers.rb
│   └── VERSION
├── post-py
│   ├── docker_build.sh
│   ├── helpers.py
│   ├── post_app.py
│   ├── requirements.txt
│   └── VERSION
├── README.md
└── ui
    ├── config.ru
    ├── docker_build.sh
    ├── Gemfile
    ├── Gemfile.lock
    ├── helpers.rb
    ├── middleware.rb
    ├── ui_app.rb
    ├── VERSION
    └── views
        ├── create.haml
        ├── index.haml
        ├── layout.haml
        └── show.haml

~~~


3. Создаем соответствующие Docker в нашей структуре:

- [Сервис post-py](./src/post-py/Dockerfile)
- [Сервис comment](./src/comment/Dockerfile)
- [Сервис ui](./src/comment/ui)


4. Скачаем последний образ MongoDB:

Скачаем последний образ MongoDB:

~~~bash
docker pull mongo:latest
~~~

5. Соберем образы с нашими сервисами:

~~~bash
cd src
docker build -t deron73/post:1.0 ./post-py
docker build -t deron73/comment:1.0 ./comment
docker build -t deron73/ui:1.0 ./ui
~~~


6. Проверяем создание обрвзов. Создаем специальную сеть для приложения.
~~~bash
➜  src git:(docker-3) ✗ docker images
REPOSITORY        TAG            IMAGE ID       CREATED              SIZE
deron73/ui        1.0            667a7b4ca6c7   15 seconds ago       772MB
deron73/comment   1.0            02f4337e4dda   About a minute ago   770MB
deron73/post      1.0            eff55993fbb9   3 minutes ago        111MB
mongo             latest         5285cb69ea55   3 weeks ago          698MB
ruby              2.2            6c8e6f9667b2   3 years ago          715MB
python            3.6.0-alpine   cb178ebbf0f2   4 years ago          88.6MB

➜  src git:(docker-3) ✗ docker network create reddit
74ae6ce357a50def43bf424eddcce15e15ffee1b4d4efae1d817266b5d582055
➜  src git:(docker-3) ✗ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
5607fd2beb5f   bridge    bridge    local
ce687ca5c751   host      host      local
1de7e8f59dd4   none      null      local
74ae6ce357a5   reddit    bridge    local
~~~


7. Создадим bridge-сеть для контейнеров, так как сетевые алиасы не работают в сети по умолчанию.
Запустим наши контейнеры в этой сети.
Добавим сетевые алиасы контейнерам.

~~~bash
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post deron73/post:1.0
docker run -d --network=reddit --network-alias=comment deron73/comment:1.0
docker run -d --network=reddit -p 9292:9292 deron73/ui:1.0

➜  src git:(docker-3) ✗ docker ps
CONTAINER ID   IMAGE                 COMMAND                  CREATED              STATUS              PORTS                                       NAMES
51640271f386   deron73/ui:1.0        "puma"                   6 seconds ago        Up 5 seconds        0.0.0.0:9292->9292/tcp, :::9292->9292/tcp   friendly_cori
f27c93323484   deron73/comment:1.0   "puma"                   25 seconds ago       Up 24 seconds                                                   vigorous_mclaren
34ead3eb6bc4   mongo:latest          "docker-entrypoint.s…"   About a minute ago   Up About a minute   27017/tcp                                   optimistic_roentgen
~~~

Проверяем

~~~bash
➜  src git:(docker-3) ✗ curl 51.250.5.113:9292 | head -9
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1834  100  1834    0     0  63241      0 --:--:-- --:--:-- --:--:-- 63241
<!DOCTYPE html>
<html lang='en'>
<head>
<meta charset='utf-8'>
<meta content='IE=Edge,chrome=1' http-equiv='X-UA-Compatible'>
<meta content='width=device-width, initial-scale=1.0' name='viewport'>
<title>Microservices Reddit :: All posts</title>
<link crossorigin='anonymous' href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css' integrity='sha384-1q8mTJOASx8j1Au+a5WDVnPi2lkFfwwEAa8hDDdjZlpLegxhjVME1fgjWPGmkzs7' rel='stylesheet' type='text/css'>
<link crossorigin='anonymous' href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap-theme.min.css' integrity='sha384-fLW2N01lMqjakBkx3l/M9EahuwpSfeNvV63J5ezn3uZzapT0u7EYsXMjQV+0En5r' rel='stylesheet' type='text/css'>
~~~

Задание со ⭐

Остановим контейнеры:

~~~bash
docker kill $(docker ps -q)
~~~

Запустим контейнеры с другими сетевыми алиасами. Адреса для взаимодействия контейнеров зададим через ENV -
переменные внутри Dockerfile 'овю. При запуске контейнеров ( docker run ) зададим им переменные окружения соответствующие новым сетевым алиасам, не пересоздавая образ:

~~~bash
docker run -d --network=reddit --network-alias=dpp_post_db --network-alias=dpp_comment_db mongo:latest
docker run -d --network=reddit --network-alias=dpp_post --env POST_DATABASE_HOST=dpp_post_db deron73/post:1.0
docker run -d --network=reddit --network-alias=dpp_comment --env COMMENT_DATABASE_HOST=dpp_comment_db  deron73/comment:1.0
docker run -d --network=reddit -p 9292:9292 --env POST_SERVICE_HOST=dpp_post --env COMMENT_SERVICE_HOST=dpp_comment deron73/ui:1.0
~~~

Проверяем:
~~~bash
➜  Deron-D_microservices git:(docker-3) ✗ curl 51.250.5.113:9292 | head -9
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1673  100  1673    0     0  36369      0 --:--:-- --:--:-- --:--:-- 36369
<!DOCTYPE html>
<html lang='en'>
<head>
<meta charset='utf-8'>
<meta content='IE=Edge,chrome=1' http-equiv='X-UA-Compatible'>
<meta content='width=device-width, initial-scale=1.0' name='viewport'>
<title>Microservices Reddit :: All posts</title>
<link crossorigin='anonymous' href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css' integrity='sha384-1q8mTJOASx8j1Au+a5WDVnPi2lkFfwwEAa8hDDdjZlpLegxhjVME1fgjWPGmkzs7' rel='stylesheet' type='text/css'>
<link crossorigin='anonymous' href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap-theme.min.css' integrity='sha384-fLW2N01lMqjakBkx3l/M9EahuwpSfeNvV63J5ezn3uZzapT0u7EYsXMjQV+0En5r' rel='stylesheet' type='text/css'>
~~~

8. Сервис ui - улучшаем образ
~~~bash
➜  Deron-D_microservices git:(docker-3) docker images
REPOSITORY        TAG            IMAGE ID       CREATED        SIZE
deron73/post      1.0            740c208a0d53   23 hours ago   121MB
deron73/ui        1.0            667a7b4ca6c7   24 hours ago   772MB
deron73/comment   1.0            02f4337e4dda   24 hours ago   770MB
~~~

Поменяем содержимое `./ui/Dockerfile`
~~~Dockerfile
FROM ubuntu:16.04
RUN apt-get update \
    && apt-get install -y ruby-full ruby-dev build-essential \
    && gem install bundler --no-ri --no-rdoc

ENV APP_HOME /app
RUN mkdir $APP_HOME

WORKDIR $APP_HOME
ADD Gemfile* $APP_HOME/
RUN bundle install
ADD . $APP_HOME

ENV POST_SERVICE_HOST post
ENV POST_SERVICE_PORT 5000
ENV COMMENT_SERVICE_HOST comment
ENV COMMENT_SERVICE_PORT 9292

CMD ["puma"]
~~~

Пересоберем `ui`
~~~bash
➜  src git:(docker-3) ✗ docker build -t deron73/ui:2.0 ./ui
Sending build context to Docker daemon  30.72kB
Step 1/13 : FROM ubuntu:16.04
....

➜  src git:(docker-3) ✗ docker images
REPOSITORY        TAG            IMAGE ID       CREATED          SIZE
deron73/ui        2.0            ddb37a276d36   29 seconds ago   463MB
deron73/post      1.0            740c208a0d53   23 hours ago     121MB
deron73/ui        1.0            667a7b4ca6c7   24 hours ago     772MB
deron73/comment   1.0            02f4337e4dda   24 hours ago     770MB
~~~

Запустим еще раз:
~~~bash
docker kill $(docker ps -q)
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post deron73/post:1.0
docker run -d --network=reddit --network-alias=comment deron73/comment:1.0
docker run -d --network=reddit -p 9292:9292 deron73/ui:2.0

➜  Deron-D_microservices git:(docker-3) ✗ curl 51.250.5.113:9292 | head -9
~~~

Задание со ⭐

Соберем образ сервиса `UI` на основе Alpine Linux. [Dockerfile](./src//ui/Dockerfile.1)

~~~bash
docker build -t deron73/ui:3.0 ./ui --file ui/Dockerfile.1

➜  src git:(docker-3) ✗ docker images
REPOSITORY        TAG            IMAGE ID       CREATED         SIZE
deron73/ui        3.0            98406fd81821   2 minutes ago   261MB
deron73/ui        2.0            ddb37a276d36   2 days ago      463MB
deron73/post      1.0            740c208a0d53   2 days ago      121MB
deron73/ui        1.0            667a7b4ca6c7   3 days ago      772MB
deron73/comment   1.0            02f4337e4dda   3 days ago      770MB
~~~

Проверяем:
~~~bash
docker kill $(docker ps -q)
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db mongo:latest
docker run -d --network=reddit --network-alias=post deron73/post:1.0
docker run -d --network=reddit --network-alias=comment deron73/comment:1.0
docker run -d --network=reddit -p 9292:9292 deron73/ui:3.0

➜  src git:(docker-3) ✗ curl 51.250.5.113:9292 | head -9
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1673  100  1673    0     0  26555      0 --:--:-- --:--:-- --:--:-- 26140
<!DOCTYPE html>
<html lang='en'>
<head>
<meta charset='utf-8'>
<meta content='IE=Edge,chrome=1' http-equiv='X-UA-Compatible'>
<meta content='width=device-width, initial-scale=1.0' name='viewport'>
<title>Microservices Reddit :: All posts</title>
<link crossorigin='anonymous' href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css' integrity='sha384-1q8mTJOASx8j1Au+a5WDVnPi2lkFfwwEAa8hDDdjZlpLegxhjVME1fgjWPGmkzs7' rel='stylesheet' type='text/css'>
<link crossorigin='anonymous' href='https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap-theme.min.css' integrity='sha384-fLW2N01lMqjakBkx3l/M9EahuwpSfeNvV63J5ezn3uZzapT0u7EYsXMjQV+0En5r' rel='stylesheet' type='text/css'>
~~~

9. Запуск приложения с volume

Создадим Docker volume:

~~~bash
docker volume create reddit_db
~~~

Подключим его к контейнеру с MongoDB:
~~~bash
docker kill $(docker ps -q)
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db -v reddit_db:/data/db mongo:latest
~~~

Перезапуск приложения с volume
~~~bash
docker run -d --network=reddit --network-alias=post deron73/post:1.0
docker run -d --network=reddit --network-alias=comment deron73/comment:1.0
docker run -d --network=reddit -p 9292:9292 deron73/ui:3.0
~~~

- Зайдем на [http://51.250.5.113:9292/](http://51.250.5.113:9292/)
- Напишем пост
- Перезапустим контейнеры
~~~bash
docker kill $(docker ps -q)
docker run -d --network=reddit --network-alias=post_db --network-alias=comment_db -v reddit_db:/data/db mongo:latest
docker run -d --network=reddit --network-alias=post deron73/post:1.0
docker run -d --network=reddit --network-alias=comment deron73/comment:1.0
docker run -d --network=reddit -p 9292:9292 deron73/ui:3.0
~~~

- Проверим, что пост остался на месте

10. Освободим ресурсы `завтра-завтра, не сегодня...`
~~~bash
docker-machine rm docker-host
yc compute instance delete docker-host
~~~



## **Полезное:**

[Linter для работы с Docker-образами](https://github.com/hadolint/hadolint)

</details>


# **Лекция №18: Сетевое взаимодействие Docker контейнеров. Docker Compose. Тестирование образов**
> _docker-4_
<details>
  <summary>Docker Compose. Сетевое взаимодействие</summary>

## **Задание:**
Практика работы с основными типами Docker сетей. Декларативное описание Docker инфраструктуры при помощи Docker Compose.

Цель:
В данном дз студент продолжит работать с Docker. Узнает типы сетей используемые Docker. Научится создавать и управлять сетями.
В данном задании тренируются навыки: создания и управления сетями Docker.

Описание/Пошаговая инструкция выполнения домашнего задания:
Все действия описаны в методическом указании.

Критерии оценки:
0 б. - задание не выполнено
1 б. - задание выполнено
2 б. - выполнены все дополнительные задания


## **План**
- **Работа с сетями в Docker**
- **Использование docker-compose**


---

## **Выполнено:**

## **Полезное:**

</details>
