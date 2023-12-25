# Домашнее задание к занятию 5 «Тестирование roles»

## Подготовка к выполнению

1. Установите molecule: `pip3 install "molecule==3.5.2"` и драйвера `pip3 install molecule_docker molecule_podman`.
2. Выполните `docker pull aragast/netology:latest` —  это образ с podman, tox и несколькими пайтонами (3.7 и 3.9) внутри.

## Основная часть

Ваша цель — настроить тестирование ваших ролей. 

Задача — сделать сценарии тестирования для vector. 

Ожидаемый результат — все сценарии успешно проходят тестирование ролей.

### Molecule

1. Запустите  `molecule test -s centos_7` внутри корневой директории clickhouse-role, посмотрите на вывод команды. Данная команда может отработать с ошибками, это нормально. Наша цель - посмотреть как другие в реальном мире используют молекулу.
2. Перейдите в каталог с ролью vector-role и создайте сценарий тестирования по умолчанию при помощи `molecule init scenario --driver-name docker`.
3. Добавьте несколько разных дистрибутивов (centos:8, ubuntu:latest) для инстансов и протестируйте роль, исправьте найденные ошибки, если они есть.
4. Добавьте несколько assert в verify.yml-файл для  проверки работоспособности vector-role (проверка, что конфиг валидный, проверка успешности запуска и др.). 
5. Запустите тестирование роли повторно и проверьте, что оно прошло успешно.
6. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

### Tox

1. Добавьте в директорию с vector-role файлы из [директории](./example).
2. Запустите `docker run --privileged=True -v <path_to_repo>:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash`, где path_to_repo — путь до корня репозитория с vector-role на вашей файловой системе.
3. Внутри контейнера выполните команду `tox`, посмотрите на вывод.
5. Создайте облегчённый сценарий для `molecule` с драйвером `molecule_podman`. Проверьте его на исполнимость.
6. Пропишите правильную команду в `tox.ini`, чтобы запускался облегчённый сценарий.
8. Запустите команду `tox`. Убедитесь, что всё отработало успешно.
9. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

После выполнения у вас должно получится два сценария molecule и один tox.ini файл в репозитории. Не забудьте указать в ответе теги решений Tox и Molecule заданий. В качестве решения пришлите ссылку на  ваш репозиторий и скриншоты этапов выполнения задания. 

## Необязательная часть

1. Проделайте схожие манипуляции для создания роли LightHouse.
2. Создайте сценарий внутри любой из своих ролей, который умеет поднимать весь стек при помощи всех ролей.
3. Убедитесь в работоспособности своего стека. Создайте отдельный verify.yml, который будет проверять работоспособность интеграции всех инструментов между ними.
4. Выложите свои roles в репозитории.

В качестве решения пришлите ссылки и скриншоты этапов выполнения задания.

---

# Ответы

### Molecule

1. Очень польшой лог для скриншота, поэтому текстом:
<details><summary>Clickhouse_test:</summary>
   
```
root@bubuntu:/home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse# molecule test -s centos_7
INFO     centos_7 scenario test matrix: dependency, lint, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun...
INFO     Set ANSIBLE_LIBRARY=/root/.cache/ansible-compat/7e099f/modules:/root/.ansible/plugins/modules:/usr/share/ansible/plugins/modules
INFO     Set ANSIBLE_COLLECTIONS_PATH=/root/.cache/ansible-compat/7e099f/collections:/root/.ansible/collections:/usr/share/ansible/collections
INFO     Set ANSIBLE_ROLES_PATH=/root/.cache/ansible-compat/7e099f/roles:/root/.ansible/roles:/usr/share/ansible/roles:/etc/ansible/roles
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/hosts.yml linked to /root/.cache/molecule/clickhouse/centos_7/inventory/hosts
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/group_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/group_vars
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/host_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/host_vars
INFO     Running centos_7 > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/hosts.yml linked to /root/.cache/molecule/clickhouse/centos_7/inventory/hosts
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/group_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/group_vars
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/host_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/host_vars
INFO     Running centos_7 > lint
COMMAND: yamllint .
ansible-lint
flake8

WARNING: PATH altered to include /usr/bin
WARNING  Loading custom .yamllint config file, this extends our internal yamllint config.
an AnsibleCollectionFinder has not been installed in this process
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/hosts.yml linked to /root/.cache/molecule/clickhouse/centos_7/inventory/hosts
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/group_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/group_vars
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/host_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/host_vars
INFO     Running centos_7 > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/hosts.yml linked to /root/.cache/molecule/clickhouse/centos_7/inventory/hosts
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/group_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/group_vars
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/host_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/host_vars
INFO     Running centos_7 > destroy
INFO     Sanity checks: 'docker'

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos_7)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
ok: [localhost] => (item=centos_7)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/hosts.yml linked to /root/.cache/molecule/clickhouse/centos_7/inventory/hosts
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/group_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/group_vars
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/host_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/host_vars
INFO     Running centos_7 > syntax

playbook: /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/resources/playbooks/converge.yml
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/hosts.yml linked to /root/.cache/molecule/clickhouse/centos_7/inventory/hosts
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/group_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/group_vars
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/host_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/host_vars
INFO     Running centos_7 > create

PLAY [Create] ******************************************************************

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'capabilities': ['SYS_ADMIN'], 'command': '/usr/sbin/init', 'dockerfile': '../resources/Dockerfile.j2', 'env': {'ANSIBLE_USER': 'ansible', 'DEPLOY_GROUP': 'deployer', 'SUDO_GROUP': 'wheel', 'container': 'docker'}, 'image': 'centos:7', 'name': 'centos_7', 'privileged': True, 'tmpfs': ['/run', '/tmp'], 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup']})

TASK [Create Dockerfiles from image names] *************************************
changed: [localhost] => (item={'capabilities': ['SYS_ADMIN'], 'command': '/usr/sbin/init', 'dockerfile': '../resources/Dockerfile.j2', 'env': {'ANSIBLE_USER': 'ansible', 'DEPLOY_GROUP': 'deployer', 'SUDO_GROUP': 'wheel', 'container': 'docker'}, 'image': 'centos:7', 'name': 'centos_7', 'privileged': True, 'tmpfs': ['/run', '/tmp'], 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup']})

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'diff': [], 'dest': '/root/.cache/molecule/clickhouse/centos_7/Dockerfile_centos_7', 'src': '/root/.ansible/tmp/ansible-tmp-1703419735.8741012-18385-8138045693111/source', 'md5sum': 'e90d08cd34f49a5f8a41a07de1348618', 'checksum': '4b70768619482424811f2977aa277a5acf2b13a1', 'changed': True, 'uid': 0, 'gid': 0, 'owner': 'root', 'group': 'root', 'mode': '0600', 'state': 'file', 'size': 2199, 'invocation': {'module_args': {'src': '/root/.ansible/tmp/ansible-tmp-1703419735.8741012-18385-8138045693111/source', 'dest': '/root/.cache/molecule/clickhouse/centos_7/Dockerfile_centos_7', 'mode': '0600', 'follow': False, '_original_basename': 'Dockerfile.j2', 'checksum': '4b70768619482424811f2977aa277a5acf2b13a1', 'backup': False, 'force': True, 'unsafe_writes': False, 'content': None, 'validate': None, 'directory_mode': None, 'remote_src': None, 'local_follow': None, 'owner': None, 'group': None, 'seuser': None, 'serole': None, 'selevel': None, 'setype': None, 'attributes': None}}, 'failed': False, 'item': {'capabilities': ['SYS_ADMIN'], 'command': '/usr/sbin/init', 'dockerfile': '../resources/Dockerfile.j2', 'env': {'ANSIBLE_USER': 'ansible', 'DEPLOY_GROUP': 'deployer', 'SUDO_GROUP': 'wheel', 'container': 'docker'}, 'image': 'centos:7', 'name': 'centos_7', 'privileged': True, 'tmpfs': ['/run', '/tmp'], 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup']}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
changed: [localhost] => (item=molecule_local/centos:7)

TASK [Create docker network(s)] ************************************************
skipping: [localhost]

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'capabilities': ['SYS_ADMIN'], 'command': '/usr/sbin/init', 'dockerfile': '../resources/Dockerfile.j2', 'env': {'ANSIBLE_USER': 'ansible', 'DEPLOY_GROUP': 'deployer', 'SUDO_GROUP': 'wheel', 'container': 'docker'}, 'image': 'centos:7', 'name': 'centos_7', 'privileged': True, 'tmpfs': ['/run', '/tmp'], 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup']})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos_7)

TASK [Wait for instance(s) creation to complete] *******************************
failed: [localhost] (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j330869775869.19083', 'results_file': '/root/.ansible_async/j330869775869.19083', 'changed': True, 'item': {'capabilities': ['SYS_ADMIN'], 'command': '/usr/sbin/init', 'dockerfile': '../resources/Dockerfile.j2', 'env': {'ANSIBLE_USER': 'ansible', 'DEPLOY_GROUP': 'deployer', 'SUDO_GROUP': 'wheel', 'container': 'docker'}, 'image': 'centos:7', 'name': 'centos_7', 'privileged': True, 'tmpfs': ['/run', '/tmp'], 'volumes': ['/sys/fs/cgroup:/sys/fs/cgroup']}, 'ansible_loop_var': 'item'}) => {"ansible_job_id": "j330869775869.19083", "ansible_loop_var": "item", "attempts": 2, "changed": false, "finished": 1, "item": {"ansible_job_id": "j330869775869.19083", "ansible_loop_var": "item", "changed": true, "failed": 0, "finished": 0, "item": {"capabilities": ["SYS_ADMIN"], "command": "/usr/sbin/init", "dockerfile": "../resources/Dockerfile.j2", "env": {"ANSIBLE_USER": "ansible", "DEPLOY_GROUP": "deployer", "SUDO_GROUP": "wheel", "container": "docker"}, "image": "centos:7", "name": "centos_7", "privileged": true, "tmpfs": ["/run", "/tmp"], "volumes": ["/sys/fs/cgroup:/sys/fs/cgroup"]}, "results_file": "/root/.ansible_async/j330869775869.19083", "started": 1}, "msg": "Unsupported parameters for (community.docker.docker_container) module: command_handling. Supported parameters include: api_version, auto_remove, blkio_weight, ca_cert, cap_drop, capabilities, cgroup_parent, cleanup, client_cert, client_key, command, comparisons, container_default_behavior, cpu_period, cpu_quota, cpu_shares, cpus, cpuset_cpus, cpuset_mems, debug, default_host_ip, detach, device_read_bps, device_read_iops, device_requests, device_write_bps, device_write_iops, devices, dns_opts, dns_search_domains, dns_servers, docker_host, domainname, entrypoint, env, env_file, etc_hosts, exposed_ports, force_kill, groups, healthcheck, hostname, ignore_image, image, init, interactive, ipc_mode, keep_volumes, kernel_memory, kill_signal, labels, links, log_driver, log_options, mac_address, memory, memory_reservation, memory_swap, memory_swappiness, mounts, name, network_mode, networks, networks_cli_compatible, oom_killer, oom_score_adj, output_logs, paused, pid_mode, pids_limit, privileged, published_ports, pull, purge_networks, read_only, recreate, removal_wait_timeout, restart, restart_policy, restart_retries, runtime, security_opts, shm_size, ssl_version, state, stop_signal, stop_timeout, sysctls, timeout, tls, tls_hostname, tmpfs, tty, ulimits, user, userns_mode, uts, validate_certs, volume_driver, volumes, volumes_from, working_dir (cacert_path, cert_path, docker_api_version, docker_url, expose, exposed, forcekill, key_path, log_opt, ports, tls_ca_cert, tls_client_cert, tls_client_key, tls_verify).", "results_file": "/root/.ansible_async/j330869775869.19083", "started": 1, "stderr": "/tmp/ansible_community.docker.docker_container_payload_1dqiycgr/ansible_community.docker.docker_container_payload.zip/ansible_collections/community/docker/plugins/modules/docker_container.py:1169: DeprecationWarning: The distutils package is deprecated and slated for removal in Python 3.12. Use setuptools or check PEP 632 for potential alternatives\n", "stderr_lines": ["/tmp/ansible_community.docker.docker_container_payload_1dqiycgr/ansible_community.docker.docker_container_payload.zip/ansible_collections/community/docker/plugins/modules/docker_container.py:1169: DeprecationWarning: The distutils package is deprecated and slated for removal in Python 3.12. Use setuptools or check PEP 632 for potential alternatives"], "stdout": "", "stdout_lines": []}
FAILED - RETRYING: [localhost]: Wait for instance(s) creation to complete (300 retries left).

PLAY RECAP *********************************************************************
localhost                  : ok=6    changed=3    unreachable=0    failed=1    skipped=2    rescued=0    ignored=0

CRITICAL Ansible return code was 2, command was: ['ansible-playbook', '--inventory', '/root/.cache/molecule/clickhouse/centos_7/inventory', '--skip-tags', 'molecule-notest,notest', '/usr/local/lib/python3.10/dist-packages/molecule_docker/playbooks/create.yml']
WARNING  An error occurred during the test sequence action: 'create'. Cleaning up.
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/hosts.yml linked to /root/.cache/molecule/clickhouse/centos_7/inventory/hosts
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/group_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/group_vars
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/host_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/host_vars
INFO     Running centos_7 > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/hosts.yml linked to /root/.cache/molecule/clickhouse/centos_7/inventory/hosts
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/group_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/group_vars
INFO     Inventory /home/jlljully/mnt-homeworks/08-ansible-05-testing/roles/clickhouse/molecule/centos_7/../resources/inventory/host_vars/ linked to /root/.cache/molecule/clickhouse/centos_7/inventory/host_vars
INFO     Running centos_7 > destroy

PLAY [Destroy] *****************************************************************

TASK [Destroy molecule instance(s)] ********************************************
changed: [localhost] => (item=centos_7)

TASK [Wait for instance(s) deletion to complete] *******************************
FAILED - RETRYING: [localhost]: Wait for instance(s) deletion to complete (300 retries left).
ok: [localhost] => (item=centos_7)

TASK [Delete docker networks(s)] ***********************************************
skipping: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0

INFO     Pruning extra files from scenario ephemeral directory
```
</details>

2. скрин1

