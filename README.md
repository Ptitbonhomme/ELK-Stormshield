# ELK-Stormshield
Procédure d'installation de configuration de la suite ELK pour la gestion de logs d'équipements Stormshield

//---/!\---// LA SUITE LOGICIEL DOIT ETRE INSTALLEE PAR LA METHODE RPM POUR CENTOS //---/!\---//
//---/!\---// PROCEDURE VALABLE POUR LA VERSION 7.5 DE LA SUITE LOGICIELLE //---/!\---//

INFOS VM :

CentOS 7 x64
4GB de ram
2cores process

INFOS ELK :

fichiers de conf yml :			/etc/<logstash ou kibana ou elasticsearch>/
Logstash fichiers de conf :		/etc/logstash/conf.d/*.conf
Logstash dossier bin/logstash :		/usr/share/logstash/

INFOS JAVA :

java -version :				OpenJDK-11
java dossier :				/usr/lib/jvm/


PROCEDURE :

1 - Vérifier que toute la suite est fonctionnelle (SAUF LOGSTASH): systemctl status ...

2 - Créer votre fichier de conf pour Logstash ( voir INFOS ELK )

3 - Lancer Logstash avec la conf de votre choix :
	cd /usr/share/logstash
	bin/logstash -f /etc/logstash/conf.d/<Votre_Fichier>.conf

4 - Attendre le message de validation (exemple):
	[INFO ] 2020-02-04 11:34:21.328 [[main]<tcp] tcp - Starting tcp input listener {:address=>"0.0.0.0:601", :ssl_enable=>"false"}
	[INFO ] 2020-02-04 11:34:21.344 [[main]-pipeline-manager] javapipeline - Pipeline started {"pipeline.id"=>"main"}
	[INFO ] 2020-02-04 11:34:21.572 [Agent thread] agent - Pipelines running {:count=>1, :running_pipelines=>[:main], :non_running_pipelines=>[]}
	[INFO ] 2020-02-04 11:34:21.706 [[main]<udp] udp - Starting UDP listener {:address=>"0.0.0.0:601"}
	[INFO ] 2020-02-04 11:34:21.838 [[main]<udp] udp - UDP listener started {:address=>"0.0.0.0:601", :receive_buffer_bytes=>"106496", :queue_size=>"2000"}
	[INFO ] 2020-02-04 11:34:22.111 [Api Webserver] agent - Successfully started Logstash API endpoint {:port=>9600}

5 - DEBUG (En cas d'echec)
	-	journalctl -xe
	-	tcpdump
	-	netstast -tnlp
	-	nmap -p 601 <IP_du_PC>

	- 	SI LOGSTASH NE DEMARRE PAS ET MONTRE UNE ERREUR "libjli.so : no such file or directory" :

		1- cd /etc/ld.so.conf.d/
		2- vim java.conf
		3- AJOUTER DANS LE FICHIER : /usr/share/elasticsearch/jdk/lib
		4- ldconfig
