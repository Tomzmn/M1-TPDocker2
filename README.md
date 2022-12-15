# TP 2 - Docker

[📖 | Voir la documentation sur cette plateforme.](https://stack.tomz.fr/books/conteneurisation/page/tp-2-docker)

#### Mise en pratique - Docker Application Express JS

##### 1. Récupérer le zip TP-2-Docker.zip sur Moodle.

Initialiser un nouveau repository git qui vous permettra de sauvegarder les fichiers créés pendant le TP. Vous enverrez un zip du repository à la fin du TP avec vos réponses aux questions / exécutions et résultats sur la console dans un **fichier Markdown**   
<span style="text-decoration: underline;">Utilisez git progressivement ! (Ne pas faire qu’un seul commit à la fin) </span>

##### 2. Compléter le Dockerfile afin de builder correctement l’application contenu dans src/

- Une option de npm vous permet de n’installer que ce qui est nécessaire. Quelle est cette option ? Quelle bonne pratique Docker permet t-elle de respecter ?

<p class="callout info">Pour n'installer que les modules nécessaires à l'application, vous pouvez utiliser l'option `--production` de npm lors de l'installation des modules.</p>

<p class="callout info">Voici comment cela pourrait être intégré dans le Dockerfile :</p>

```bash
FROM node:12-alpine3.9

# Copier les fichiers de l'application dans le conteneur
COPY . .

# Installer les modules nécessaires à l'application
RUN npm install --production

# Lancer l'application
CMD ["node", "src/index.js"]
```

<p class="callout info">En utilisant cette option, seuls les modules listés dans le fichier `package.json` dans la section `dependencies` seront installés, ce qui permet de respecter la bonne pratique Docker consistant à inclure uniquement les éléments nécessaires pour exécuter l'application dans le conteneur. Cela permet de limiter la taille du conteneur et d'améliorer les performances de l'application.</p>

##### 3. A l’aide de la commande docker build, créer l’image ma\_super\_app

<p class="callout info">Pour créer l'image de l'application avec la commande `docker build`, vous devez spécifier le chemin du fichier Dockerfile à utiliser pour construire l'image en utilisant l'option `-f` suivie du chemin du fichier. Vous pouvez également spécifier un nom pour l'image en utilisant l'option `-t` suivie du nom souhaité.</p>

<p class="callout info">Voici comment cela pourrait être utilisé pour créer une image nommée `ma_super_app` à partir d'un fichier Dockerfile situé dans le répertoire courant :</p>

```bash
docker build -f Dockerfile -t ma_super_app .
```

<p class="callout info">L'image sera construite en exécutant les commandes spécifiées dans le fichier Dockerfile, en commençant par la première (`FROM`) et en continuant jusqu'à la dernière (`CMD` dans notre exemple). Une fois l'image construite, vous pouvez l'utiliser pour démarrer un conteneur en utilisant la commande `docker run`. Par exemple :</p>

```bash
docker run ma_super_app
```

##### 4. Compléter le fichier docker-compose.yml afin d’éxécuter ma\_super\_app avec sa base de données.

<p class="callout info">Pour exécuter une application avec sa base de données en utilisant Docker Compose, vous devez définir deux services dans le fichier `docker-compose.yml` : un service pour l'application et un service pour la base de données. Chaque service peut être configuré en spécifiant les options suivantes :</p>

- `image` : l'image Docker à utiliser pour le service.
- `ports` : les ports sur lesquels le service sera exposé.
- `environment` : les variables d'environnement à définir pour le service.
- `links` : les autres services auxquels le service doit être lié.
- `depends_on` : les autres services sur lesquels le service dépend.

<p class="callout info">Voici un exemple de fichier `docker-compose.yml` qui permet d'exécuter une application nommée `ma_super_app` avec une base de données PostgreSQL :</p>

```yaml
version: '3.7'

services:
  app:
    image: ma_super_app
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgres://user:password@db:5432/ma_super_app
    links:
      - db
    depends_on:
      - db
  db:
    image: postgres:10
    ports:
      - "5432:5432"
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=ma_super_app

```

<p class="callout info">Pour exécuter les services définis dans ce fichier, utilisez la commande `docker-compose up` dans le même répertoire que le fichier `docker-compose.yml`. Cela démarrera les deux services et les fera fonctionner en parallèle. Vous pouvez également utiliser la commande `docker-compose down` pour arrêter les services et supprimer les conteneurs.</p>

<p class="callout warning">**/!\\ Utiliser correctement les variables d’environnement afin de configurer la base de données et l’application /!\\**</p>

---

#### Le compte rendu du TP doit être déposé sur moodle par chacun des membres du groupe au format GIT le 4 janvier 2023 au plus tard.
