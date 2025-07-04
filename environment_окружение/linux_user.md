Вот краткая инструкция по созданию нового пользователя в Linux:

1. Откройте терминал.
2. Выполните команду для создания пользователя (замените `username` на нужное имя):

```sh
    sudo adduser username
```

3. Следуйте инструкциям для задания пароля и заполнения информации о пользователе.
4. Чтобы добавить пользователя в группу `sudo` (для прав администратора):

```sh
    sudo usermod -aG sudo username
```

5. Проверьте, что пользователь создан:

```sh
    id username
```

Пользователь готов к использованию.




Чтобы добавить пользователя в группы `sudo` и `docker`, выполните в терминале (замените `username` на имя пользователя):

```sh
    sudo usermod -aG sudo,docker username
```

После этого пользователь получит права администратора и сможет работать с Docker без `sudo`. Для применения изменений выйдите и снова войдите в систему под этим пользователем.

Чтобы создать пользователя без пароля и разрешить вход только по SSH-ключу:

1. Создайте пользователя без пароля:

```sh
sudo adduser --disabled-password username
```

2. Создайте папку для ключей и задайте права:

```sh
sudo mkdir -p /home/username/.ssh
sudo chown username:username /home/username/.ssh
sudo chmod 700 /home/username/.ssh
```

3. Добавьте публичный SSH-ключ в файл `authorized_keys`:

```sh
echo "ssh-rsa AAAA... user@host" | sudo tee /home/username/.ssh/authorized_keys
sudo chown username:username /home/username/.ssh/authorized_keys
sudo chmod 600 /home/username/.ssh/authorized_keys
```

4. Убедитесь, что в `/etc/ssh/sshd_config` установлены параметры:

```
PasswordAuthentication no
PubkeyAuthentication yes
```

5. Перезапустите SSH-сервис:

```sh
sudo systemctl restart ssh
```

Теперь пользователь сможет входить только по SSH-ключу, без пароля.

Чтобы удалить пользователя в Linux вместе с его домашней директорией, используйте команду:

```sh
sudo deluser --remove-home username
```

Где `username` — имя пользователя, которого нужно удалить.