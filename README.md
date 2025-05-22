TP 4 : Déploiement d'Odoo avec Docker Compose
Ce projet déploie une instance d'Odoo (un ERP open-source) avec une base de données PostgreSQL et pgAdmin pour la gestion, en utilisant Docker Compose. Il a été réalisé dans le cadre du TP 4, en couvrant les points 1 à 4 des exigences. La persistance des données (point 5) et le déploiement en cluster Docker Swarm (point 6) ne sont pas encore fonctionnels.
Fonctionnalités

1)Services déployés : Odoo, PostgreSQL, et pgAdmin.
2)Configuration : Chaque service a un nom, un hostname, des ports exposés, une image Docker, un réseau commun, et des volumes (bien que la persistance ne soit pas encore effective).
3)Redémarrage automatique : Les conteneurs redémarrent en cas d’arrêt ou de crash (restart: always).
4)Dépendances : Odoo est configuré pour attendre que PostgreSQL soit prêt avant de démarrer.

5) Pour odoo on aura les volumes persistants vers:
	- /var/lib/odoo/filestore.
	-/mnt/extra-addons.

6) Créer un cluster de conteneurs avec dockerswarm
	-minimum 2 nœuds.
	-Failover du deploiement plus haut.

Prérequis
Pour lancer ce projet, vous devez avoir :

Docker installé sur votre machine.
Docker Compose installé.
Git pour cloner le dépôt.

Installation et Configuration

Cloner le projet :
git clone https://github.com/votre-utilisateur/tp4-odoo.git
cd tp4-odoo


Créer un fichier .env dans le répertoire du projet avec les variables suivantes :
POSTGRES_USER=odoo
POSTGRES_PASSWORD=mysecretpassword
POSTGRES_DB=odoo
PGADMIN_EMAIL=votre-email@example.com
PGADMIN_PASSWORD=admin123


Vérifier le fichier docker-compose.yml (fourni dans le dépôt) qui contient :

Service odoo : utilise l’image odoo:18, exposé sur le port 8069.
Service postgresql : utilise l’image postgres:13, exposé sur le port 5432.
Service pgadmin : utilise l’image dpage/pgadmin4, exposé sur le port 5050.
Un réseau commun odoo_network et des volumes pour chaque service.



Lancement des Services

Démarrer les conteneurs en arrière-plan :
docker-compose up -d


Vérifier que les conteneurs tournent :
docker ps

Vous devriez voir trois conteneurs : odoo, postgresql, et pgadmin.

Accéder aux services :

Odoo : Ouvrez http://localhost:8069 dans votre navigateur.
pgAdmin : Ouvrez http://localhost:5050, connectez-vous avec PGADMIN_EMAIL et PGADMIN_PASSWORD.
PostgreSQL : Connectez-vous via :docker exec -it postgresql psql -U odoo -d odoo





Réalisation des Points 1 à 4
Point 1 : Création des trois services

Les services Odoo, PostgreSQL et pgAdmin sont définis dans docker-compose.yml.
Vérification : docker ps liste les trois conteneurs.

Point 2 : Configuration des services

Noms : Visibles dans docker ps.
Hostnames : Testez avec docker exec -it odoo hostname (retourne odoo), etc.
Ports : 8069 (Odoo), 5432 (PostgreSQL), 5050 (pgAdmin).
Images : odoo:18, postgres:13, dpage/pgadmin4.
Réseau : docker network inspect tp4-odoo_odoo_network montre les conteneurs connectés.
Volumes : docker volume ls liste les volumes, mais la persistance n’est pas effective.

Point 3 : Redémarrage automatique

Configuré avec restart: always dans docker-compose.yml.
Vérification : Simulez un crash (docker kill odoo) et vérifiez que le conteneur redémarre (docker ps).

Point 4 : Gestion des dépendances

Odoo dépend de PostgreSQL via depends_on dans docker-compose.yml.
Vérification : Arrêtez PostgreSQL (docker stop postgresql), redémarrez (docker-compose up -d), et vérifiez les logs d’Odoo (docker-compose logs odoo) pour confirmer qu’il attend PostgreSQL.

Problèmes Connus

Persistance (point 5) : Les données dans /var/lib/odoo/filestore et /var/lib/postgresql/data ne persistent pas après un arrêt des conteneurs.
Docker Swarm (point 6) : Non implémenté dans ce projet.

Dépannage

Si Odoo ne charge pas : Vérifiez que PostgreSQL est démarré (docker ps) et consultez les logs (docker-compose logs odoo).
Si pgAdmin ne répond pas : Assurez-vous que le port 5050 n’est pas utilisé par une autre application.

Contribution
Pour suggérer des améliorations, ouvrez une issue ou une pull request sur le dépôt GitHub.Licence
Ce projet est sous licence MIT.
