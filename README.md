# Terraform-модуль `proxmox-templates`

Модуль создаёт Proxmox VE VM-шаблон на основе ISO-образа и Cloud-Init сниппета для последующего развёртывания Kubernetes-кластеров.

## Требования

- Proxmox VE 8.x
- Terraform 1.5+ или OpenTofu 1.6+
- Доступ к Proxmox API ( токен или логин/пароль )

## Установка

Клонируйте репозиторий и перейдите в него:

```bash
git clone git@github.com:<ваш-пользователь>/proxmox-templates.git
cd proxmox-templates
```

## Использование

Пример вызова модуля из корневого `main.tf` вашего проекта:

```hcl
module "template" {
  source                   = "git@github.com:<ваш-пользователь>/proxmox-templates.git//modules/template"
  node_name                = var.proxmox_node         # например, "pve"
  template_name            = "k8s-base-template"
  datastore_iso            = "local"
  datastore_vmdisk         = "local-lvm"
  image_url                = "https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img"
  file_name                = "ubuntu-jammy.qcow2"
  agent_snippet_path       = "../cloud-init/cloud-init-agent.yaml"
  agent_snippet_file_name  = "cloud-init-agent.yaml"
  cpu_cores                = 2
  memory_mib               = 2048
  disk_size                = 20
  network_bridge           = "vmbr0"
}
```

После успешного применения `terraform apply` в Proxmox появится VM-шаблон, готовый к клонированию.

## Переменные

| Имя                       | Описание                                             | Тип     | По умолчанию |
|---------------------------|------------------------------------------------------|---------|--------------|
| `node_name`               | Proxmox node, на котором создаётся шаблон            | string  | —            |
| `template_name`           | Имя создаваемого VM-шаблона                          | string  | —            |
| `datastore_iso`           | Название хранилища для ISO                           | string  | —            |
| `datastore_vmdisk`        | Название хранилища для диска шаблона                 | string  | —            |
| `image_url`               | URL исходного образа ISO/QCOW2                       | string  | —            |
| `file_name`               | Имя файла образа в Proxmox                           | string  | —            |
| `agent_snippet_path`      | Локальный путь к Cloud-Init сниппету с установкой агента | string  | —            |
| `agent_snippet_file_name` | Имя файла сниппета в Proxmox                         | string  | —            |
| `cpu_cores`               | Количество ядер CPU                                  | number  | 2            |
| `memory_mib`              | Размер RAM (MiB)                                     | number  | 2048         |
| `disk_size`               | Размер диска шаблона (GiB)                           | number  | 20           |
| `network_bridge`          | Сетевой мост (bridge)                                | string  | "vmbr0"     |

## Outputs

| Имя              | Описание                                 |
|------------------|------------------------------------------|
| `template_id`    | ID созданного VM-шаблона в Proxmox       |

## Применение

```bash
terraform init
terraform apply
```

## Лицензия

MIT

