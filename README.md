# Docker-1.12.6 Deploy

Docker 容器引擎从 1.13 版本开始内置了 swarm 容器调度引擎，因此最新版本的 Kubernetes 目前只支持到Docker 1.12版本。

## 配置 ansible.cfg

cat ansibe.cfg

```
[defaults]
inventory = hosts
remote_user = vagrant
private_key_file = ~/.vagrant.d/insecure_private_key
host_key_checking = False
```

## 配置 Inventory

cat hosts

```
[k8s]
master ansible_host=127.0.0.1 ansible_port=2222
node1 ansible_host=127.0.0.1 ansible_port=2200
node2 ansible_host=127.0.0.1 ansible_port=2201
```

## 配置启动文件

site.yaml

```yaml
---
- name: Install docker
  hosts:
    - k8s
  become: yes
  roles:
    - docker

```

## 开始部署

```
➜  ansiblePlay tree -L 3
.
├── README.md
├── defaults
│   └── main.yml
├── files
│   ├── docker
│   ├── docker-containerd
│   ├── docker-containerd-ctr
│   ├── docker-containerd-shim
│   ├── docker-proxy
│   ├── docker-runc
│   └── dockerd
├── handlers
│   └── main.yaml
├── meta
│   └── main.yaml
├── tasks
│   └── main.yaml
├── templates
│   └── docker.j2
├── tests
│   ├── inventory
│   ├── roles
│   │   └── docker -> ../../
│   ├── test.retry
│   └── test.yml
└── vars
    └── main.yml
```

```
ansibe_playbook ./tests/test.yml
```

## License

MIT
