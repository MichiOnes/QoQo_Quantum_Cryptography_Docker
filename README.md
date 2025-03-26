# The QoQo Quantum Cryptography Docker

*QoQo Quantum Cryptography Docker* is a tool that allows to (locally) simulate cryptographic protocols using classical (modern), quantum and post-quantum cryptography. It relies on the *software* *[Docker](https://www.docker.com/)* to get an encapsulated environment with all the libraries needed for the crypto lab of the course *Quantum internet and quantum cryptography* of the *Master in Quantum Technologies and Engineering*, at the *Universidad Carlos III de Madrid*.

This repository contains the instructions to install al launch *QoQo Quantum Cryptography Docker*.

To go to the documentation in Spanish (this README but in Spanish), click on the following [link](./README_ES.md).

## Table of contents
- [Dependencies of the QoQo Quantum Cryptography Docker](#dependencies-of-the-qoqo-quantum-cryptography-docker)
- [Running the QoQo Quantum Cryptography Docker](#running-the-qoqo-quantum-cryptography-docker)
- [How to exit from the QoQo Quantum Cryptography Docker](#how-to-exit-from-the-qoqo-quantum-cryptography-docker)
- [How to Rebuild the QoQo Quantum Cryptography Docker](#how-to-rebuild-the-qoqo-quantum-cryptography-docker)
- [Useful Docker Cli (Command Line Interface) Commands](#useful-dockere-cli-command-line-interface-commands)
- [Possible Errors with Windows Operating System](#possible-errors-with-windows-operating-system)

## Dependencies of the QoQo Quantum Cryptography Docker

To run the QoQo Docker we **should have the following dependencies already installed**:
1. ***Docker Desktop***: open source software for deploying applications on software containers isolated from the main host. To download it, click on the next link [link](https://www.docker.com/).
2. ***Visual Studio Code*** (VSC): Integrated Development Environment (IDE) for source code developed by *Microsoft*.  To download it, click on the next link [link](https://code.visualstudio.com/).
3. ***Git***: version control software designed by *Linus Torvalds*. We will use *Git* to clone this repository on our *PC*, in case the files are not provided by the course teacher. To download it, click on the next link [link](https://git-scm.com/downloads).
4. **Extension *dev-containers* by *Visual Studio Code***: *Visual Studio Code* extension that allows you to run *Docker* containers and to replace the execution environment used and displayed in *Visual Studio Code* by that of the *Docker* container. To install this dependency, follow the next steps:

    1. Open *Visual Studio Code* and **click on the square icon that contains four small squares**  (one is separated from the other three). Leaving the mouse pointer on that icon will display a label stating that this is the icon for the *Extensions*, as shown in the next image:<br>
        <img src="./images_readme_md/install_dev_container/icone_extension.png" alt="Icone Extensions Visual Studio Code" width="400" height="270">

    After clicking, **in the left panel of *Visual Studio Code*, a new sub-panel will open, next to the icon column**, as shown in the next image:<br>
        <img src="./images_readme_md/install_dev_container/page_extensions.png" alt="Page Extensions Visual Studio Code" width="400" height="270">

    2. In the Extensions sub-panel, **use the search box, at the top of the sub-panel, to search for *Dev Container***, as shown in the next image:<br>
        <img src="./images_readme_md/install_dev_container/page_extensions_dev_container.png" alt="Page Extensions Dev Container Visual Studio Code" width="400" height="270">
    
    3. **Click on the extension named *Dev Container*** created by *Microsoft* and that should have the aspect shown in the next image:<br>
        <img src="./images_readme_md/install_dev_container/page_extension_container.png" alt="Icone Dev Container Extension Visual Studio Code" width="600" height="170">

    After clicking, the next page will be opened in the main panel of *Visual Studio Code*:<br>
        <img src="./images_readme_md/install_dev_container/page_dev_container.png" alt="Icone Dev Container Extension Visual Studio Code" width="400" height="270">

    4. In the previous page, **click on the *Install* button located below the extension name**, as shown in the next image:<br>
        <img src="./images_readme_md/install_dev_container/install_dev_container.png" alt="Install Dev Container Extension Visual Studio Code" width="400" height="270">

    After following the steps above, the *Visual Studio Code* *Dev Containers* extension will have been installed. To check that **the extension is correctly working**, follow the **next steps**:
    1. **Click the 'facing' arrows in the bottom left-hand corner of the *Visual Studio Code* window**, as shown in the image:<br>
        <img src="./images_readme_md/install_dev_container/comprobation_install_1.png" alt="Install Dev Container Extension Visual Studio Code" width="400" height="270">

    2. **In the top of the page where the search box of *Visual Studio Code* is located** we will find the options highlighted in the next image:<br>
        <img src="./images_readme_md/install_dev_container/comprobation_install_2.png" alt="Install Dev Container Extension Visual Studio Code" width="400" height="270">

## Running the QoQo Quantum Cryptography Docker

Once you have installed all the required dependencies listed in the previous section [Dependencies of the QoQo Quantum Cryptography Docker](#dependencies-of-the-qoqo-quantum-cryptography-docker) we are ready to run the simulation environment, following the next steps:

1. First, we must **specify the path to the project  *QoQo Quantum Cryptography Docker*** in your **PC**, to link the *docker* container with the folder `volumen_archivos_docker`. This will allow that all the files stay in that folder (i.e., it will be the working folder). The *path* must be specified differently depending on the operating system where the *docker* is being deployed:
    - ***Windows***: in this case we will have to modify some variables defined in the project files. Follow the next steps:
        1. Open the *Windows* File Explorer.
        2. Go to the project folder.
        3. Click on the address bar (where the name of the folder is shown).
        4. The full *path* will be shown, for example: `C:\Users\TuUsuario\Documents\MiCarpeta`.
        5. Copy that full *path* clicking the keyboard keys *Ctrl + C*.
        6. Replace in the file [/.devcontainer/docker-compose.yml](./.devcontainer/docker-compose.yml) the volume defined in the section *Volumes* with `- C:/Users/TuUsuario/Documents/MiCarpeta/volumen_archivos_docker/:/volumen_archivos_docker/`. **Notice that the left-side bars \ must be replaced by right-side bars / , which are the ones supported by  *Docker***. Next image shows a snapshot of the file after modification [/.devcontainer/docker-compose.yml](./.devcontainer/docker-compose.yml):<br>
            <img src="./images_readme_md/windows_path/docker_compose_path_windows.png" alt="Windows Path Docker Compose" width="400" height="270">

        7. Save the changes done in the file [/.devcontainer/docker-compose.yml](./.devcontainer/docker-compose.yml).

        8. Replace in the file [/.devcontainer/devcontainer.json](./.devcontainer/devcontainer.json) the *mount* defined in the section *mounts* by `"source=${localEnv:USERPROFILE}/volumen_archivos_docker,target=/volumen_archivos_docker,type=bind"`. Next image shows a snapshot of the file after modification [/.devcontainer/devcontainer.json](./.devcontainer/devcontainer.json):<br>
            <img src="./images_readme_md/windows_path/devcontainer_path_windows.png" alt="Windows Path Dev Container" width="400" height="270">

        9. Save the changes done in the file [/.devcontainer/devcontainer.json](./.devcontainer/devcontainer.json).

    - ***Mac y Linux***: the project can be run without any problem, as *Docker* will infer the *paths*.

2. **Open *Docker Desktop*** to run the *Docker* *daemon* and deploy *dockers*.

3. **Open the folder of the project *QoQo Quantum Cryptography Docker* in *Visual Studio Code***.

4. To **open and run the *Dev Container*** we have **two options**:
    - **(Option 1) Open the folder of the project *QoQo Quantum Cryptography Docker* in *Visual Studio Code***: in the bottom righ-hand corner a box will appear indicating that if you click on *Reopen in Container* the *Docker* container will be launched and *Visual Studio Code* will open a window linked with the *docker* and its volume. This volume will be the working folder where you can create source code files and run them --in the container, from the *Visual Studio Code* window--. Next image shows where the said box is shown:<br>
        <img src="./images_readme_md/dev_container_execution/dev_container_reopen_principal_page.png" alt="Dev Container Open Principal Page" width="400" height="270">

    - **(Option 2) Follow the next steps**:
        1. **Click on the `facing' arrows in the bottom left-side corner**, as shown in the next image:<br>
        <img src="./images_readme_md/dev_container_execution/access_to_connection_menu.png" alt="Connection Menu" width="400" height="270">

        2. A contextual menu will open where **clicking on *Reopen in Container*** will run the container as explained in Option 1. Next, the contextual menu is shown in the image, with the required option highligthed:<br>
        <img src="./images_readme_md/dev_container_execution/dev_contianer_reopen_open_new_connection_menu.png" alt="Reopen or Open New Dev Container Connection Menu" width="400" height="270">

5. After the previous step, **a new window of *Visual Studio Code* will open, that will be linked to the *docker* *QoQo Quantum Cryptography Docker***. Next image illustrates the window that opens when the container is launched:<br>
    <img src="./images_readme_md/dev_container_execution/dev_container_link_visual_studio_code.png" alt="Dev Container Link Visual Studio Code" width="400" height="270">

6. **To check that we are in *Docker*** we can  **visualize in the bottom left-side corner** (where the 'facing' arrows are) of the new windom of *Visual Studio Code* something similar to **`Dev Container @ desktop-linux`**. This means that we are in the *Docker* environment.

## How to exit from the QoQo Quantum Cryptography Docker

To **exit from the simulation environment** we have to follow the next steps:
1. **Click on the 'facing' arrows in the bottom left-side corner of the window** (where something similar to `Dev Container @ desktop-linux` is shown). Next image shows the location of this icon:<br>
    <img src="./images_readme_md/dev_container_exit/dev_container_link_visual_studio_code.png" alt="Dev Container Link Visual Studio Code" width="400" height="270">

2. **A contextual menu will open where you can clik on *Close Remote Connection***, and *Visual Studio Code* will exit from the *docker* and the window will close. Next, an image illustrating this step:<br>
    <img src="./images_readme_md/dev_container_exit/dev_container_exit_button.png" alt="Dev Container Exit Button" width="400" height="270">

## How to rebuild the QoQo Quantum Cryptography Docker 

In case of needing to **rebuild the simulation environment**, e.g., we have installed some libraries that we do not need anymore or some library connection with *Python* is broken, follow the next steps:
1. **Click on the 'facing' arrows icon located in the bottom left-hand corner of the window**, where something similar to `Dev Container @ desktop-linux` is shown. Next image shows the location of the icon:<br>
    <img src="./images_readme_md/dev_container_rebuild/dev_container_link_visual_studio_code.png" alt="Dev Container Link Visual Studio Code" width="400" height="270">

2. **A contextual menu will open where we can click on the option *Rebuild Container***, *Visual Studio Code* will restart the window. Next image illustrates the said option:<br>
    <img src="./images_readme_md/dev_container_rebuild/dev_containe_rebuild_option.png" alt="Dev Container Rebuild Button" width="400" height="270">

## Useful Docker Cli (Command Line Interface) Commands

List of  **useful *Docker Cli* commands**:
- List active *dockers* --> `docker ps`
- List all *dockers* (active and non active) --> `docker ps -a`
- Stopping an active *docker*  --> `docker stop <id_or_docker_name>`
- Delete a *docker* --> `docker rm <id_or_docker_name>`
- List *docker* images --> `docker image ls`
- Delete a *docker* image --> `docker rmi <id_or_docker_image_name>`
- To visualize the space used by images, volumes and containers in Docker, and also how much of that space is not being used by active *dockers* (and can be deleted) --> `docker system df`
- Delete the cache of the *docker* *builder* --> `docker builder prune`

**Notice that all these actions can also be run from *Docker Desktop***.

## Possible Errors with Windows Operating System

If **running the *Docker* on *Windows* operating system, raises the error *Mount denied* or *permission denied***, we can follow the next steps to solve the issue:

1. Open *Docker Desktop*.
2. Click on the configuration (gear) icon, located in the top right-hand corner, that will open the *Settings*.
3. In the left panel, click on *Resources* --> *File sharing*.
4. In this page, at the bottom, we will see a list of folders or disks that can be shared with *Docker* containers.
5. Check that the disk or folder where the project is located appears in that list. For example, if your project is in `D:\proyectos\docker`, the disk `D:` should be shared. If the only disk appearing is `C:` but your project is in a different disk, select the disk or folder and click on the symbol *+* to add that *path* to the list.
6. Click on *Apply & Restart* to apply the changes.

### License

This project is licenced under GNU GENERAL PUBLIC LICENSE v3.0 - see file [LICENSE](./LICENSE) for additional details.

### Credits 

Designed and Created by Miguel Salas Heras [LinkedIn](https://www.linkedin.com/in/miguelsalasheras/), as part of his Bachelor Thesis *Diseño y desarrollo de entorno de simulación de protocolos criptográficos pre-cuánticos, cuánticos y post-cuánticos*

Supervisor of the Bachelor Thesis: Ana Isabel Gonzalez-Tablas Ferreres [LinkedIn](https://www.linkedin.com/in/ana-isabel-gonzalez-tablas-ferreres-31b5331/)
