# **Guide Détaillé : Lancer un Bureau Linux Graphique dans Google Cloud Shell via Docker**

Ce guide vous montre comment exécuter un environnement de bureau Linux (Ubuntu avec LXDE ou XFCE) directement depuis le terminal Google Cloud Shell en utilisant des images Docker. Cela vous permet d'accéder à une interface graphique et des applications comme un navigateur web, sans installer de machine virtuelle complète.

---

### **Prérequis : Accès à Google Cloud et Cloud Shell**

#### **Étape 1 : Accéder à Google Cloud Console**

1.  Rendez-vous sur **[Google Cloud Console](https://console.cloud.google.com/)**.
2.  Connectez-vous avec votre compte Google.
3.  Assurez-vous d’avoir un projet actif dans Google Cloud. Si ce n'est pas le cas, créez-en un.

#### **Étape 2 : Ouvrir le terminal Cloud Shell**

1.  En haut à droite de la console Google Cloud, cliquez sur l’icône **"Activer Cloud Shell"** (ressemble à `>_`).
2.  Attendez que la session Cloud Shell se charge en bas de votre écran. C'est ici que vous taperez les commandes Docker.

---

### **Choix de l'Environnement de Bureau**

Vous avez maintenant le choix entre deux options principales, en fonction de vos besoins :

*   **Option 1 :** Un bureau LXDE très léger mais avec des logiciels plus anciens (image `dorowu`).
*   **Option 2 :** Un bureau XFCE plus moderne sur une base Ubuntu récente, incluant un navigateur à jour (image `linuxserver/webtop`).

Choisissez l'option qui vous convient le mieux et suivez les étapes correspondantes ci-dessous.

---

### **Option 1 : Bureau Ubuntu LXDE Léger (Image `dorowu`)**

Cet environnement est minimaliste et rapide à démarrer, mais les applications comme Firefox peuvent être anciennes.

#### **Étape 3a : Lancer le Conteneur Docker LXDE**

1.  Dans le terminal Cloud Shell que vous avez ouvert à l'étape 2, copiez et collez la commande suivante :

    ```bash
    docker run -d --name lxde-desktop -p 6070:80 dorowu/ubuntu-desktop-lxde-vnc
    ```

    *   `-d` : Lance en arrière-plan.
    *   `--name lxde-desktop` : Donne un nom facile à identifier.
    *   `-p 6070:80` : Mappe le port **6070** de Cloud Shell au port **80** du conteneur (utilisé par noVNC dans cette image).

2.  Appuyez sur **Entrée**. Docker téléchargera l'image (si c'est la première fois) et démarrera le conteneur. Vous verrez un long identifiant s'afficher si tout se passe bien.

#### **Étape 4a : Accéder au Bureau LXDE via l'Aperçu Web**

1.  Attendez quelques secondes que le conteneur démarre complètement.
2.  Cliquez sur l'icône **"Aperçu sur le Web"** (Web Preview) en haut à droite de la barre d'outils du Cloud Shell.
3.  Sélectionnez **"Prévisualiser sur le port 6070"** (Preview on port 6070).
4.  Un nouvel onglet s'ouvrira avec une interface VNC.
5.  Si un mot de passe est demandé, essayez `vncpassword` (c'est souvent le défaut pour ces images). Cliquez sur "Connect".
6.  Vous devriez maintenant voir le bureau Ubuntu LXDE.

---

### **Option 2 : Bureau Ubuntu XFCE Moderne (Image `linuxserver/webtop`)**

Cet environnement utilise une base Ubuntu plus récente, l'environnement XFCE (également léger), et inclut généralement des navigateurs plus à jour. C'est l'option recommandée si vous avez besoin de logiciels récents.

#### **Étape 3b : Lancer le Conteneur Docker Webtop (XFCE)**

1.  Dans le terminal Cloud Shell, copiez et collez la commande suivante :

    ```bash
    docker run -d \
      --name=webtop-ubuntu-xfce \
      -p 6070:3000 \
      -e PUID=1000 \
      -e PGID=1000 \
      -e TZ=Etc/UTC \
      --shm-size="1gb" \
      --security-opt seccomp=unconfined \
      --restart unless-stopped \
      lscr.io/linuxserver/webtop:ubuntu-xfce
    ```

    *   `-d` : Lance en arrière-plan.
    *   `--name=webtop-ubuntu-xfce` : Nom du conteneur.
    *   `-p 6070:3000` : Mappe le port **6070** de Cloud Shell au port **3000** du conteneur (utilisé par l'interface web de Webtop).
    *   `-e PUID/PGID/TZ` : Paramètres utilisateur et fuseau horaire.
    *   `--shm-size="1gb"` : Important pour la stabilité des navigateurs modernes.
    *   `--security-opt seccomp=unconfined` : Recommandé pour la compatibilité.
    *   `--restart unless-stopped` : Redémarrage automatique.
    *   `lscr.io/linuxserver/webtop:ubuntu-xfce` : L'image Docker spécifique (Ubuntu avec XFCE).

2.  Appuyez sur **Entrée**. Docker téléchargera l'image et démarrera le conteneur. Un identifiant s'affichera.

#### **Étape 4b : Accéder au Bureau XFCE via l'Aperçu Web**

1.  Attendez quelques instants (parfois jusqu'à une minute au premier démarrage) que le conteneur soit prêt.
2.  Cliquez sur l'icône **"Aperçu sur le Web"** (Web Preview) en haut à droite de la barre d'outils du Cloud Shell.
3.  Sélectionnez **"Prévisualiser sur le port 6070"** (Preview on port 6070).
4.  Un nouvel onglet s'ouvrira directement sur le bureau Ubuntu XFCE (normalement, aucun mot de passe n'est requis avec cette commande spécifique). Vous trouverez les navigateurs et autres applications dans les menus.

---

### **Gérer vos Conteneurs**

*   **Pour arrêter un conteneur :**
    *   Option 1 : `docker stop lxde-desktop`
    *   Option 2 : `docker stop webtop-ubuntu-xfce`
*   **Pour redémarrer un conteneur arrêté :**
    *   Option 1 : `docker start lxde-desktop`
    *   Option 2 : `docker start webtop-ubuntu-xfce`
*   **Pour supprimer un conteneur (attention, supprime toutes les données non sauvegardées à l'extérieur) :**
    *   Arrêtez-le d'abord (`docker stop ...`).
    *   Puis : `docker rm lxde-desktop` ou `docker rm webtop-ubuntu-xfce`
*   **Pour voir les conteneurs en cours d'exécution :** `docker ps`
*   **Pour voir tous les conteneurs (même arrêtés) :** `docker ps -a`
