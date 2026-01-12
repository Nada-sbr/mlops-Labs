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

### Conclusion

Ce lab a permis de démontrer l'importance de DVC (Data Version Control) comme pilier d'un projet MLOps moderne. En intégrant DVC à notre flux de travail Git, nous avons résolu les défis majeurs liés à la gestion des projets de science des données.

Les points clés retenus sont :

Gestion optimisée des données : Nous avons appris à séparer le code (géré par Git) des données volumineuses (gérées par DVC), garantissant un dépôt léger et performant.

Sécurité et Collaboration : La mise en place d'un "remote storage" et l'utilisation des commandes push et pull assurent que les datasets sont sauvegardés et accessibles à tous les collaborateurs de manière identique.

Reproductibilité Totale : Grâce à la définition d'un pipeline dans dvc.yaml et au verrouillage des versions dans dvc.lock, nous pouvons désormais garantir que n'importe quelle étape (préparation, entraînement ou évaluation) peut être reproduite avec les mêmes résultats.

Traçabilité : Le lien permanent entre une version du code et une version spécifique des données permet un audit complet du modèle, ce qui est essentiel pour le déploiement en production.


###
