# Automation 2021

## Antes de empezar

* Realizar estos pasos desconectado de la VPN para una mejor experiencia y evitar problemas con los accesos a Internet.
* Necesitará permisos de administrador local para instalar los componentes necesarios y gestionar las redes virtuales de VirtualBox.
* Existen versiones *portables* de Git y Visual Studio Code que no requieren permisos de administrador.
* Se asume que el sistema operativo donde se instalará el laboratorio es Windows 10 Pro o Enterprise versión 20H2 o más reciente.
* Activar la visualización de extensiones de nombres y archivos ocultos en **File Explorer**.
* Crear cuenta de usuario en [GitHub](https://github.com).

En las instrucciones se utilizan los siguientes parámetros como ejemplos (sustituir por los valores que correspondan a su ambiente):

* Cuenta de usuario en GitHub: `usuario-github`
* Directorio de trabajo para el laboratorio: `D:\Automation_2021` (o `/d/Automation_2021/` en `bash`)
* Nueva ubicación para las máquinas virtuales de VirtualBox: `D:\Automation_2021\VirtualBox_VMs`
* Nueva ubicación para el directorio de inicio de Vagrant: `D:\Automation_2021\.vagrant.d`
  
## Instalar Git

> Git for Windows incluye un ambiente MSYS2/MINGW64 básico con las herramientas más comunes de Linux para Windows, por ejemplo: `bash`, `vim`, `grep`, `less`, `ssh`, `gawk`, etc.

1. Descargar e instalar [Git for Windows](https://git-scm.com/download/win).

2. Abrir **Start** > **Git Bash**.

3. En la interfaz de línea de comandos (CLI) de `bash`, ejecutar los siguientes comandos (cambiar `Nombres Apellidos` y `mailbox@example.com` por los valores que correspondan a su ambiente):

```bash
git config --global user.name "Nombres Apellidos"
git config --global user.email "mailbox@example.com"
git config --global color.ui auto
```

4. Cerrar **Git Bash**.

## Instalar VirtualBox

> Es importante cambiar la ubicación de las máquinas virtuales de VirtualBox para evitar problemas de espacio en la unidad preterminada del sistema. Considerar que la nueva ubicación debe tener espacio libre suficiente para almacenar varias máquinas virtuales.

1. Descargar e instalar [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

2. Abrir **Start** > **Oracle VM VirtualBox**.

3. Desde la barra del menú de **Oracle VM VirtualBox Manager**, elegir **File** > **Preferences...** > **General** > **Default Machine Folder:** y establecer la nueva ubicación `D:\Automation_2021\VirtualBox_VMs`.

4. Dar clic en el botón **OK**.

5. Cerrar **Oracle VM VirtualBox Manager**.

## Instalar Vagrant

> Es importante cambiar la ubicación del directorio inicio de Vagrant para evitar problemas de espacio en la unidad predeterminada del sistema. Considerar que la nueva ubicación debe tener espacio libre suficiente para almacenar varias Vagrant Boxes.

1. Descargar e instalar [Vagrant](https://www.vagrantup.com/downloads).

2. Abrir **Start** > **File Explorer**.

3. Ir a la ubicación `%USERPROFILE%` desde la **Location bar**.

4. Seleccionar el directorio `.vagrant.d`.

5. Dar clic en **Home** > **Clipboard** > **Cut**.

6. Ir a la nueva ubicación `D:\Automation_2021`.

7. Dar clic en **Home** > **Clipboard** > **Paste**. Aceptar los cambios en caso de requerirse.

8. Cerrar **File Explorer**.

9. Abrir **Start** > **Windows System** > **Command Prompt**.

10. En la interfaz de línea de comandos (CLI) de **Command Prompt** (`cmd`), ejecutar el siguiente comando:

```cmd
setx VAGRANT_HOME "D:\Automation_2021\.vagrant.d"
```

11. Cerrar **Command Prompt** (`cmd`).

## Instalar IDE/editor de texto

Descargar e instalar [Visual Studio Code](https://code.visualstudio.com/Download) o su IDE/editor de texto favorito, por ejemplo: [Atom](https://atom.io/download/windows_x64), [Notepad++](https://notepad-plus-plus.org/downloads/), [Sublime Text](https://www.sublimetext.com/download), [Geany](https://geany.org/download/releases/), [Emacs](http://ftp.gnu.org/gnu/emacs/windows/emacs-27/), Vim (incluido en Git for Windows), etc.

## Clonar repositorio del laboratorio

1. Abrir **Start** > **Git Bash**.

2. En la interfaz de línea de comandos (CLI) de `bash`, ejecutar los siguientes comandos:
        
```
cd /d/Automation_2021
git clone https://github.com/karkul/ansiblelab.git
cd ansiblelab
git status
git log
```

3. Explorar el contenido del directorio, subdirectorios y archivos sin hacer cambios.

4. Estudiar el contenido del archivo `Vagrantfile` sin hacer cambios. *Al momento de escribir estas instrucciones, el laboratorio consta de dos VMs: `host1` y `tower`.*

## Levantar y explorar el laboratorio

> Para seguir los pasos de esta sección, debe continuar en la interfaz de línea de comandos (CLI) de `bash`, en el directorio `/d/Automation_2021/ansiblelab`.

1. Ejecutar el siguiente comando para levantar el laboratorio:

> La primera vez que se levante el laboratorio es probable que se requieran permisos de administrador local.

> Atención a los mensajes mostrados durante el arranque de las VMs. En estos mensajes se muestra información útil sobre el redireccionamiento de puertos y el acceso SSH, así como posibles errores que habrá que resolver para poder continuar.

> Este paso puede durar varios minutos. La primera vez que se ejecuta este comando, Vagrant descargará desde Internet las Vagrant Boxes necesarias para crear las VMs según se indica en el archivo `Vagrantfile`.

```
vagrant up
```

2. Ejecutar el siguiente comando para mostrar el estado de las VMs del laboratorio:

```
vagrant status
```

3. Si alguna VM no está corriendo o hubo errores, resolver el problema antes de continuar.

4. Conectarse e iniciar sesión en la VM `host1`. Esta VM será el nodo controlado por Ansible:

```
vagrant ssh host1
```

> También puede iniciar sesión directamente en la consola de la VM desde **Oracle VM VirtualBox Manager**, utilizando la cuenta de usuario `vagrant` con la contraseña `vagrant`.
   
> También puede conectarse e iniciar sesión con cualquier cliente SSH (OpenSSH, PuTTY, mRemoteNG, MobaXterm, etc.), utilizando las llaves privadas del usuario `vagrant` ubicadas en `/d/Automation_2021/ansiblelab/.vagrant/machines/host1/virtualbox/private_key` y `/d/Automation_2021/ansiblelab/.vagrant/machines/tower/virtualbox/private_key`, con la dirección IP `127.0.0.1`, puerto TCP `2222` o `2200`, según se indique en los mensajes mostrados durante el arranque.

5. Explorar los directorios `/home/vagrant` y `/vagrant` en la VM `host1` sin hacer cambios.

6. Para cerrar sesión en la VM `host1`, ejecutar el comando `exit` dentro de la misma VM.

7. Conectarse e iniciar sesión en la VM `tower`. Esta VM será el controlador de Ansible:

```
vagrant ssh tower
```

8. Explorar los directorios `/home/vagrant` y `/vagrant` en la VM `tower` sin hacer cambios.

9. Para cerrar sesión en la VM `tower`, ejecutar el comando `exit` dentro de la misma VM.

10. En un navegador web abrir el URL [http://127.0.0.1:8080](http://127.0.0.1:8080) que está redirigido a AWX (a.k.a. Ansible Tower) en la VM  `tower`, el controlador Ansible.

11. En la página de inicio de sesión de AWX, introducir el USERNAME `admin` y el PASSWORD `password`.

12. Dar clic en `SIGN IN`.

13. Explorar AWX sin hacer cambios.

14. Para terminar, en la interfaz de línea de comandos (CLI) de `bash`, desde el directorio `/d/Automation_2021/ansiblelab`, ejecutar el siguiente comando:

```
vagrant destroy -f
```

## Crear branch personal de trabajo para los retos

1. En la interfaz de línea de comandos (CLI) de `bash`, desde el directorio `/d/Automation_2021/ansiblelab`, ejecutar el siguiente comando para crear el branch personal de trabajo:

```
git branch usuario-github
```

2. Ejecutar el siguiente comando para activar el branch personal de trabajo:

```
git checkout usuario-github
```

3. Levantar el laboratorio en el branch personal de trabajo.

```
vagrant up
```

## Continuar con los retos

* [Reto 1](https://github.com/karkul/ansiblelab/blob/main/RETO1.md)

* [Reto 2](https://github.com/karkul/ansiblelab/blob/main/RETO2.md)

**Happy Hacking!**

---

## Referencias útiles

### Git y GitHub

* https://docs.github.com/en/get-started/quickstart/git-and-github-learning-resources

* https://training.github.com/downloads/github-git-cheat-sheet.pdf

* https://training.github.com/downloads/es_ES/github-git-cheat-sheet.pdf

* https://education.github.com/git-cheat-sheet-education.pdf

### Ansible

* https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html

* https://www.redhat.com/en/blog/system-administrators-guide-getting-started-ansible-fast

### Vagrant

* https://www.vagrantup.com/docs/vagrantfile

### Comandos de Linux

* https://www.man7.org/linux/man-pages/

### Markdown

* https://guides.github.com/features/mastering-markdown/

* https://docs.github.com/en/github/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax

* https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf
