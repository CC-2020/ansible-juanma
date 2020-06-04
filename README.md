Ansible con GCP
    
      Crea una máquina virtual GCP

      Creamos un proyecto en GCP y en Compute Engine creamos una máquina virtual Ubuntu Server 18.04
        
Instala Ansible

      Nos conectamos por ssh y ejecutamos:

      sudo apt update && sudo apt upgrade
      sudo apt install software-properties-common
      sudo apt-add-repository ppa:ansible/ansible
      sudo apt install ansible

Crea dos máquinas virtuales

      Igual que antes

Crea un inventario con esas máquinas

      nano ansible-playbooks/hosts



      Una vez creadas las demás maquinas virtuales, así queda nuestro inventario

      [servers]
      server1 ansible_host=174.138.13.97
      server2 ansible_host=188.166.115.68
      server3 ansible_host=188.166.123.245

      Debe haber instalada una versión de SSH para generar un par de claves RSA y
      autenticar por SSH:

      -En la VM 1 creamos el par de claves publica/privada RSA.

      cd
      mkdir -p .ssh
      cd .ssh
      ssh-keygen -t dsa -P '' -f id_dsa #'' son dos comillas sin espacio
      ls -l #comprobamos se han creado los ficheros id_dsa.pub y id_dsa.pub


      LA CLAVE SSH TENDRÁ QUE SER AÑADIDA A SSH METADATA, como se habla aquí

      https://cloud.google.com/compute/docs/instances/adding-removing-ssh-keys





      Para cada VM, realizamos dos pasos:
      a.-Añadir el fichero id_dsa.pub al fichero ~/.ssh/authorized_keys.

      Desde el ordenador 1
      cd
      cd .ssh
      ssh ubuntu@<ip> mkdir -p .ssh
      cat id_dsa.pub | ssh ubuntu@<ip> "cat - >>.ssh/authorized_keys"

      b.-Modificar los permisos para poder acceder sin contraseña.

      Desde VM 1 accedemos al pc destino y modificamos permisos
      ssh usulocal@<ip> #sustituir <ip_worker-node> por una IP
      Solicita contraseña y se le facilita
      cd ~/.ssh
      chmod go-rwx .
      chmod go-rw authorized_keys


Crea un PlayBook para instalar Apache, PHP y MySQL

      En la VM 1
      Hemos usado los ficheros de lamp_ubuntu1804/

      https://github.com/do-community/ansible-playbooks.git

      debemos editar:

      nano ansible-playbooks/hosts 

      con:
      [servers]
      server1 ansible_host=174.138.13.97
      server2 ansible_host=188.166.115.68
      server3 ansible_host=188.166.123.245
      
      
