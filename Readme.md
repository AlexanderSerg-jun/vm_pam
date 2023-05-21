Vagrant-стенд c PAM

Цель домашнего задания
Научиться создавать пользователей и добавлять им ограничения
Создаем  Vagrantfile и запустим нашу ВМ командой vagrant up. Будет создана одна виртуальная машина. 
![Снимок экрана от 2023-05-21 14-34-27](https://github.com/AlexanderSerg-jun/vm_pam/assets/85576634/af112bf2-fccb-40e1-8c7d-273713c9d3d3)
Настройка запрета для всех пользователей (кроме группы Admin) логина в выходные дни (Праздники не учитываются)

1. Подключаемся к нашей созданной ВМ: vagrant ssh
2. Переходим в root-пользователя: sudo -i
3. Создаём пользователя otusadm и otus: sudo useradd otusadm && sudo useradd otus
4. Создаём пользователям пароли: echo "Otus2022!" | sudo passwd --stdin otusadm && echo "Otus2022!" | sudo passwd --stdin otus
Для примера мы указываем одинаковые пароли для пользователя otus и otusadm
![Снимок экрана от 2023-05-21 14-30-47](https://github.com/AlexanderSerg-jun/vm_pam/assets/85576634/1c71e32f-29e9-48ff-9399-4e2dc1ec09fe)
