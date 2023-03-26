# Instructions pour le TPs 

Forker dans votre compte github le repo https://github.com/crunchy-devops/tp-coaching-webforce3.git  
Faire un git clone de ce repo en local dans votre directory c:\projet  
Faire un git clone dans la home directorie de votre VM fournie 
Ouvrir la directorie **tp-coaching-webforce3** dans PyCharm.

## Résultats et attentes
Vous devez compléter le fichier README.md avec toutes les instructions nécessaires pour
faire les différents exercices.

## Exercice 1  - Scrum 
Voici les détails de ce mini-projet.  
*Installer sur une VM (fournie), un serveur Web en Python Flask fonctionnant sur le port 30101. 
Ce serveur écrit les actions des utilisateurs dans un disque attaché à la VM. 
écrire les instructions pour le pare-feu*

**Préparer le dashboard Scrum pour ce projet.**

Utilisez un outil de dashboard Scrum/Kanban de votre choix, ou simplement créer un project dans github
Faire une copie d'écran de votre dashboard et placez le fichier de l'image 
dans git/github pour qu'il soit versionné 


## Exercice 2  - Linux 
Mettre à jour les packages de votre VM ubuntu  
Vérifier la version de python3 déjà installée  
Le programme python est executé avec le nom python3, créer un alias nommé python valide pour le user ubuntu de votre VM 
vérifier en faisant  ```python -V```  
Faire un ```pip install flask``` , suivre les instructions pour installer pip si nécessaire

## Exercice 3  - Storage 

Recherche le disque supplémentaire de 1Gb connecté à la VM, précisez la commande utilisée  
Formattez ce disque au format ext4   
Monter (mount) ce disque sur le point montage /home/ubuntu/tp-coaching-webforce3/log


## Exercice 4  - Git/Github 
Dans PyCharm allez dans File->Settings->Version control->github  
Appuyer sur la croix, en haut a gauche de cette fenetre et selectionnez log in with token.   
entrez votre token github  
Vous pouvez maintenant faire des git commit et git push depuis PyCharm   

Faire régulierement des commit/push dans github de votre machine local vers github
Comme vous avez fait un git clone de votre projet sur votre VM, vous devez
faire des git pull pour mettre jour votre Web serveur sans utiliser d'éditeur 
dans votre VM, ne pas faire de git push à partir de la VM sinon vous risquez 
d'avoir des conflits à résoudre entre les push faits de votre machine locale et ceux faits à partir de la VM. 

## Exercice 5  - Python
Créez un fichier blogs.py  
Copier le script python suivant  

```python
from flask import Flask
import logging
 
app = Flask(__name__)
 
logging.basicConfig(filename='log/record.log', level=logging.DEBUG, format=f'%(asctime)s %(levelname)s %(name)s %(threadName)s : %(message)s')
 
@app.route('/blogs')
def blog():
    app.logger.info('Info level log')
    app.logger.warning('Warning level log')
    return f"Welcome to the Blog"
 
app.run(host='localhost', debug=True)
```
Avec l'aide de la documentation Flask, et de la documentation Python mettre des commentaires 
dans ce script.

Ajouter une variable d'environnement ```FLASK_APP=blogs``` , mettre cette variable dans le fichier ~/.bashrc
de votre user ubuntu  
Lancer le web server avec la commande ```flask run --host=0.0.0.0 -p 30101```  
Vérifier avec votre navigateur en utilisant l'url ```http://<ip_de_votre_vm>:30101/blogs```    
Vérifier que le fichier record.log existe bien dans la directory log    


## Exercice 6  - Pare-feu  
Trouvez la commande de gestion du firewall sous ubuntu 20.04  
Exemple : fermer le port 5000 et autoriser le port 30101  
Vérifier l'application Web sur ces ports  

---

## Ansible 1  Installation avec virtualenv 
Mettre en place ansible dans votre VM. 
Nous allons créer un virtualenv python pour installer la derniere version 
d'Ansible
```shell
cd ~/tp-coaching-webforce3
python3 -m venv venv  # set up the module venv in the directory venv
source venv/bin/activate  # activate the virtualenv python
pip3 install wheel  # set for permissions purpose
pip3 install --upgrade pip # update pip3
pip3 install ansible # install ansible 
pip3 install requests # extra packages
pip3 install natsort # require for an ansible filter
ansible --version # check the version number
```
## TP ansible 1 
Dans une sous directory de votre projet tp-coaching-webforce3 nommée **ansible**   
Créer un fichier ansible-1.yaml qui automatise l'exercice 2 ci-dessus.  
1. Le script doit mettre à jour les packages ubuntu.   
2. Vérifier la version de python3  
3. Créer un alias dans ~/.bashrc  
4. installer le package pip
Testez votre script

## TP ansible 2 
Dans une sous directory de votre projet tp-coaching-webforce3 nommée **ansible**   

Trouvez le fichier ansible-2-filtre.yml qui affiche les devices en mode raw
1. Analysez le fichier ansible-2-filtre.yml
2. Dans la directory filter_plugins etudier le code de la fonction get_device
3. Regardez egalement le fichier ansible.cfg, mettre des commentaires dans le README.md
Ce filtre doit etre utilise en local, pas sur une machine remote

Une maniere qui cette fois fonctionne en remote est le script ansible-2.yml  
4. Analysez le fichier ansible-2.yml  
5. Executez le  
6. Le script ansible-2-filter.yml ne formatte pas le disk. Modifier le script ansible-2-filter.yaml pour qu'il formatte le disque en
en vous inspirant du script ansible-2.yml 


## TP ansible 3 
Dans une sous directory de votre projet tp-coaching-webforce3 nommée **ansible**   
Créer un fichier ansible-3.yaml qui automatise l'exercice 6 ci-dessus.  
1. activer le firewall d'ubuntu
2. fermer le port 5000
3. ouvrir le port 30101  
Testez votre script

## TP ansible 4 
Copiez votre cle ssh sur la machine remote.      
Creer votre fichier inventory pour acceder depuis ansible vers cette machine. 
Nommez un groupe leader qui contient cette machine. comme par exemple
```shell
[leader]
centos01 ...
```
Faire la commande ansible Ad-hoc pour verifier la connectivite.      
Dans votre home directory creez une directory webforce.   
Dans cette directory , creer un role postgresql.role   
Dans la directory webforce , creer un playbook postgres.yml qui utilise le role  
faire les commandes ansible Ad-hoc pour verifier OS et la version de centos01  
Dans role postgresql.role  et dans la directory tasks  
Creez un fichier nommee variables.yml , Copiez le code suivant:  
```yaml
---
# Variables configuration
- name: Include OS-specific variables (Centos)
  include_vars: "{{ ansible_distribution }}-{{ ansible_distribution_version.split('.')[0] }}.yml"
  when: ansible_distribution == "CentOS"
```
Etudiez ce code, et commentez le dans votre README.md
Editez le fichier main.yml dans la directory tasks
Copiez le code suivant:
```yaml
- include_tasks: variables.yml

# Setup /install task
- include_tasks: setup-CentOS.yml
  when: ansible_distribution == 'CentOS'

- include_tasks: initialize.yml

- name: Ensure Postgresql is started and enable on boot
  service:
    name: "{{ postgresql_daemon }}"
    state: "{{ postgresql_service_state }}"
    enabled: "{{ postgresql_service_enabled }}"

- import_tasks: users.yml
```
dans la directory tasks copiez le code dans setup-CentOS.yml 
```yaml
---
- name: Check if the postgresql packages are installed
  yum:
    name: "{{ postgresql_packages }}"
    state: present

- name: Check if the postgresql librairies are installed
  yum:
    name: "{{ postgresql_python_library }}"
    state: present
```
dans la directory tasks copiez le code dans initialize.yml
```yaml
---
- name: Set Postgresql environment variables
  template:
    src: postgres.sh.j2
    dest: /etc/profile.d/postgres.sh
    mode: 0644
  notify: restart postgresql

- name: Check if Postgresql directory exists
  file:
    path: "{{ postgresql_data_dir }}"
    owner: "{{ postgresql_user }}"
    group: "{{ postgresql_group }}"
    state: directory
    mode: 0700

- name: Check if Postgresql database is initialized
  stat:
    path: "{{ postgresql_data_dir }}/PG_VERSION"
  register: pgdata_dir_version
  tags:
    - db_init

- name: Ensure PostgreSQL database is initialized
  command: "{{ postgresql_bin_path }}/initdb -D {{ postgresql_data_dir }}"
  when: not pgdata_dir_version.stat.exists
  become: true
  become_user: "{{ postgresql_user }}"
```

dans la directory tasks copiez le code dans users.yml
```yaml
---
- name: Ensure Postgresql users are present
  postgresql_user:
    name: "{{ item.name }}"
    password: "{{ item.password | default(omit) }}"
    encrypted: "{{ item.encrypted | default(omit) }}"
    priv: "{{ item.priv | default(omit) }}"
    role_attr_flags: "{{ item.role_attr_flags | default(omit) }}"
    db: "{{ item.db | default(omit) }}"
    login_host: "{{ item.login_host | default(omit) }}"
    login_password: "{{ item.login_password | default(omit) }}"
    login_user: "{{item.login_user | default(omit) }}"
    port: "{{item.port | default(omit) }}"
    state: "{{ item.state | default(omit) }}"
  with_items: "{{ postgresql_users }}"
  #no_log: "{{ postgresql__users_no_log }}"
  become: true
  become_user: "{{ postgresql_user}}"
```

Le fichier qui mixte le nom de l'OS et la version est CentOS-7.yml
```yaml
---
postgresql_version: "9.2"
postgresql_data_dir: "/var/lib/pgsql/data"
postgresql_bin_path: "/usr/bin"
postgresql_config_path: "/var/lib/pgsql/data"
postgresql_daemon: postgresql
postgresql_packages:
  - postgresql
  - postgresql-server
  - postgresql-contrib
  - postgresql-libs
postgresql_python_library:
  - postgresql-plpython
  - python-psycopg2
```
dans le fichier main.yml postgresql.role/handlers
```yaml
- name: restart postgresql
  service:
    name: "{{ postgresql_daemon}}"
    state: "{{ postgresql_restarted_state }}"
    sleep: 5
```
Pourquoi vous avez besoin d'un handler?   

dans le fichier main.yml postgresql.role/defaults
```yaml
postgresql_enablerepo: ""

postgresql_restarted_state: "restarted"

postgresql_user: postgres
postgresql_group: postgres

postgresql_service_state: started
postgresql_service_enabled: true

postgresql_users_no_log: true

postgresql_users: []
```
dans le fichier postgres.sh.j2 postgresql.role/templates
```shell
export PGDATA={{ postgresql_data_dir }}
export PATH=$PATH:{{ postgresql_bin_path }}
```

---

# TP Puppet 
Connectez vous a la VM/host de rebond fournie, entrez ```sh connect.sh```
vous devez etre sur la branche puppet
## Pre-requis
### Lancer Portainer s'il n'est pas present
```shell
docker volume create portainer_data
docker run -d -p 32125:8000 -p 32126:9443 --name portainer --restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data portainer/portainer-ce:latest
```

### Lancer un container puppetmaster
```shell
cd  # vous etes dans la home directory 
# create un lien symbolique 
ln -s  tp-coaching-webforce3 main  # lien symbolique nommee main  
cd main
docker run -d --name puppetmaster --hostname puppet  -v .:/etc/puppetlabs/code/environments/main puppet/puppetserver # container
```
### Install le module apt
```shell
docker exec -it puppetmaster puppet module install puppetlabs-apt --environment main


```
## First manifest
in your main directory
```shell
mkdir manifests
cd manifest
vi site.pp
# copy how to run a apt update with the module apt
# save 
docker exec -it puppetmaster apt -y install gnupg2 # package is missing inside puppetmaster container.  
docker exec -it puppetmaster puppet agent -t --environment main
```
# Exercice Puppet 1 :
Nous   
```shell
fichier 2-puppet.pp: Vérifier la version de python3  
3. fichier 3-puppet.pp: Créer un alias dans ~/.bashrc  
4. fichier 4-puppet.pp: installer le package pip 
# TP Puppet 
Connectez vous a la VM/host de rebond fournie, entrez ```sh connect.sh```


# Exercice Puppet 1 :
The default environment is production.  
```shell
puppet config print
puppet config print config
puppet config print manifest --section master --environment production
 
#vi /etc/puppetlabs/code/environments/production/manifests/1-puppet.pp
``` 
Placez les 4 scripts: 1-puppet.pp, 2-puppet.pp, 3-puppet.pp, 4-puppet.pp
dans /etc/puppetlabs/code/environments/production/manifests/ 


# Exercice Puppet 2 : 
Recherche le disk additionnel et le formatter
 

# Exercice Puppet 3 :
1. Activez le pare-feu
2. Bloquez le port 5000
3. Autoriser le port 30101
