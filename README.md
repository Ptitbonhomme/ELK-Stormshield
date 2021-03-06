# ELK-Stormshield
	Procédure d'installation de configuration de la suite ELK pour la gestion de logs d'équipements Stormshield

#### //---/!\---// LA SUITE LOGICIEL DOIT ETRE INSTALLEE PAR LA METHODE RPM POUR CENTOS //---/!\---//
	
* https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html
		
#### //---/!\---// PROCEDURE VALABLE POUR LA VERSION 7.5 DE LA SUITE LOGICIELLE //---/!\---//

* Mes fichiers de conf sont inspirés de ce guide :
	https://blog.ineat-group.com/2019/03/monitoring-comment-tirer-profit-des-logs-de-votre-pare-feu-avec-elassandra/
	
	- Sur Kibana il vous faudra créer l'index pattern : firewall_*

## INFOS VM :

* CentOS 7 x64
* 8GB de ram
* 6cores process
* Disque de 100GB minimum 
* Suite utilisée : syslog-ng --> filebeat --> logstash --> elasticsearch --> kibana

## INFOS ELK :

	1- fichiers de conf yml :		/etc/<logstash ou kibana ou elasticsearch>/
	2- Logstash fichiers de conf :		/etc/logstash/conf.d/*.conf
	3- Logstash dossier bin/logstash :	/usr/share/logstash/

## INFOS JAVA :

	- java -version :	OpenJDK-11
	- java dossier :	/usr/lib/jvm/


## PROCEDURE :

### 1 - Créer et éditer les différents fichiers de configuration

* Mis a disposition ci-dessus
* Le port d'écoute syslog-ng peut varier (UDP 514 / TCP 601) en fonction de la configuration syslog votre équipement stormshield
* Redemarrez tous les services dont la configuration a été modifiée

### 2 - Vérifier que toute la suite est fonctionnelle : systemctl status ...

* Si Logstash ne démarre pas voir partie [DEBUG] ci-dessous
* Si un autre logiciel ne démarre pas, il vous faut revoir vos fichiers de configuration

### 3 - Lancer Logstash avec la conf de votre choix :

* CETTE PARTIE N'EST PAS NECESSAIRE SI VOUS N'AVEZ QU'UN SEUL FICHIER DE CONF

	1- cd /usr/share/logstash
	2- bin/logstash -f /etc/logstash/conf.d/<Votre_Fichier>.conf
### 4 - Chiffrement des flux de logs et protection compte utilisateur

https://www.elastic.co/fr/blog/configuring-ssl-tls-and-https-to-secure-elasticsearch-kibana-beats-and-logstash#enable-ts-logstash

### 5 - DEBUG : 

	-	journalctl -xe
	-	tcpdump
	-	netstast -tnlp
	-	nmap -p 601 <IP_du_PC>

	- 	SI LOGSTASH NE DEMARRE PAS ET MONTRE UNE ERREUR "libjli.so : no such file or directory" :

		1- cd /etc/ld.so.conf.d/
		2- vim java.conf
		3- AJOUTER DANS LE FICHIER : /usr/share/elasticsearch/jdk/lib
		4- ldconfig
		
## UPGRADE DU TRAITEMENT DES LOGS :

* Ajouter la partie FILTRE au fichier logstash.conf
	- Ajouter la géolocalisation des IPs
* Sécuriser le trafic des logs
