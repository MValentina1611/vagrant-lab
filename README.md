# :clipboard: Informe de Práctica: Configuración de Apache en Vagrant

## Introducción

En esta práctica se utilizó Vagrant para crear y configurar una máquina virtual que ejecuta Ubuntu 18.04 (Bionic Beaver) y sirve un servidor web Apache. El objetivo fue automatizar el despliegue de un servidor Apache en una máquina virtual y verificar su accesibilidad desde la máquina host.

## :open_file_folder: Vagrantfile

El siguiente `Vagrantfile` fue utilizado para configurar la máquina virtual:

```ruby
Vagrant.configure("2") do |config|
  # Usar una box de Ubuntu 18.04
  config.vm.box = "ubuntu/bionic64"

  # Configurar la red para acceder al servidor web
  config.vm.network "private_network", type: "dhcp"

  # Provisión para instalar Apache
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y apache2
  SHELL

  # Configurar la carpeta sincronizada
  config.vm.synced_folder ".", "/var/www/html"

  # Configurar la máquina virtual
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end
end

```

## :hammer_and_wrench: Explicación del Vagrantfile

+ **Box:** Se utiliza la box ubuntu/bionic64 para crear una máquina virtual con Ubuntu 18.04.
+ **Red Privada:** La máquina virtual se configura en una red privada utilizando DHCP para asignar una IP dinámica.
+ **Provisión:** Se incluye un script de shell para instalar Apache automáticamente al iniciar la máquina virtual.
+ **Carpeta Sincronizada:** Se sincroniza el directorio actual del host con el directorio /var/www/html de la máquina virtual, donde se almacenan los archivos servidos por Apache.
+ **Configuración del Proveedor:** Se asigna 1 GB de memoria RAM a la máquina virtual.


## 🚀 Despliegue
Para desplegar la máquina virtual y configurar Apache, se utilizó el siguiente comando: 
```vagrant up```

Esto descargó la box de Ubuntu 18.04, configuró la red, instaló Apache y sincronizó la carpeta del host con la máquina virtual.

## 🌐 Acceso al Servidor Apache
Después de iniciar la máquina virtual, se accedió a la máquina utilizando:

```vagrant ssh```

Luego para encontrar la ip asignada por DHCP

```ip addr show```

Finalmente se accedió al servidor Apache desde un navegador utilizando la siguiente url:

http://192.168.56.3

## 🎯 Conclusión
Esta práctica demostró cómo Vagrant puede automatizar el despliegue de entornos de desarrollo, en este caso, configurando un servidor web Apache en una máquina virtual. El proceso fue exitoso y permitió acceder al servidor web desde la máquina host a través de la IP asignada dinámicamente.

