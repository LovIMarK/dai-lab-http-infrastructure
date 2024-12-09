# Objectifs

L'objectif principal de ce TP est d'apprendre à construire une infrastructure Web complète. Cela signifie que nous allons mettre en place une infrastructure serveur qui servira un site Web statique et une API HTTP dynamique. Le diagramme ci-dessous montre l'architecture de l'infrastructure que nous allons créer.

En plus de la nécessité de servir du contenu statique et dynamique, l'infrastructure aura les caractéristiques suivantes :

- **Scalabilité** : tant le serveur statique que le serveur dynamique seront déployés en tant que clusters de plusieurs instances. Le proxy inverse sera configuré pour répartir la charge entre les instances.
- **Sécurité** : la connexion entre le navigateur et le proxy inverse sera chiffrée via HTTPS.
- **Gestion** : une application Web sera déployée pour gérer l'infrastructure. Cette application permettra de démarrer/arrêter des instances de serveurs et de surveiller l'état de l'infrastructure.

## Instructions générales

C'est un grand TP et vous aurez besoin de beaucoup de temps pour le terminer.  
Vous travaillerez en binôme et utiliserez un workflow Git pour collaborer.  
Pour certaines étapes, vous devrez faire des recherches dans la documentation par vous-même (nous sommes là pour vous aider, mais nous ne vous donnerons pas des instructions pas à pas !) ou faire preuve de créativité (ne vous attendez pas à des lignes directrices complètes).  
Lisez attentivement les critères d'acceptation de chaque étape. Ils vous diront ce que vous devez faire pour compléter l'étape.  
Après le TP, chaque groupe fera une courte démo de son infrastructure.  
Vous devez rédiger un rapport avec une courte description pour chaque étape. Faites-le directement dans le dépôt, dans un ou plusieurs fichiers markdown. Commencez par le fichier README.md à la racine de votre répertoire.  
Le rapport doit contenir la procédure que vous avez suivie pour prouver que votre configuration est correcte (ce que vous avez fait pour que l'étape fonctionne et ce que vous feriez si vous faisiez une démo).

---

## Étape 0 : Dépôt GitHub

Créez un dépôt GitHub pour votre projet. Vous utiliserez ce dépôt pour collaborer avec votre coéquipier. Vous l'utiliserez aussi pour soumettre votre travail.

### Important

Soyez attentif à garder une structure claire du dépôt afin que les différents composants soient bien séparés.

### Critères d'acceptation
- Vous avez créé un dépôt GitHub pour votre projet.
- Le dépôt contient un fichier Readme que vous utiliserez pour documenter votre projet.

---

## Étape 1 : Site Web statique

L'objectif de cette étape est de construire une image Docker qui contient un serveur HTTP statique Nginx. Le serveur servira un site Web statique. Ce site Web sera une seule page avec un modèle attrayant. Vous pouvez utiliser un modèle gratuit par exemple sur Free-CSS ou Start Bootstrap.

### Critères d'acceptation
- Vous avez créé un dossier séparé dans votre dépôt pour votre serveur Web statique.
- Vous avez un Dockerfile basé sur l'image Nginx. Le Dockerfile copie le contenu du site statique dans l'image.
- Vous avez configuré le fichier nginx.conf pour servir le contenu statique sur un port (généralement 80).
- Vous êtes capable d'expliquer le contenu du fichier nginx.conf.
- Vous pouvez lancer l'image et accéder au contenu statique depuis un navigateur.
- Vous avez documenté votre configuration dans votre rapport.

---

## Étape 2 : Docker Compose

L'objectif de cette étape est d'utiliser Docker Compose pour déployer une première version de l'infrastructure avec un seul service : le serveur Web statique.

En plus de la configuration Docker Compose de base, nous voulons pouvoir reconstruire l'image Docker du serveur Web. Consultez la documentation Docker Compose Build pour cette partie.

### Critères d'acceptation
- Vous avez ajouté un fichier de configuration Docker Compose à votre dépôt GitHub.
- Vous pouvez démarrer et arrêter une infrastructure avec un serveur Web statique via Docker Compose.
- Vous pouvez accéder au serveur Web sur votre machine locale sur le port respectif.
- Vous pouvez reconstruire l'image Docker avec `docker compose build`.
- Vous avez documenté votre configuration dans votre rapport.

---

## Étape 3 : Serveur API HTTP

Cette étape nécessite plus de travail. L'objectif est de construire une API HTTP avec Javalin. Vous pouvez implémenter n'importe quelle API de votre choix, par exemple :

- une API pour gérer une liste de citations du jour
- une API pour gérer une liste de tâches
- une API pour gérer une liste de personnes

Utilisez votre imagination et soyez créatif !  
La seule exigence est que l'API supporte toutes les opérations CRUD, c'est-à-dire : Créer, Lire, Mettre à jour, Supprimer.

Utilisez un outil de test d'API comme Insomnia, Hoppscotch ou Bruno pour tester toutes ces opérations.

Le serveur n'a pas besoin d'utiliser une base de données. Vous pouvez stocker les données en mémoire. Mais si vous souhaitez ajouter une base de données, n'hésitez pas à le faire.

Une fois l'implémentation terminée, créez un Dockerfile pour le serveur API. Ajoutez-le ensuite en tant que service dans votre configuration Docker Compose.

### Critères d'acceptation
- Votre API supporte toutes les opérations CRUD.
- Vous êtes capable d'expliquer votre implémentation et de parcourir le code.
- Vous pouvez démarrer et arrêter le serveur API avec Docker Compose.
- Vous pouvez accéder à la fois à l'API et au serveur statique depuis votre navigateur.
- Vous pouvez reconstruire l'image Docker avec Docker Compose.
- Vous pouvez faire une démo où vous utilisez un outil de test d'API pour montrer que toutes les opérations CRUD fonctionnent.
- Vous avez documenté votre implémentation dans votre rapport.

---

## Étape 4 : Proxy inverse avec Traefik

L'objectif de cette étape est de placer un proxy inverse devant les serveurs Web dynamiques et statiques de manière à ce que le proxy inverse reçoive toutes les connexions et les relaie vers les serveurs Web respectifs.

Vous utiliserez Traefik comme proxy inverse. Traefik interagit directement avec Docker pour obtenir la liste des serveurs backend actifs. Cela signifie qu'il peut s'ajuster dynamiquement au nombre de serveurs en cours d'exécution. Traefik a la particularité qu'il peut être configuré à l'aide d'étiquettes dans le fichier Docker Compose. Cela signifie que vous n'avez pas besoin d'écrire un fichier de configuration pour Traefik, mais que Traefik lira les configurations des conteneurs à partir du moteur Docker via le fichier `/var/run/docker.sock`.

Les étapes à suivre pour cette section sont donc :

1. Ajouter un nouveau service "reverse_proxy" dans votre fichier Docker Compose en utilisant l'image Docker Traefik.
2. Lire la documentation Quick Start de Traefik pour établir la configuration de base.
3. Lire la documentation Traefik & Docker pour apprendre à configurer Traefik pour fonctionner avec Docker.
4. Implémenter le proxy inverse :
   - Relayer les requêtes arrivant sur "localhost" vers le serveur HTTP statique.
   - Relayer les requêtes arrivant sur "localhost/api" vers le serveur API. Consultez la documentation des routeurs Traefik pour gérer les routes basées sur des préfixes de chemins.
5. Vous devrez supprimer la configuration des ports dans le serveur statique et dynamique dans le fichier Docker Compose et les remplacer par une configuration `expose`. Traefik pourra alors accéder aux serveurs via le réseau interne de Docker.
6. Vous pouvez utiliser le tableau de bord de Traefik pour surveiller l'état du proxy inverse.

### Critères d'acceptation
- Vous pouvez faire une démo où vous partez d'un environnement Docker "vide" (aucun conteneur en cours d'exécution) et, à l'aide de Docker Compose, vous pouvez démarrer votre infrastructure avec 3 conteneurs : serveur statique, serveur dynamique et proxy inverse.
- Dans la démo, vous pouvez accéder à chaque serveur depuis le navigateur. Vous pouvez prouver que le routage est effectué correctement par le proxy inverse.
- Vous êtes capable d'expliquer comment vous avez implémenté la solution et de parcourir la configuration et le code.
- Vous êtes capable d'expliquer pourquoi un proxy inverse est utile pour améliorer la sécurité de l'infrastructure.
- Vous êtes capable d'expliquer comment accéder au tableau de bord de Traefik et comment il fonctionne.
- Vous avez documenté votre configuration dans votre rapport.

---

## Étape 5 : Scalabilité et répartition de la charge

L'objectif de cette section est de permettre à Traefik de détecter dynamiquement plusieurs instances des serveurs (dynamiques/statique). Vous avez peut-être déjà fait cela à l'étape 3.

Modifiez votre fichier Docker Compose de manière à ce que plusieurs instances de chaque serveur soient lancées. Vérifiez que le proxy inverse répartit les connexions entre les différentes instances. Ensuite, trouvez un moyen de mettre à jour dynamiquement le nombre d'instances de chaque service avec Docker Compose, sans avoir à arrêter et redémarrer la topologie.

### Critères d'acceptation
- Vous pouvez utiliser Docker Compose pour démarrer l'infrastructure avec plusieurs instances de chaque serveur (statique et dynamique).
- Vous pouvez ajouter et supprimer dynamiquement des instances de chaque serveur.
- Vous pouvez faire une démo pour montrer que Traefik effectue la répartition de la charge entre les instances.
- Si vous ajoutez ou supprimez des instances, vous pouvez montrer que le répartiteur de charge est mis à jour dynamiquement pour utiliser les instances disponibles.
- Vous avez documenté votre configuration dans votre rapport.

---

## Étape 6 : Répartition de la charge avec round-robin et sessions persistantes

Par défaut, Traefik utilise un round-robin pour répartir la charge entre toutes les instances disponibles. Cependant, si un service est avec état, il serait préférable d'envoyer les requêtes de la même session toujours vers la même instance. C'est ce qu'on appelle les sessions persistantes.

L'objectif de cette étape est de modifier la configuration de manière à ce que :

- Traefik utilise des sessions persistantes pour les instances du serveur dynamique (service API).
- Traefik continue à utiliser le round-robin pour les serveurs statiques (aucun changement requis).

### Critères d'acceptation
- Vous avez une configuration pour démontrer la notion de session persistante.
- Vous prouvez que votre répartiteur de charge peut répartir les requêtes HTTP de manière round-robin vers les nœuds du serveur statique (car il n'y a pas d'état).
- Vous prouvez que votre répartiteur de charge peut gérer les sessions persistantes lors du transfert des requêtes HTTP vers les nœuds du serveur dynamique.
- Vous avez documenté votre configuration et votre procédure de validation dans votre rapport.

---

## Étape 7 : Sécurisation de Traefik avec HTTPS

Toute infrastructure Web réelle doit être sécurisée avec HTTPS au lieu du HTTP en clair. L'objectif de cette étape est de configurer Traefik pour utiliser HTTPS avec les clients. Le schéma ci-dessous montre l'architecture.

Cela signifie que HTTPS est utilisé pour la connexion avec les clients, via Internet. À l'intérieur de l'infrastructure, les connexions entre le proxy inverse et les serveurs se font toujours en HTTP en clair.

### Certificat
Pour ce faire, vous devrez d'abord générer un certificat de chiffrement. Étant donné que le système n'est pas exposé à Internet, vous ne pouvez pas utiliser un certificat public comme Let's Encrypt, mais devez générer un certificat auto-signé. Vous pouvez le faire en utilisant `openssl`.

Une fois que vous avez les deux fichiers (certificat et clé), vous pouvez les placer dans un dossier qui doit être monté en tant que volume dans le conteneur Traefik. Vous pouvez monter ce volume à n'importe quel chemin dans le conteneur, par exemple `/etc/traefik/certificates`.

### Fichier de configuration Traefik
Jusqu'à présent, vous avez configuré Traefik via des étiquettes directement dans le fichier Docker Compose. Cependant, il n'est pas possible de spécifier l'emplacement des certificats à Traefik avec des étiquettes. Vous devez créer un fichier de configuration `traefik.yaml`.

Encore une fois, vous devrez monter ce fichier dans le conteneur Traefik en tant que volume, à l'emplacement `/etc/traefik/traefik.yaml`.

Le fichier de configuration doit contenir plusieurs sections :

- La section `providers` pour configurer Traefik afin qu'il lise la configuration depuis Docker.
- La section `entrypoints` pour configurer deux points d'entrée : http et https.
- La section `tls` pour configurer les certificats TLS. Indiquez l'emplacement des certificats comme le chemin où vous avez monté le répertoire dans le conteneur (par exemple `/etc/traefik/certificates`).
- Pour rendre le tableau de bord accessible, vous devez configurer la section `api`. Vous pouvez supprimer les étiquettes respectives du fichier Docker Compose.

### Activation du point d'entrée HTTPS pour les serveurs
Enfin, vous devez activer HTTPS pour les serveurs statique et dynamique. Cela se fait dans le fichier Docker Compose. Vous devez ajouter deux étiquettes à chaque serveur :

- pour activer le point d'entrée HTTPS,
- pour définir TLS sur `true`.

Consultez la documentation Traefik pour Docker pour ces deux étiquettes.

### Test
Après ces configurations, il devrait être possible d'accéder aux serveurs statiques et dynamiques via HTTPS. Le navigateur se plaindra que les sites ne sont pas sécurisés, puisque le certificat est auto-signé. Mais vous pouvez ignorer cet avertissement.

Si cela ne fonctionne pas, allez sur le tableau de bord Traefik et vérifiez la configuration des routeurs et des points d'entrée.

### Critères d'acceptation
- Vous pouvez faire une démo où l'accès au proxy inverse se fait en HTTPS.
- Vous pouvez expliquer la configuration de Traefik avec HTTPS.
- Vous êtes capable d'expliquer le rôle des certificats et pourquoi nous utilisons un certificat auto-signé dans cet environnement.
- Vous avez documenté votre configuration dans votre rapport.
