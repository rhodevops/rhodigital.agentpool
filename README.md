# Documentación

Consultar

- [https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/configuration](https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/configuration)
- [https://app.vagrantup.com/ubuntu/boxes/bionic64](https://app.vagrantup.com/ubuntu/boxes/bionic64)

# Desplegar Virtual Box vm con Vagrant

Iniciar Vagrant con el box `ubuntu/bionic64` y modificar Vagrantfile

```powershell
> vagrant init ubuntu/bionic64
> vi Vagrantfile
add Custom Configuration
```

Levantar la virtual machine (guardando logs)

```powershell
> vagrant up > vagrantup.log
```

# Comandos básicos

Conexión vía ssh

```bash
vagrant ssh
```

Apagar y volver a encender

```bash
> vagrant suspend
> vagrant up
```

Otros comandos

```md
vagrant
     destroy         stops and deletes all traces of the vagrant machine
     global-status   outputs status Vagrant environments for this user
     ssh-config      outputs OpenSSH valid configuration to connect to the machine
     status          outputs status of the vagrant machine
```

# Configurar agente AZ Devops

Crear agente y descargar archivo para `Linux - x64`

```bash
> vagrant ssh
~$ mkdir az-agents && cd az-agents
~/az-agents$ mv /vagrant/vsts-agent-linux-x64-2.214.1.tar.gz .
~/az-agents$ tar zxvf vsts-agent-linux-x64-2.214.1.tar.gz
```

Conexión y registro de agente:

```bash
~/az-agents$ ./config.sh
server URL > https://dev.azure.com/{organization}
PAT token > ******
agent pool > {agent pool name}
Connecting to server ...
```

Ejecutar agente:

```bash
Enter agent pool (press enter for default) > 
~/az-agents$ ./run.sh
```
