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

