---
date: 2022-07-13T13:50:11+08:00
author: "Rustle Karl"

title: "provisioning 自动化操作引擎"
url:  "posts/tools/docs/hashicorp/vagrant/provisioning"  # 永久链接
tags: [ "Tools", "README" ]  # 标签
categories: [ "Tools 学习笔记" ]  # 分类

toc: true  # 目录
draft: true  # 草稿
---

Provisioners 允许在 `vagrant up` 命令执行过程中自动安装软件，免去通过 `vagrant ssh` 手动安装相关软件。

## 执行条件

- 一般只在首次创建时执行（显示指定 `--no-provision` 则不执行），如果虚拟机在之前已经创建了，那么除非显式指定 `--provision`，否则不执行。
- 已经运行的虚拟机，执行 `vagrant provision` 命令。
- 执行 `vagrant reload --provision` 命令。

## 标准用法

https://www.vagrantup.com/docs/provisioning/basic_usage

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64" # 22.04 LTS, Jammy
  config.vm.hostname = "example"
  config.vm.provision "shell", inline: "echo hello"
end
```

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64" # 22.04 LTS, Jammy
  config.vm.hostname = "example"
  config.vm.provision "shell" do |s|
    s.inline = "echo hello"
  end
end
```

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64" # 22.04 LTS, Jammy
  config.vm.hostname = "example"
  config.vm.provision "bootstrap", type: "shell" do |s|
    s.inline = "echo hello"
  end
end
```

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: "echo hello",
    run: "always"
end
```

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.provision "bootstrap", type: "shell", run: "never" do |s|
    s.inline = "echo hello"
  end
end
```

## 文件

提供了从宿主机复制文件到虚拟机的能力。

```Vagrantfile
Vagrant.configure("2") do |config|
  # ... other configuration

  config.vm.provision "file", source: "~/.gitconfig", destination: ".gitconfig"
end
```

```Vagrantfile
Vagrant.configure("2") do |config|
  # ... other configuration

  config.vm.provision "file", source: "~/path/to/host/folder", destination: "$HOME/remote/newfolder"
end
```

## Shell 命令

提供执行 Shell 命令的能力。

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.provision "shell",
    inline: "echo Hello, World"
end
```

```Vagrantfile
$script = <<-SCRIPT
echo I am provisioning...
date > /etc/vagrant_provisioned_at
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: $script
end
```

```Vagrantfile
$script = <<-'SCRIPT'
echo "These are my \"quotes\"! I am provisioning my guest."
date > /etc/vagrant_provisioned_at
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: $script
end
```

允许提供宿主机文件，自动会上传到虚拟机，然后执行：

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.provision "shell", path: "script.sh"
end
```

从网络获取：

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.provision "shell", path: "https://example.com/provisioner.sh"
end
```

### 参数

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.provision "shell" do |s|
    s.inline = "echo $1"
    s.args   = "'hello, world!'"
  end
end
```

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.provision "shell" do |s|
    s.inline = "echo $1"
    s.args   = ["hello, world!"]
  end
end
```

```Vagrantfile

```

```Vagrantfile

```

```Vagrantfile

```



## 二级

### 三级



## 二级

### 三级

```Vagrantfile

```

```Vagrantfile

```


## 二级

### 三级

```Vagrantfile

```

```Vagrantfile

```


