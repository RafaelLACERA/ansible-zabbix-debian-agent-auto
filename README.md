# ansible-zabbix-debian-agent-auto
ansible conf to remote install zabix agent
Déploiement automatique de l’agent Zabbix sur Debian

Ce projet permet d’installer et de configurer automatiquement Zabbix Agent sur des serveurs Debian (testé sur Debian 12) à l’aide d’Ansible.

📦 Prérequis

Installer Ansible sur ta machine de gestion (Ubuntu/Debian) :

sudo apt update && sudo apt install ansible -y


Cloner ce dépôt :

git clone https://github.com/RafaelLACERA/ansible-zabbix-debian-agent-auto.git
cd ansible-zabbix-debian-agent-auto


Configurer les droits sudo pour l’utilisateur qui exécute Ansible.
Édite le fichier sudoers avec :

sudo visudo


Ajoute la ligne suivante (remplace rafael par ton utilisateur) :

rafael ALL=(ALL) NOPASSWD:ALL


Cela permet à Ansible d’utiliser become: yes sans demander de mot de passe sudo.

⚙️ Configuration
1. Modifier ansible.cfg

Dans ce fichier, remplace rafael par ton utilisateur local :

[defaults]
inventory = ./VMs.cfg
remote_user = rafael

2. Modifier VMs.cfg

C’est ton fichier d’inventaire qui contient les serveurs cibles.
Exemple minimal (remplace par tes IPs) :

[prod]
192.168.1.100
192.168.1.101

🧪 Tester la connexion aux hôtes

Avant de lancer le playbook, vérifie que la connexion SSH fonctionne avec :

ansible -m ping all


👉 Tu devrais voir une sortie du type :

192.168.1.100 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
192.168.1.101 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}


Si ça échoue, vérifie :

que les clés SSH sont bien déployées,

que l’utilisateur défini dans ansible.cfg a accès aux machines.

▶️ Exécution du playbook

Lancer le déploiement de l’agent Zabbix :

ansible-playbook installation-zabbix-agent.yml

🛠️ Personnalisation

Pour changer l’adresse du serveur Zabbix, modifie la variable dans le playbook :

Server=172.16.243.141
ServerActive=172.16.243.141


Pour vérifier si l’agent est bien installé et tourne :

systemctl status zabbix-agent

✅ Résultat attendu

À la fin :

Le paquet Zabbix Agent est installé.

Le fichier /etc/zabbix/zabbix_agentd.conf contient tes paramètres (Server, ServerActive, Hostname, etc.).

Le service zabbix-agent est activé et redémarré.
