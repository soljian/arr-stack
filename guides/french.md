# Préambule

Tous les chemins spécifiés ici peuvent être modifiés à votre convenance. J'utiliserais ceux par défaut indiqués dans le fichier `compose.yml`. Veuillez en prendre connaissance et changer les chemins si nécessaire.

Téléchargez Docker pour le système sur lequel vous allez installer la solution en suivant leur guide https://docs.docker.com/engine/install/

NB : les clés d'API, secrets et autres qui sont montrés sur les captures ont été créées pour l'occasion et ne seront pas réutilisées.

NB2 : les liens que je donne sont basés sur localhost, évidemment si votre stack est hébergé sur un autre périphérique de votre réseau, remplacez par son IP.

# 0. Téléchargez et démarrer la stack

```
git clone https://github.com/soljian/arr-stack.git
cd arr-stack
docker compose up -d
```

# 1. Client VPN et killswitch

1. Modifier la variable d'environnement `WIREGUARD_PRIVATE_KEY` avec la clé privée de votre profil wireguard. Vous pouvez en trouver une sur Proton VPN. L'utilisation d'un abonnement premium est obligatoire lors de l'utilisation afin de pouvoir seed vos torrents aux autres utilisateurs. Le VPN sert a éviter d'exposer votre adresse IP (Hadopi). Si ce n'est pas un sujet pour vous, commentez/supprimez le service Gluetun et deluge-port-sync sur `compose.yml` puis supprimez le bloc `depends_on` du service `deluge`. Le killswitch inclus sur Gluetun permet d'empêcher Deluge de fonctionner dès lors que la connexion au VPN est impossible.

![alt text]({319A389A-B9D2-4B0E-BA0C-7F92523B9C19}.png)

# 2. Client torrent

1. Se rendre sur http://localhost:8112 (mot de passe par défaut : `deluge`)

![alt text]({E3D26C68-0280-4387-AA4E-9BEA6CF5F166}.png)

2. Ouvrir `Preferences` et configurer les chemins de téléchargement

![alt text]({1C6D083A-2F66-4E63-B02D-9E9963C0C734}.png)

4. Activer le plugin/module `Label`

![alt text]({8B82DEDF-8D23-4C7B-8350-CDCA2F754BCA}.png)

5. Modifier le mot de passe par défaut (rappel : `deluge`)

![alt text]({BBADD7F8-49FB-4C05-BC9D-EC1366FACF29}.png)

# 3. Prowlarr

1. Se rendre sur http://localhost:9696, activez l'authentification et créez votre compte

![alt text]({56F5DC6C-6A82-4251-B3C9-A5FF8B41D3ED}.png)

2. Se rendre sur `Indexers` puis `Add indexer`

![alt text]({C9CE9312-4A74-46BA-81A0-FBF63A704918}.png)

3. Choisissez votre indexer afin de sourcer vos fichiers torrent. Si vous avez un indexer privé tel que C411 ou consort, utilisez le et saisissez vos infos de connexion (Selon l'indexer la marche à suivre peut être légèrement différente, ça devrait être précisé sur la même page). Vous pouvez utiliser le bouton `Test` pour vérifier que tout est en ordre.

![alt text]({B54863CE-78A9-47DC-AB81-39359A2B651D}.png)

# 4. Sonarr

1. Se rendre sur http://localhost:8989 et configurez l'authentification de la même manière que pour Prowlarr.

2. Se rendre dans `Settings` puis `General`. Désactivez l'envoie des données anonymes et récupérez votre clé API.

![alt text]({C380C33C-3949-4E02-A028-B196A9EF9E28}.png)

3. Sur Prowlarr, se rendre sur `Settings` puis `Apps`. Cliquez sur le bouton pour ajouter une application, sélectionnez `Sonarr`. Remplissez le formulaire comme sur la capture.

```
Prowlarr server: http://gluetun:9696
Sonarr server: http://sonarr:8989
API Key : votre clé API Sonarr
```

![alt text]({5235D180-1243-42C1-BCB7-858317D6D19F}.png)

4. Sur Sonarr, sur la page `Settings` puis `Indexers`, vous devriez maintenant voir votre indexer configuré sur Prowlarr. Si ce n'est pas le cas, vérifiez les étapes précédentes.

![alt text]({197BCD52-9D51-47F5-A8BA-5DED339AA484}.png)

5. Se rendre sur `Settings` puis `Media Management`, puis configurez selon votre souhait. Je vous suggère la configuration suivante. Attention : n'oubliez pas la section `Root folders`, précisez bien le dossier où seront enregistrées vos séries. Si le dossier n'existe pas encore, créez le.

![alt text]({3817F1E4-6724-4012-8D11-905FF21569B6}.png)

6. Se rendre sur `Settings` puis `Profiles`, puis supprimez tous les profils prédéfinis.

![alt text]({4C9648E7-5D85-418D-8D03-7706104F3119}.png)

7. Créez votre propre profil selon vos souhaits. Pour ma part, je vais préférer le 1080p avec cette configuration. Vous pouvez réordonner les qualités. L'outil téléchargera la meilleure qualité disponible selon vos critères et si un nouveau torrent est disponible dans une qualité supérieure, alors il l'a récupérera automatiquement jusqu'à la limite que vous avez définie. Veillez à sélectionner votre langue, ici French.

![alt text]({03BDC689-F880-4D71-9402-24FB4BABDBAE}.png)

8. Se rendre sur `Settings` puis `Download Clients`, cliquez sur le bouton pour ajouter un client puis sélectionnez `Deluge`. Configurez le ainsi puis utilisez le bouton `Test` pour vérifier.

![alt text]({2C61891E-C3B1-4159-8B23-3A97C7A85DAC}.png)

# 5. Radarr

_Cette étape va être très proche de Sonarr, je ne vais donc ajouter de capture d'écran que lorsque nécessaire._

1. Se rendre sur http://localhost:7878 et configurez l'authentification de la même manière que pour Prowlarr.

2. Se rendre dans `Settings` puis `General`. Désactivez l'envoie des données anonymes et récupérez votre clé API.

3. Sur Prowlarr, se rendre sur `Settings` puis `Apps`. Cliquez sur le bouton pour ajouter une application, sélectionnez `Radarr`. Remplissez le formulaire comme sur la capture.

```
Prowlarr server: http://gluetun:9696
Radarr server: http://radarr:7878
API Key : votre clé API Radarr
```

4. Sur Radarr, sur la page `Settings` puis `Indexers`, vous devriez maintenant voir votre indexer configuré sur Prowlarr. Si ce n'est pas le cas, vérifiez les étapes précédentes.

5. Se rendre sur `Settings` puis `Media Management`, puis configurez selon votre souhait. Je vous suggère la configuration suivante. Attention : n'oubliez pas la section `Root folders`, précisez bien le dossier où seront enregistrées vos séries. Si le dossier n'existe pas encore, créez le.

![alt text]({A78B1ABC-5313-42FB-B96A-FD3C1FAA5D96}.png)

6. Se rendre sur `Settings` puis `Profiles`, puis supprimez tous les profils prédéfinis.

7. Créez votre propre profil selon vos souhaits. Pour ma part, je vais préférer le 1080p avec cette configuration. Vous pouvez réordonner les qualités. L'outil téléchargera la meilleure qualité disponible selon vos critères et si un nouveau torrent est disponible dans une qualité supérieure, alors il l'a récupérera automatiquement jusqu'à la limite que vous avez définie.

8. Se rendre sur `Settings` puis `Download Clients`, cliquez sur le bouton pour ajouter un client puis sélectionnez `Deluge`. Configurez le ainsi puis utilisez le bouton `Test` pour vérifier.

# 6. Lidarr

_Cette étape va être très proche de Sonarr, je ne vais donc ajouter de capture d'écran que lorsque nécessaire._

1. Se rendre sur http://localhost:8686 et configurez l'authentification de la même manière que pour Prowlarr.

2. Se rendre dans `Settings` puis `General`. Désactivez l'envoie des données anonymes et récupérez votre clé API.

3. Sur Prowlarr, se rendre sur `Settings` puis `Apps`. Cliquez sur le bouton pour ajouter une application, sélectionnez `Lidarr`. Remplissez le formulaire comme sur la capture.

```
Prowlarr server: http://gluetun:9696
Lidarr server: http://lidarr:8686
API Key : votre clé API Lidarr
```

4. Sur Radarr, sur la page `Settings` puis `Indexers`, vous devriez maintenant voir votre indexer configuré sur Prowlarr. Si ce n'est pas le cas, vérifiez les étapes précédentes.

5. Se rendre sur `Settings` puis `Media Management`, puis configurez selon votre souhait. Je vous suggère la configuration suivante. Attention : n'oubliez pas la section `Root folders`, précisez bien le dossier où seront enregistrées vos séries. Si le dossier n'existe pas encore, créez le.

![alt text]({0CBA5A33-2248-4356-A766-366125BD86C6}.png)

6. Se rendre sur `Settings` puis `Profiles`, puis supprimez tous les profils prédéfinis.

7. Créez votre propre profil selon vos souhaits. Pour ma part, je vais préférer le 1080p avec cette configuration. Vous pouvez réordonner les qualités. L'outil téléchargera la meilleure qualité disponible selon vos critères et si un nouveau torrent est disponible dans une qualité supérieure, alors il l'a récupérera automatiquement jusqu'à la limite que vous avez définie.

8. Se rendre sur `Settings` puis `Download Clients`, cliquez sur le bouton pour ajouter un client puis sélectionnez `Deluge`. Configurez le ainsi puis utilisez le bouton `Test` pour vérifier.

# 7. Plex

1. Se rendre sur http://localhost:32400/web, connectez vous (ou créer un compte si besoin) puis suivez le guide d'installation.

2. Configurez vos bibliothèques comme suit :

![alt text]({EE3B8CF0-A87B-45CB-83C9-F2DE0D3B26E6}.png)

![alt text]({E4B70E4F-8E51-43A2-91E4-4F744351F6DD}.png)

_On reviendra sur Plex plus tard._

# 8. Seerr

1. Se rendre sur http://localhost:5055/setup puis suivez le guide.

2. Sélectionnez `Plex` > `Configure Plex` > Login with Plex.

3. Cliquez sur le bouton `refresh 🔄️` à droite du champ server, puis sélectionner votre serveur ajouté précédemment.

4. Save changes

![alt text]({902D48A2-C354-493D-AD08-41844D6C4835}.png)

5. Cochez les bibliothèques que vous souhaitez gérer avec Seerr (films et séries)

![alt text]({73291A00-873C-4800-BBAA-427189CB8CC1}.png)

6. Start Scan, puis Continue

7. Pour l'écran suivant, vous devrez configurer Radarr et Sonarr. Beaucoup des configurations à saisir ont déjà été saisies sur la configuration avec Prowlarr, n'hésitez pas à y retourner au besoin.

**Radarr :**

![alt text]({B1413785-F2D1-4A46-A0B3-8AC439756096}.png)

**Sonarr :**

![alt text]({2A49FE57-FA18-4AA0-B184-BD46B0AAA4AB}.png)

8. Finish setup

**A ce stade, vous pouvez commencer à utiliser votre stack. Parcourez Seerr pour trouver ce que vous voulez regarder, faites une demande et observez Seerr passer l'ordre à Sonarr ou Radarr qui fera automatiquement le téléchargement pour vous**

Par exemple, je choisi de télécharger The Matrix

![alt text]({DD6C9338-B161-44B7-9ED5-A86B0EB3A503}.png)

Je le retrouve dans Radarr

![alt text]({FA70C4C9-8C30-42CB-8FEC-9E4AF332A68C}.png)

Deluge télécharge bien le fichier

![alt text]({6A4AFB53-C854-4923-9926-8A327AF37219}.png)

Puis il le déplace et renomme correctement

![alt text]({7EE3B2D1-20EA-4E45-A7E5-EFD3AE9E75A1}.png)

Puis je le retrouve bien dans Plex

![alt text]({D6CEDD62-CA70-4ACD-9BAE-6E1FC27E2A94}.png)

Et on profite

![alt text]({65D96334-5FA3-42C2-85F7-E7762077ED3F}.png)

# Rappels

## Torrents

Si vous utilisez un fournisseur de torrent avec Ratio, vous devrez conserver vos torrent en seed afin de faire augmenter votre ratio et rendre service à la communauté. Il est donc vivement conseillé de garder tout vos torrents actifs le plus de temps possible et donc de laisser tourner votre serveur tout le temps. Plus vous aurez de torrent actifs plus vous gagnerez de ratio.

Télécharger du contenu que vous ne possédez pas est illégal. Le faire vous expose à des risques, c'est pourquoi l'utilisation d'un VPN est plus qu'encouragée. Privilégiez un service qui propose du port forwarding sinon vous ne pourrez que très peu seed.

## Hardware

Le mieux est d'utiliser un NAS pour installer cette solution, mais un Raspberry PI peut très bien faire le taf. J'ai moi même utilisé un Raspberry PI 4 puis un 5 pendant de longues années avant de passer récemment sur un NAS Ugreen.

Il est tout à fait possible de ne faire tourner que Plex sur le NAS et le reste sur un autre périphérique de votre réseau. Vous devrez configurer les points de montages en conséquences, mais si vous en êtes a ce stade je suppose que vous n'avez pas besoin de guide.
