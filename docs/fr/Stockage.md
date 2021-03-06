# Stockage

La plateforme propose diff&eacute;rents types de stockage, con&ccedil;us 
pour diff&eacute;rents types de cas d'utilisation. Par cons&eacute;quent, 
cette section vous concerne, que vous soyez en train d'exp&eacute;rimenter, 
de cr&eacute;er des pipelines, ou d'&eacute;diter.  

En surface, il existe deux types de stockage :

- des disques (aussi appel&eacute;s volumes)

- des compartiments (stockage S3 ou "blob")


## Disques
Les disques sont les syst&egrave;mes de fichiers courants de type disque dur ou SSD. 
Vous pouvez monter les disques dans votre serveur Kubeflow, et m&ecirc;me si vous 
supprimez votre serveur, vous pouvez remonter les disques, car ils ne sont jamais 
d&eacute;truits par d&eacute;faut. C'est un moyen tr&egrave;s simple de stocker vos donn&eacute;es, 
et si vous partagez un espace de travail avec une &eacute;quipe, tous les membres peuvent 
utiliser le disque du m&ecirc;me serveur (comme un lecteur partag&eacute;).

![Ajout d’un volume existant à un nouveau serveur de bloc-notes](images/kubeflow_existing_volume.png)
C’est un moyen très simple de stocker vos données. Si vous partagez un espace de travail avec
une équipe, tous les membres peuvent utiliser le disque du même serveur comme un lecteur partagé.


## Compartiments


![Compartiments/Stockage d'objets](images/minio_self_serve_bucket.png)

Les compartiments sont un peu plus compliqu&eacute;s, mais ils pr&eacute;sentent trois avantages :

- Le stockage de grandes quantit&eacute;s de donn&eacute;es
  - Les compartiments peuvent &ecirc;tre &eacute;normes (bien plus grands que les disques durs), 
    et ils sont rapides.
  
- Le partage de donn&eacute;es
  - Vous pouvez partager des fichiers &agrave; partir d'un compartiment en partageant 
    une URL que vouspouvez obtenir par l'interm&eacute;diaire d'une interface Web simple. 
    C'est une excellente fa&ccedil;on de partager des donn&eacute;es avec des personnes &agrave; 
    l'ext&eacute;rieur de votre espace de travail.
    
- L'acc&egrave;s &agrave; la programmation
  - Plus important encore, il est beaucoup plus facile pour les pipelines et 
    les navigateurs Web d'acc&eacute;der aux donn&eacute;es provenant de compartiments que 
    d'un disque dur. Donc, si vous voulez utiliser des pipelines, il faut d'abord 
    les configurer pour qu'ils fonctionnent avec un compartiment.
    

# Stockage en compartiment

Nous avons quatre types de stockage en compartiment.

**Libre-service**

- [Minimal](https://minimal-tenant1-minio.covid.cloud.statcan.ca)

- [Premium](https://premium-tenant1-minio.covid.cloud.statcan.ca)

- [Pachyderm](https://pachyderm-tenant1-minio.covid.cloud.statcan.ca)

**Accessible au grand public**

- [Public (en lecture seule)](https://datasets.covid.cloud.statcan.ca)


## Libre-service

Dans chacune des trois options de libre-service, vous pouvez créer un compartiment personnel. Pour ouvrir
une session, il vous d’utiliser **OpenID**, comme indiqué ci-dessous.

![Compartiments / Stockage d’objets](images/minio_self_serve_login.png)	![Vue d’ouverture de session Minio, indiquant l’option OpenID](images/minio_self_serve_login.png)

Une fois que vous &ecirc;tes connect&eacute;, vous pouvez cr&eacute;er un compartiment personnel 
selon le format `prenom-nom`. 


![Compartiments/Stockage d’objets](images/minio_self_serve_bucket.png)	![Navigateur Minio avec compartiment personnel utilisant le format prénom, nom de famille (avec trait d’union)](images/minio_self_serve_bucket.png)

## Partage
Vous pouvez facilement partager des fichiers individuels. Utilisez simplement l’option "partager" pour
un fichier particulier, et vous recevrez un lien que vous pourrez envoyer à un
![Partage de fichiers Minio](images/minio_self_serve_share.png)	collaborateur.


![Navigateur Minio avec lien partageable vers un fichier](images/minio_self_serve_share.png)


## Acc&egrave;s &agrave; la programmation

Nous travaillons actuellement à un moyen de vous permettre d’accéder à votre stockage de compartiment par l’intermédiaire d’un dossier
dans votre bloc-notes, mais en attendant, vous pouvez y accéder par programme en utilisant
l’outil de ligne de commande `mc`, ou par l’intermédiaire des appels d’API S3 dans R ou Python.

<!-- prettier-ignore -->
!!! danger "Configuration Kuberflow requise"
    Si vous souhaitez activer le stockage en compartiment pour votre bloc-notes, sélectionnez "Injecter les justificatifs d'identité pour accéder au stockage d'objets MinIO" à partir du menu **Configurations** lorsque vous créez votre serveur. Sinon, votre serveur ne saura pas comment se connecter à votre stockage personnel.
    ![Création d’un serveur de bloc-notes Kubeflow avec l’option "Injecter les justificatifs d'identité pour accéder au stockage d'objets MinIO" sélectionnée](images/kubeflow_minio_option.png)

<!-- prettier-ignore -->
!!! tip "Voir les exemples de blocs-notes!"
    Un modèle est fourni pour se connecter dans `R`, `python`, ou par
    la ligne de commande fournie dans `jupyter-notebooks/self-serve-storage`. Vous pouvez
    copier-coller et modifier ces exemples. Ils devraient répondre à la plupart de vos besoins.

### Connexion à l’aide de `mc`

Pour vous connecter, exécutez la commande suivante (remplacer `NOMCOMPLET=blair-drummond` par
votre `nom-prénom` réel) :

```sh
#!/bin/sh
NOMCOMPLET=blair-drummond
# Obtenir les justificatifs d’identité
source /vault/secrets/minio-minimal-tenant1
# Ajouter le stockage sous le pseudonyme "minio-minimal"
mc config host add minio-minimal $MINIO_URL $MINIO_ACCESS_KEY $MINIO_SECRET_KEY
# Créer un compartiment à votre nom
# NOTE : Vous pouvez *uniquement* créer des compartiments nommés avec votre PRÉNOM-NOM. 
# Tout autre nom sera rejeté.
# Compartiment privé ("mb" = "créer compartiment")
mc mb minio-minimal/${NOMCOMPLET}
# Compartiment partagé
mc mb minio-minimal/shared/${NOMCOMPLET}
# Voilà! Vous pouvez maintenant copier des fichiers ou des dossiers!
[ -f test.txt ] || echo "Ceci est un test" > test.txt
mc cp test.txt minio-minimal/${NOMCOMPLET}/test.txt
```

Maintenant, ouvrez le document
[minimal-tenant1-minio.example.ca](https://minimal-tenant1-minio.example.ca),
Vous y verrez votre fichier de test.

Vous pouvez utiliser `mc` pour copier des fichiers vers/depuis le compartiment. Cette opération est très rapide. Vous pouvez également
utiliser `mc --help` pour voir les autres options qui s’offrent à vous, comme
`mc ls minio-minimal/PRÉNOM-NOM/` pour afficher le contenu de votre compartiment.

<!-- prettier-ignore -->
??? tip "Autres options de stockage"
    Pour utiliser une de nos autres options de stockage, `pachyderm` ou `premium`, 
    remplacez simplement  la valeur `minimal` dans le programme ci-dessus par le type dont vous avez besoin.

