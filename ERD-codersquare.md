Ce document explore la conception de codersquare, une expérience sociale permettant de partager des ressources de programmation utiles.
Nous utiliserons une architecture client/serveur de base, dans laquelle un seul serveur est déployé sur un fournisseur de cloud à côté d'une base de données relationnelle, et dessert le trafic HTTP à partir d'un point de terminaison public.
# Storage
Nous utiliserons une base de données relationnelle (le schéma suit) pour récupérer rapidement les publications et les commentaires. Une implémentation minimale de base de données telle que sqlite3 suffit, même si nous pouvons potentiellement passer à quelque chose avec un peu plus de puissance comme Postgresql si nécessaire. Les données seront stockées sur le serveur sur un volume distinct et sauvegardé pour des raisons de résilience. Il n’y aura pas de réplication ni de partage des données à ce stade précoce.
Nous avons exclu les services de stockage en tant que service tels que Firestore et autres afin de présenter la création d'un backend autonome à des fins éducatives.
# Schéma
Nous aurons besoin d'au moins les entités suivantes pour implémenter le service :
**users**:
| Column | Type |
|--------|------|
|ID |STRING/UUID |
|First/Last name | STRING |
|Password | STRING |
| Email (optional) | STRING |
**Postsex**:
| Column |Type |
|--------|------|
| ID |STRING/UUID | 
|Title |STRING |
|URL |STRING |
|UserId | STRING/UUID |
| PostedAt |Timestamp |
**likes**:
| Column | Type |
|--------|------|
|userid|string/uuid|
|postid|string|
**comments**:
| Column | Type |
|--------|------|
|userid|string/uuid|
|postid|string|
|comment|string|
|postedat|timestamp|
**serveur**:
un simple serveur HTTP est responsable de l'authentification, de la diffusion des données stockées et potentiellement de l'ingestion et de la diffusion des données analytiques.
• Node.js est sélectionné pour implémenter le serveur en raison de la rapidité de développement.
• Express.js est le framework de serveur Web.
• Sequelize à utiliser comme ORM.
Authentification
Pour la v1, un simple mécanisme d'authentification basé sur JWT doit être utilisé, avec des mots de passe cryptés et stockés dans la base de données. OAuth doit être ajouté initialement ou ultérieurement pour Google + Facebook et peut-être d'autres (Github ?).
API
Auth:
/signIn [POST]
/signUp [POST]
/signOut [POST]
**Posts**:
/posts/list [GET]
/posts/new [POST]
/posts/:id [GET]
/posts/:id [DELETE]
Likes:
/likes/new [POST]
Comments:
/comments/new [POST]
/comments/list [GET]
/comments/:id [DELETE]
**Clients**:
Pour l'instant, nous allons commencer avec un seul client Web, en ajoutant éventuellement des clients mobiles ultérieurement.
Le client Web sera implémenté dans React.js.
**Hosting**:
Le code sera hébergé sur Github, les relations publiques et les problèmes seront les bienvenus.
Le client Web sera hébergé à l'aide de n'importe quelle plate-forme d'hébergement Web gratuite telle que Firebase ou Netlify. Un domaine sera acheté pour le site et configuré pour pointer vers l'adresse IP publique du serveur de l'hébergeur.
Nous déploierons le serveur sur un VPS (probablement partagé) pour plus de flexibilité. La VM aura des ports HTTP/HTTPS ouverts, et nous commencerons par un déploiement manuel, qui sera automatisé ultérieurement à l'aide d'actions Github ou similaires. Le serveur aura une politique CORS fermée, à l'exception du nom de domaine et du serveur hôte Web.


