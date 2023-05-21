Vagrant-стенд c PAM

Цель домашнего задания
Научиться создавать пользователей и добавлять им ограничения
Создаем  Vagrantfile и запустим нашу ВМ командой vagrant up. Будет создана одна виртуальная машина. 
![Снимок экрана от 2023-05-21 14-36-07](https://github.com/AlexanderSerg-jun/vm_pam/assets/85576634/264c3ce9-b331-46ae-8a9d-82098363c7b6)

Настройка запрета для всех пользователей (кроме группы Admin) логина в выходные дни (Праздники не учитываются)

1. Подключаемся к нашей созданной ВМ: vagrant ssh
2. ![Снимок экрана от 2023-05-21 14-34-27](https://github.com/AlexanderSerg-jun/vm_pam/assets/85576634/af112bf2-fccb-40e1-8c7d-273713c9d3d3)
3. Переходим в root-пользователя: sudo -i
4. Создаём пользователя otusadm и otus: sudo useradd otusadm && sudo useradd otus
5. Создаём пользователям пароли: echo "Otus2022!" | sudo passwd --stdin otusadm && echo "Otus2022!" | sudo passwd --stdin otus
Для примера мы указываем одинаковые пароли для пользователя otus и otusadm
![Снимок экрана от 2023-05-21 14-30-47](https://github.com/AlexanderSerg-jun/vm_pam/assets/85576634/1c71e32f-29e9-48ff-9399-4e2dc1ec09fe)

5. Создаём группу admin: sudo groupadd -f admin
6. Добавляем пользователей vagrant,root и otusadm в группу admin:
  usermod otusadm -a -G admin && usermod root -a -G admin && usermod vagrant -a -G admin
  *в первый раз была допушена синтаксическая ошибка вместо пользователя otusadm был введен пользователь ptusadm как результат была выведена ошибка "usermod : user 'ptusadm' does not exist" (пользователь ptusadm не существует)
  
![изображение](https://github.com/AlexanderSerg-jun/vm_pam/assets/85576634/887d8f8a-529c-40e8-ac2c-42ad09483735)

После создания пользователей, нужно проверить, что они могут подключаться по SSH к нашей ВМ. Для этого пытаемся подключиться с хостовой машины: 
ssh otus@192.168.57.10
Далее вводим наш созданный пароль.  
( для работы с VM используется не классический терминал Ubuntu , а используется приложение Terminator.  на снимке две сессии. 
 1. Слева сама виртуальная машина.
 2. Подключение к этой машины из консоли локальной машины

![Снимок экрана от 2023-05-21 14-44-41](https://github.com/AlexanderSerg-jun/vm_pam/assets/85576634/d3867069-c5b9-40ab-bb37-3e6da8c55471)

Настроим правило, по которому все пользователи кроме тех, что указаны в группе admin не смогут подключаться в выходные дни:

1.Проверим, что пользователи root, vagrant и otusadm есть в группе admin (используем конвейр для более точного поиска):
![изображение](https://github.com/AlexanderSerg-jun/vm_pam/assets/85576634/03ccab3c-3c2f-4654-97c9-dd7cfa6cc803)

2. Создадим файл-скрипт /usr/local/bin/login.sh. Для создания скрипта используется текстовой редактор nano ( т.к. он в системе не установлен, то инсталируем его используя команду yum install nano)
![изображение](https://github.com/AlexanderSerg-jun/vm_pam/assets/85576634/fd5f1935-cb2b-45b2-a8a3-0c9bf30da100)

![Снимок экрана от 2023-05-21 14-52-29](https://github.com/AlexanderSerg-jun/vm_pam/assets/85576634/25dd16fa-7d6e-486f-9400-6e1dfc6bbda4)

#!/bin/bash
if [ $(date +%a) = "Sat"] || [ $(date +%a) = "Sun"]; then
if getent group admin | grep -qw "$PAM_USER"; then
        exit 0
      else
	exit 1
    fi
else 
   exit 0
fi


