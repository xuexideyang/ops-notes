## Ansible 学习笔记

## 核心概念

| 概念 | 作用 |
|------|------|
| Inventory（清单） | 定义要管理的服务器列表 |
| Module（模块） | 具体执行任务的工具（如 `apt`、`service`、`copy`） |
| Playbook（剧本） | 按顺序执行的任务集合（YAML 格式） |
| become | 提权（sudo），普通用户登录时必须加 `become: yes` |

## 安装
apt update && apt install ansible -y
ansible --version

## mask清单格式
- hosts: localhost(webserve)
  become: yes
  tasks:
    - name: 安装 Nginx
      apt:
        name: nginx
        state: present
    - name: 启动 Nginx
      service:
        name: nginx
        state: started


## webserve格式
cat > /tmp/inventory.ini << 'EOF'
[webservers]
192.168.1.10 ansible_user=root ansible_ssh_pass=密码