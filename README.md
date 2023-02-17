# Documentación

Consultar

- [https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/configuration](https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/configuration)
- [https://app.vagrantup.com/ubuntu/boxes/bionic64](https://app.vagrantup.com/ubuntu/boxes/bionic64)
- [https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=azure-devops](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/v2-linux?view=azure-devops)

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

# Ejecutar agente

Ejecutar agente:

```bash
Enter agent pool (press enter for default) > 
~/az-agents$ ./run.sh
```

# Ejecutar agente como servicio

Crear el servicio

```bash
~/az-agents$ sudo ./svc.sh install vagrant
Creating launch agent in /etc/systemd/system/vsts.agent.rhodigital.rhodigital\x2dap.ubuntu\x2dbionic.service
Run as user: vagrant
Run as uid: 1000
gid: 1000
Created symlink /etc/systemd/system/multi-user.target.wants/vsts.agent.rhodigital.rhodigital\x2dap.ubuntu\x2dbionic.service → /etc/systemd/system/vsts.agent.rhodigital.rhodigital\x2dap.ubuntu\x2dbionic.service.
```

Levantar el servicio

```bash
~/az-agents$ sudo ./svc.sh start

/etc/systemd/system/vsts.agent.rhodigital.rhodigital\x2dap.ubuntu\x2dbionic.service
● vsts.agent.rhodigital.rhodigital\x2dap.ubuntu\x2dbionic.service - Azure Pipelines Agent (rhodigital.rhodigital-ap.ubuntu-bionic)
   Loaded: loaded (/etc/systemd/system/vsts.agent.rhodigital.rhodigital\x2dap.ubuntu\x2dbionic.service; enabled; vendor preset: enabled)
   Active: active (running) since Fri 2023-02-03 15:01:30 UTC; 15ms ago
 Main PID: 14382 (runsvc.sh)
    Tasks: 2 (limit: 2361)
   CGroup: /system.slice/vsts.agent.rhodigital.rhodigital\x2dap.ubuntu\x2dbionic.service
           └─14382 /bin/bash /home/vagrant/az-agents/runsvc.sh

Feb 03 15:01:30 ubuntu-bionic systemd[1]: Started Azure Pipelines Agent (rhodigital.rhodigital-ap.ubuntu-bionic).
```

Otros comandos

- Status `sudo ./svc.sh status`
- Stop `sudo ./svc.sh stop`
- Uninstall `sudo ./svc.sh uninstall` (antes hay que pararlo)

