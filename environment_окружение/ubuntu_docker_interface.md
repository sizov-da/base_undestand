Установить Lazydocker на Ubuntu 22.04
14 сентября 2021 г. (17 января 2023 г.)
Ubuntu 
0 Комментариев
17168 Просмотров
Lazydocker это инструмент пользовательского интерфейса на базе терминала, который позволяет управлять контейнерами, образами и томами для Docker и Docker Compose. Lazydocker - это проект с открытым исходным кодом, написанный на языке программирования Go.

В этом руководстве показано, как установить Lazydocker в Ubuntu 22.04.





Install Lazydocker on Ubuntu 22.04
 September 14, 2021 (January 17, 2023)
 Ubuntu
 0 Comments
 17168 Views
Lazydocker is a terminal based UI tool that allows to manage containers, images and volumes for Docker and Docker Compose. Lazydocker is an open-source project written in the Go programming language.

This tutorial shows how to install Lazydocker on Ubuntu 22.04.

Prepare environment
Before starting, make sure you have installed Docker. You can read post how to install it.

Install Lazydocker
Get the latest version tag of Lazydocker release from GitHub. Assign version tag to variable.

    LAZYDOCKER_VERSION=$(curl -s "https://api.github.com/repos/jesseduffield/lazydocker/releases/latest" | grep -Po '"tag_name": "v\K[0-9.]+')

Download archive from releases page of the Lazydocker repository.

    curl -Lo lazydocker.tar.gz "https://github.com/jesseduffield/lazydocker/releases/latest/download/lazydocker_${LAZYDOCKER_VERSION}_Linux_x86_64.tar.gz"
    mkdir lazydocker-temp
    tar xf lazydocker.tar.gz -C lazydocker-temp

Move binary file to /usr/local/bin directory:

    sudo mv lazydocker-temp/lazydocker /usr/local/bin

Now lazydocker can be used as a system-wide command for all users.

We can check Lazydocker version:

    lazydocker --version
Archive and temporary directory is no longer necessary, remove them:

rm -rf lazydocker.tar.gz lazydocker-temp
Testing Lazydocker
Run hello-world image inside a container:

    docker run hello-world
Start Lazydocker:

    lazydocker
