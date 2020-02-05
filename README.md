# ELK-Stormshield
Procédure d'installation de configuration de la suite ELK pour la gestion de logs d'équipements Stormshield

//---/!\---// LA SUITE LOGICIEL DOIT ETRE INSTALLEE PAR LA METHODE RPM POUR CENTOS //---/!\---//
//---/!\---// PROCEDURE VALABLE POUR LA VERSION 7.5 DE LA SUITE LOGICIELLE //---/!\---//

INFOS VM :

CentOS 7 x64
4GB de ram
2cores process

INFOS ELK :

	1- fichiers de conf yml :			/etc/<logstash ou kibana ou elasticsearch>/
	2- Logstash fichiers de conf :		/etc/logstash/conf.d/*.conf
	3- Logstash dossier bin/logstash :		/usr/share/logstash/

INFOS JAVA :

java -version :				OpenJDK-11
java dossier :				/usr/lib/jvm/


PROCEDURE :

1 - Vérifier que toute la suite est fonctionnelle (SAUF LOGSTASH): systemctl status ...

2 - Créer votre fichier de conf pour Logstash ( voir INFOS ELK )

3 - Lancer Logstash avec la conf de votre choix :

	1- cd /usr/share/logstash
	2- bin/logstash -f /etc/logstash/conf.d/<Votre_Fichier>.conf

4 - Attendre le message de validation (exemple):

	1- [INFO ] Starting tcp input listener 
	2- [INFO ]  Pipeline started 
	3- [INFO ] Pipelines running
	4- [INFO ] Starting UDP listener
	5- [INFO ] UDP listener started

5 - DEBUG (En cas d'echec): 

	-	journalctl -xe
	-	tcpdump
	-	netstast -tnlp
	-	nmap -p 601 <IP_du_PC>

	- 	SI LOGSTASH NE DEMARRE PAS ET MONTRE UNE ERREUR "libjli.so : no such file or directory" :

		1- cd /etc/ld.so.conf.d/
		2- vim java.conf
		3- AJOUTER DANS LE FICHIER : /usr/share/elasticsearch/jdk/lib
		4- ldconfig
