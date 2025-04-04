[🇪🇸 Español](#-español) | [🇬🇧 English](#-english) 

## 🇪🇸 Español

# RHCSA9 Environment (KVM/Libvirt Edition)

Este entorno automatizado está diseñado para facilitar la práctica de los temas necesarios para aprobar el examen **Red Hat Certified System Administrator (RHCSA)**, versión 9.

Incluye la configuración automática de 3 máquinas virtuales:

- `repo` (servidor de repositorios)
- `server1`
- `server2`

Todo basado en **Rocky Linux 9** y desplegado mediante **Vagrant + Ansible** sobre **KVM/libvirt** (sin VirtualBox).

---

## 🔧 Requisitos previos

### Sistema operativo compatible

- **GNU/Linux** (recomendado: Fedora, Rocky, CentOS, Ubuntu, Arch)
- **macOS** (requiere soporte para virtualización con [UTM](https://mac.getutm.app/) o [Parallels] + `vagrant-libvirt`)

### Software necesario

#### Linux:

```bash
sudo dnf install -y vagrant libvirt virt-install qemu-kvm ruby-devel gcc make libvirt-devel
vagrant plugin install vagrant-libvirt
```

#### macOS (con alternativa a KVM como UTM):

```bash
brew install vagrant
brew install --cask utm
```

> **Nota:** KVM nativo no está disponible en macOS.

### Imagen base

- [`generic/rocky9`](https://app.vagrantup.com/generic/boxes/rocky9)

---

## 🚀 Instalación del entorno

```bash
# Clona el repositorio
git clone https://github.com/TU_USUARIO/rhcsa9env.git
cd rhcsa9env

# Inicia las VMs
vagrant up --provider=libvirt
```

---

## 📁 Estructura del proyecto

```
rhcsa9env/
├── Vagrantfile
├── ansible.cfg
├── inventory
├── playbooks/
│   ├── master.yml
│   ├── repo.yml
│   ├── server1.yml
│   ├── server2.yml
│   ├── welcome.yml
│   └── reset.yml
├── rhcsa_network.xml
└── README.md
```

---

## ⚙️ Comandos útiles

### Encender el laboratorio

```bash
vagrant up --provider=libvirt
```

### Acceder a una máquina virtual

```bash
vagrant ssh server1
vagrant ssh server2
vagrant ssh repo
```

### Apagar el laboratorio

```bash
vagrant halt
```

### Reiniciar todo el entorno

```bash
vagrant reload --provision
```

### Resetear configuraciones personalizadas

```bash
cd playbooks
ansible-playbook reset.yml -i ../inventory
```

---

## 🌐 Red del laboratorio

Se crea una red virtual personalizada llamada `rhcsa_network`:

- Red: `192.168.55.0/24`
- IPs asignadas:
  - `repo`: 192.168.55.149
  - `server1`: 192.168.55.150
  - `server2`: 192.168.55.151

Definida mediante `rhcsa_network.xml`:

```bash
sudo virsh net-define rhcsa_network.xml
sudo virsh net-start rhcsa_network
sudo virsh net-autostart rhcsa_network
```

---

## 📦 Repositorio local (FTP + HTTP)

La VM `repo` sirve contenido de los repositorios BaseOS y AppStream vía:

- `http://repo.nine.example.com/BaseOS/`
- `ftp://repo.nine.example.com/AppStream/`

> Los archivos `.iso` deben copiarse y montarse **dentro de la VM repo**.

---

## 🧽 Tips adicionales

- Si modificas las IPs, actualiza los `.yml` correspondientes.
- Puedes regenerar el entorno ejecutando:

```bash
vagrant destroy -f && vagrant up
```

---

## 🧠 Objetivo del laboratorio

Simular un entorno de examen real para practicar tareas clave del **RHCSA EX200**, tales como:

- Administración de usuarios, servicios y seguridad
- Configuración de almacenamiento (LVM, particiones, montaje)
- Gestión de red, firewall y SELinux
- Uso de repositorios locales
- Uso de Podman (básico)
- Entre otros...

---

## 🖋 Autor

Ricardo L.  
Proyecto de preparación para el examen **RHCSA EX200 (RHEL 9)**

---

✨ ¡Feliz aprendizaje! ✨

## 🇬🇧 English 

# RHCSA9 Environment (KVM/Libvirt Edition)

This automated environment is designed to help you practice the topics needed to pass the **Red Hat Certified System Administrator (RHCSA)** exam, version 9.

It includes the automatic setup of 3 virtual machines:

- `repo` (repository server)
- `server1`
- `server2`

All based on **Rocky Linux 9**, deployed using **Vagrant + Ansible** over **KVM/libvirt** (no VirtualBox).

---

## 🔧 Prerequisites

### Compatible Operating System

- **GNU/Linux** (recommended: Fedora, Rocky, CentOS, Ubuntu, Arch)
- **macOS** (requires virtualization support with [UTM](https://mac.getutm.app/) or [Parallels] + `vagrant-libvirt`)

### Required Software

#### Linux:

```bash
sudo dnf install -y vagrant libvirt virt-install qemu-kvm ruby-devel gcc make libvirt-devel
vagrant plugin install vagrant-libvirt
```

#### macOS (with UTM as alternative to KVM):

```bash
brew install vagrant
brew install --cask utm
```

> **Note:** Native KVM is not available on macOS.

### Base Image

- [`generic/rocky9`](https://app.vagrantup.com/generic/boxes/rocky9)

---

## 🚀 Environment Setup

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/rhcsa9env.git
cd rhcsa9env

# Start the VMs
vagrant up --provider=libvirt
```

---

## 📁 Project Structure

```
rhcsa9env/
├── Vagrantfile
├── ansible.cfg
├── inventory
├── playbooks/
│   ├── master.yml
│   ├── repo.yml
│   ├── server1.yml
│   ├── server2.yml
│   ├── welcome.yml
│   └── reset.yml
├── rhcsa_network.xml
└── README.md
```

---

## ⚙️ Useful Commands

### Start the lab

```bash
vagrant up --provider=libvirt
```

### Access a virtual machine

```bash
vagrant ssh server1
vagrant ssh server2
vagrant ssh repo
```

### Shut down the lab

```bash
vagrant halt
```

### Restart the entire environment

```bash
vagrant reload --provision
```

### Reset custom configurations

```bash
cd playbooks
ansible-playbook reset.yml -i ../inventory
```

---

## 🌐 Lab Network

A custom virtual network named `rhcsa_network` is created:

- Network: `192.168.55.0/24`
- Assigned IPs:
  - `repo`: 192.168.55.149
  - `server1`: 192.168.55.150
  - `server2`: 192.168.55.151

Defined via `rhcsa_network.xml`:

```bash
sudo virsh net-define rhcsa_network.xml
sudo virsh net-start rhcsa_network
sudo virsh net-autostart rhcsa_network
```

---

## 📦 Local Repository (FTP + HTTP)

The `repo` VM serves BaseOS and AppStream repository content via:

- `http://repo.nine.example.com/BaseOS/`
- `ftp://repo.nine.example.com/AppStream/`

> The `.iso` files must be copied and mounted **inside the repo VM**.

---

## 🧽 Additional Tips

- If you change the IPs, update the corresponding `.yml` files.
- You can regenerate the environment by running:

```bash
vagrant destroy -f && vagrant up
```

---

## 🧠 Lab Objective

Simulate a real exam environment to practice key tasks for the **RHCSA EX200**, such as:

- User, service, and security management
- Storage configuration (LVM, partitions, mounting)
- Network, firewall, and SELinux management
- Using local repositories
- Basic Podman usage
- And more...

---

## 🖋 Author

Ricardo L.  
Exam preparation project for **RHCSA EX200 (RHEL 9)**

---

✨ Happy learning! ✨