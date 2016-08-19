# Кластер Redis

## Как работать:
1. Раскомментировать в /etc/ansible/ansible.cfg строчку host_key_checking = False

2. Запускаем Vagrant up -- создаются ВМ, копируются ключи для пользователя vagrant, генерируется vagrant_ansible_inventory

3. Создается файл .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory его используем для отладки скриптов Ansible

4. запуск скриптов командой: ansible-playbook -v -i .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory services/redis_cluster_full_install.yml


## Примечание:
