# Mini-projet Ansible

objectif: installation d'application web utilisant les roles Ansible

## infrastructures utilisées:

-un serveur master Ubuntu 22

-un  worker Ubuntu 22

-un worker  CentOS 7

## outil de provisionning

-Vagrant avec Virtualbox

## prerequis 

Le projet necessite une mise en place de connexions ssh depuis le serveur ansible master vers les differents worker (Ubuntu 22 et CentOS 7)

Pour se faire,

-sur le serveur Ansible master , lancer la commande "ssh-keygen -t rsa"

-recuperer le contenu du fichier "id_ras_pub" sous /root/.ssh et aller copier sur les workers dans le fichier "authorized_keys" sous /root/.ssh (de chaque worker  )

NB:  l'installation de docker-debian neccessite le compte "root", car sinon le script n'arrive pas a écrire dans certains repertoires comme /var/lib/dpkg/ etc... sur le worker Ubuntu 22.

L'objectif de cette manipulation est de rendre les connexions ssh en automatique vers les workers.

## mise en place du projet

-initialisation du repertoire role :

Aller sur ansible master et lancer la commande :

ansible-galaxy init ansible_project 

Elle cree l'arborescence du projet.

-defaults/:  main.yml, contient tous les variables utilisés( par defaut)

-tasks/: 3 fichiers YML ont été créés 

- docker-debian.yml, contient les differntes taches d'installation de docker sur un OS de la famille Debian (Ubuntu dans notre cas)

- docker-redhat.yml, contient les differntes taches d'installation de docker sur un OS de la famille Redhat (CentOS dans notre cas)

- main.yml, appelle les 2 fichiers docker-debian.yml et docker-redhat.yml , copie le site sur les differents workers, installe et lance docker-compose pour deployer l'application sur les workers

-templates/: contient les fichiers templates docker-compose.yml.j2 et index.html.j2 , ces fichiers vont etre copiés sur les workers lors de l'execution du tasks/main.yml

-tests/: contient les fichiers inventory "hosts_ubuntu.yaml" ou figure les workers sur lesquels on deploie l'application et le fichier principal " test.yml"

test.yml est donc le playbook a jouer , entre autre il execute le role "ansible_project"

## execution du playbook

Aller sur le serveur ansible master, et lancer la commande suivante:

ansible-playbook -i hosts_ubuntu.yaml test.yml

Elle permet de deployer l'application sur une machine client quelque soit la famille d'OS installé ( famille Debian et famille Redhat dans notre cas).

## les logs d'execution

-deploiement sur le worker Ubuntu 22:



![ansible-playbook_run_on_ubuntu22](https://github.com/ravelonanosy/mini-projet-ansible/assets/138290448/3fc31ced-4630-411f-bad2-01d37632aa03)



-deploiement sur le worker CentOS 7:


![ansible-playbook_run_on_centos7](https://github.com/ravelonanosy/mini-projet-ansible/assets/138290448/2a805052-1c6b-456b-92ca-1e725fd5400d)













