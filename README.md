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
<img width="1887" height="258" alt="image" src="https://github.com/user-attachments/assets/59cec189-485c-49de-9771-6ebaadf98919" />

n observe dans l'arborescence de fichiers que le dossier dvc_storage se remplit de sous-dossiers (nommés 3d, 05, 7e, etc.). Ce sont des fichiers de hash
<img width="1267" height="611" alt="image" src="https://github.com/user-attachments/assets/f0fc4ca2-9a22-4f40-8dd5-ef1c25cf3654" />
