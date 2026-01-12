# Lab3 : Versionnement des données et pipelines ML avec DVC

## Étape 1 : Initialisation de DVC dans le projet
###  On Lance l’initialisation de DVC
Cette commande crée les fondations de DVC dans le  dossier mlops-lab-01. Elle génère un dossier caché .dvc/ qui contient les fichiers de configuration.
<img width="1822" height="767" alt="image" src="https://github.com/user-attachments/assets/87f26df0-3bc2-4a23-a094-0c67b0808cba" />


## Étape 2 : Versionner les données brutes avec DVC
### On Supprime  data/  de notre .gitignore 
<img width="1813" height="918" alt="image" src="https://github.com/user-attachments/assets/d377dbe2-1e1f-4b43-b55f-470564666d4f" />


### On Ajoute le dataset au suivi DVC :
l'exécution de dvc add data/raw.csv permet de calculer l'empreinte numérique du fichier et de le placer dans le cache interne de DVC, tout en créant un fichier de métadonnées raw.csv.dvc

Parallèlement, DVC met à jour le fichier .gitignore pour exclure le fichier lourd du suivi Git, garantissant ainsi la légèreté du dépôt.

Enfin, le commit Git des fichiers .dvc et .gitignore fige la version exacte des données associée au code, permettant une traçabilité complète sans saturer l'historique du projet.
<img width="1884" height="735" alt="image" src="https://github.com/user-attachments/assets/793a145f-fd4d-4630-bf6c-b2b49f85005a" />

## Étape 3 : Configuration d’un remote DVC
En créant le dossier dvc_storage:Ce dossier servira de serveur simulé pour stocker physiquement les versions des fichiers volumineux.
et en l'enregistrant avec dvc remote add, on définisse une destination sécurisée pour archiver les données en dehors de Git.

On a  ajouté le fichier .dvc/config à Git et effectué un commit.
<img width="1879" height="897" alt="image" src="https://github.com/user-attachments/assets/5af6dedf-f52c-47d1-846f-883028457490" />

## Étape 4 : Push des données dans le remote DVC
On Exécuton de la commande dvc push .DVC télécharge physiquement le contenu de fichiers (raw.csv) depuis la cache local vers le dossier dvc_storage. 
Le terminal confirme "1 file pushed".

<img width="1887" height="258" alt="image" src="https://github.com/user-attachments/assets/59cec189-485c-49de-9771-6ebaadf98919" />

On observe dans l'arborescence de fichiers que le dossier dvc_storage se remplit de sous-dossiers (nommés 3d, 05, 7e). Ce sont des fichiers de hash
<img width="1267" height="611" alt="image" src="https://github.com/user-attachments/assets/f0fc4ca2-9a22-4f40-8dd5-ef1c25cf3654" />
## Étape 5 : simulation d’une collaboration : supprimer localement et récupérer depuis DVC

Ona supprimé le fichier physique avec del data\raw.csv. La commande ls data/ confirme que le fichier a disparu, ne laissant que le petit fichier raw.csv.dvc.

 En exécutant dvc pull, DVC va chercher le contenu correspondant au hash stocké dans le fichier .dvc depuis le  "remote storage" (dvc_storage)

<img width="1871" height="983" alt="image" src="https://github.com/user-attachments/assets/99ceee8f-8223-4e97-8705-6b5040064c2d" />


## Étape 6 : Création d’un pipeline reproductible dvc.yaml

Ajout des étapes prepare, train, et evaluate via dvc stage add. 
Pour automatisez le flux de travail

<img width="1886" height="1715" alt="image" src="https://github.com/user-attachments/assets/8a2871bc-963c-4cea-8370-e1d1eff73b87" />

## Étape 7 : Reproduire automatiquement tout le pipeline


Le pipeline est exécuté avec succès, produisant des métriques (accuracy, precision, etc.). Le fichier dvc.lock est mis à jour pour enregistrer l'état exact des résultats produits.

<img width="1919" height="1436" alt="image" src="https://github.com/user-attachments/assets/c9aa1bc9-42e0-4865-b353-11df05290708" />


<img width="1915" height="1535" alt="image" src="https://github.com/user-attachments/assets/be1507a3-a8b1-42fc-a10c-b8309750df53" />

## Conclusion

Ce lab a permis de démontrer l'importance de DVC (Data Version Control) comme pilier d'un projet MLOps moderne. En intégrant DVC à notre flux de travail Git, nous avons résolu les défis majeurs liés à la gestion des projets de science des données.

Les points clés retenus sont :

Gestion optimisée des données : Nous avons appris à séparer le code (géré par Git) des données volumineuses (gérées par DVC), garantissant un dépôt léger et performant.

Sécurité et Collaboration : La mise en place d'un "remote storage" et l'utilisation des commandes push et pull assurent que les datasets sont sauvegardés et accessibles à tous les collaborateurs de manière identique.

Reproductibilité Totale : Grâce à la définition d'un pipeline dans dvc.yaml et au verrouillage des versions dans dvc.lock, nous pouvons désormais garantir que n'importe quelle étape (préparation, entraînement ou évaluation) peut être reproduite avec les mêmes résultats.

Traçabilité : Le lien permanent entre une version du code et une version spécifique des données permet un audit complet du modèle, ce qui est essentiel pour le déploiement en production.


# Lab 4 : Mise en place d’un pipeline CI/CD complet pour un projet Machine Learning
## Étape 1 : Créer le dépôt GitHub et connecter le remote
L'objectif de cette étape est d'établir le pont entre votre travail local (déjà versionné avec Git et DVC) et la plateforme GitHub pour permettre l'automatisation CI/CD.
Cette étape est déja realisé 

## Étape 2 : Définir les secrets GitHub

Cette étape consiste à configurer l'environnement du projet sur GitHub pour que les futurs pipelines d'automatisation (CI/CD via GitHub Actions) puissent fonctionner correctement et en toute sécurité.

Variables : Pour les paramètres de configuration qui ne sont pas secrets
<img width="1242" height="1042" alt="image" src="https://github.com/user-attachments/assets/150d71b5-f7c9-4676-8779-e0601b98d869" />
Secrets : Pour les données sensibles (mots de passe, clés API)
<img width="1221" height="821" alt="image" src="https://github.com/user-attachments/assets/2a465481-1e9a-4089-95cd-1396197a6399" />
## Étape 3 : Créer le workflow CI/CD
Cette étape consiste en la rédaction du manifeste d'automatisation via un fichier de configuration YAML pour GitHub Actions. Le processus définit un scénario d'exécution systématique : à chaque soumission de code sur le dépôt, un serveur virtuel est instancié pour préparer l'environnement de travail. La configuration inclut l'installation de la version de Python spécifiée par la variable PY_VERSION ainsi que la mise en place d'un mécanisme de mise en cache des dépendances afin d'optimiser les temps d'exécution. Ce dispositif constitue le socle de l'intégration continue (CI).
En résumé, nous avons créé le "cerveau" de notre intégration continue (CI) pour que notre pipeline DVC s'exécute tout seul
<img width="2291" height="2053" alt="image" src="https://github.com/user-attachments/assets/e555ee58-9412-40fb-bf1e-c1a8633e402f" />

## Étape 4 : Commit et push

Cette étape valide l'automatisation complète du cycle de vie du modèle via GitHub Actions, confirmant le succès des jobs d'intégration (CI) et de déploiement (CD). L'exécution automatique du pipeline en une minute démontre une gestion efficace des dépendances et du cache, garantissant la reproductibilité du code. 
<img width="1899" height="1530" alt="image" src="https://github.com/user-attachments/assets/d6a7387d-4805-451c-ae15-dccc7fcfcafa" />

La génération d'artefacts en fin de processus assure la persistance et la disponibilité des modèles et des métriques de performance pour la production

<img width="1883" height="1194" alt="image" src="https://github.com/user-attachments/assets/4dc43a11-6958-4040-9e8f-a36edf0a7a4f" />
### Conclusion 
Ce lab a permis de mettre en place une infrastructure MLOps complète, intégrant le versionnement des données avec DVC et l automatisation de cycles de vie via GitHub Actions. La création d'un pipeline reproductible et la configuration de workflows CI/CD garantissent désormais que chaque modification du code ou des données est automatiquement testée, validée et prête au déploiement. 

# Lab 5 : Du Notebook au Déploiement Conteneurisé d’un Modèle de Machine Learning

## Étape 1 : Vérifier l’installation de Docker

<img width="1780" height="246" alt="image" src="https://github.com/user-attachments/assets/a58acb3e-76e2-4090-ad57-e90d0ae5fe9d" />

## Étape 2 : Lancer un serveur Nginx dans un conteneur
Onlancé un serveur Nginx en mode détaché (-d) sur le port local 8080
La commande docker ps montre que le conteneur nommé "demo-nginx" est actif  et qu'il redirige correctement le trafic du port 8080 vers le port 80 du conteneur.
puis on a correctement terminé le test en arrêtant (stop) puis en supprimant (rm) le conteneur.


<img width="1903" height="521" alt="image" src="https://github.com/user-attachments/assets/6d99219a-40ed-4fe8-8d5b-71f1f32a7704" />
<img width="1892" height="731" alt="image" src="https://github.com/user-attachments/assets/5926a845-b501-4a3d-93e0-ff2a00cfa892" />

## Étape 3 : Ouvrir un shell Linux isolé dans un conteneur

l'isolation en lançant un système Ubuntu complet à l'intérieur d'un conteneur interactif sans affecter notre machine hôte
<img width="1882" height="1877" alt="image" src="https://github.com/user-attachments/assets/ae8fc4ff-795b-4e48-8792-ed84157d852c" />

<img width="1882" height="1916" alt="image" src="https://github.com/user-attachments/assets/d558b932-785b-4b6e-b20a-67e1177fc241" />

 Vérifions que le conteneur existe toujours mais est arrêté :

<img width="1879" height="1648" alt="image" src="https://github.com/user-attachments/assets/0352ac0b-897c-44f9-9a47-796adadd1187" />

On Supprime ce conteneur

<img width="1923" height="648" alt="image" src="https://github.com/user-attachments/assets/e63c6382-4bad-4b7e-b18b-d0d7a0977199" />

## Étape 4 : Comprendre la structure d’une commande docker run
On a testé le cycle de vie complet d'un conteneur Docker en utilisant l'image Nginx
Le terminal affiche ule Container ID . Cela confirme que le conteneur est opérationnel
on a lancé l'arrêt au conteneur.il  existe toujours surle disque, mais il ne consomme plus de processeur ni de mémoire vive puis on a effacé définitivement l'instance du conteneur.
<img width="1908" height="428" alt="image" src="https://github.com/user-attachments/assets/b10d83df-2e49-4c92-b59c-081952fe28da" />


## Étape 5 : Conteneuriser l’API churn du projet mlops-lab-01

Exécution de la commande d'activation du venv : .\venv_mlops\Scripts\activate.
(venv_mlops) apparaît dans le terminal, confirmant que les dépendances spécifiques au projet (FastAPI, Uvicorn, Scikit-Learn, etc.) sont isolées et prêtes à être utilisées.

## Étape 6 : Créer un fichier requirements.txt pour l’image Docker
on a préparé la liste des dépendances nécessaires au fonctionnement
<img width="1547" height="1233" alt="image" src="https://github.com/user-attachments/assets/1a434f45-c040-4474-b9a1-d1c5a065379e" />

## Étape 7 : Créer un Dockerfile pour l’API churn
On a créé le Dockerfile, un fichier de configuration qui définit chaque couche de l'environnement nécessaire
<img width="1998" height="1431" alt="image" src="https://github.com/user-attachments/assets/7a03b757-c644-4650-9455-abad19a55f16" />

## Étape 8 :Préparer un modèle actif avant de construire l’image
On a effectué une vérification pour s'assurer que l'image Docker contiendra un modèle valide et prêt pour la production.
<img width="1863" height="1947" alt="image" src="https://github.com/user-attachments/assets/9532d1fa-1c8d-4347-a882-4bd98877f94b" />

## Étape 9 : Construire l’image Docker du projet churn

<img width="1895" height="1904" alt="image" src="https://github.com/user-attachments/assets/86436cf2-7c1c-4ad0-b7c4-680b4eaccb4a" />

## Étape 10 : Lancer l’API churn dans un conteneur
Cette étape  confirme que l'ensemble de la chaîne (données, modèle, code et infrastructure) fonctionne parfaitement.
### Test de santé (/health)
Le système renvoie un StatusCode : 200 OK. Le corps de la réponse JSON indique que le modèle actif
churn_model_v1_20260112_081303.joblib). Cela prouve que le conteneur a correctement intégré le registre de modèles.
### Test de prédiction (/predict)
On a soumis une requête POST avec des données d'entrée et L'API a répondu avec succès 
<img width="1882" height="348" alt="image" src="https://github.com/user-attachments/assets/3f800a0b-708f-4f03-9422-a587bd25a3b2" />

<img width="1872" height="1046" alt="image" src="https://github.com/user-attachments/assets/988edc95-39c0-4b57-9d94-40607437c282" />

<img width="1759" height="1800" alt="image" src="https://github.com/user-attachments/assets/8e51debb-c098-405e-8928-0ab4d6156867" />

## Étape 11 : Vérifier les logs générés à l’intérieur du conteneur


<img width="1854" height="1456" alt="image" src="https://github.com/user-attachments/assets/43fbca99-d2fa-4f5f-96c2-8ffe30cf601b" />

## Étape 12 : Orchestration locale avec Docker Compose
On a créé le fichier docker-compose.yml, qui agit comme un "manuel d'instructions" permanent pour Docker. Il centralise tous les paramètres (ports, noms, variables d'environnement) en un seul endroit

<img width="2056" height="1823" alt="image" src="https://github.com/user-attachments/assets/73627db5-9736-4691-b2cd-a5da41fd4748" />

## Étape 13 : Démarrer l’API via Docker Compose
On a utilisé la commande docker compose up pour lancer l'ensemble des services 

Démarrage du service : Le terminal montre que le conteneur churn-api-compose a été créé et que le serveur Uvicorn a démarré avec succès.

Test de santé (/health) : Une requête curl confirme que l'API est opérationnelle  et utilise le modèle actif churn_model_v1_20260112_083455.joblib.

Test de prédiction (/predict) : On a envoyé une requête POST simulant un profil client. L'API a renvoyé une prédiction de churn de 0 (pas de churn) avec une probabilité de 0.25 .
<img width="1845" height="831" alt="image" src="https://github.com/user-attachments/assets/514b5429-41dc-4b32-8c66-e49644a87c43" />

<img width="1861" height="1187" alt="image" src="https://github.com/user-attachments/assets/590afe12-fa72-4fe3-9f4e-fd9e50448d42" />


<img width="1912" height="1691" alt="image" src="https://github.com/user-attachments/assets/c11c3e16-6a56-40ba-bc53-cac08b5038f3" />

Étape 14 : lancer les services en arrière-plan et observer les logs
Cette étape a consisté à simuler un environnement de production en lançant le service en arrière-plan avec docker compose up -d. On a utilisé docker compose logs -f pour surveiller en temps réel l'activité de l'API sans bloquer le terminal.
Les tests finaux ont confirmé que les requêtes /health et /predict sont traitées avec succès, avec une latence de 4.5 ms et une probabilité de churn précise. 
Enfin, la commande docker compose down a permis de nettoyer proprement l'infrastructure.

<img width="1927" height="2033" alt="image" src="https://github.com/user-attachments/assets/662fb26d-399f-4933-96cd-a0aff1d751f5" />

Étape 15 : lier Docker Compose au reste du cours (Git + DVC)
Versionnement du déploiement : On a utilisé git add pour inclure les fichiers de configuration de l'infrastructure (Dockerfile, docker-compose.yml, requirements.txt) dans le suivi Git. Le commit final, "feat: ajout conteneurisation Docker de l'API churn", verrouille la capacité de déploiement dans l'historique du projet.
<img width="1925" height="894" alt="image" src="https://github.com/user-attachments/assets/48b3ee46-9ca7-4b76-a8bb-e875880f57b0" />

## Conclusion

Le Lab 5 a permis d'unifier Git, DVC et Docker pour créer une infrastructure MLOps complète, portable et totalement auditable. En encapsulant l'API et son modèle performant  dans un conteneur orchestré par Docker Compose, on garantit un déploiement fiable et reproductible sur n'importe quel serveur. Cette étape assure une traçabilité totale, liant chaque version du code à une version spécifique des données et du modèle grâce aux logs persistants.



