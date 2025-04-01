# Voici un guide détaillé et clair pour installer et utiliser une machine virtuelle Ubuntu LXDE sur Google Cloud

---

### **Étape 1 : Accéder à Google Cloud Console**  
1. Rendez-vous sur **[Google Cloud Console](https://console.cloud.google.com/)**.  
2. Connectez-vous avec votre compte Google.  
3. Assurez-vous d’avoir un projet actif dans Google Cloud. Si ce n'est pas le cas, créez-en un.

---

### **Étape 2 : Ouvrir le terminal Cloud Shell**  
1. En haut à droite, cliquez sur l’icône du **Cloud Shell** (terminal intégré de Google Cloud).  
2. Une fois le terminal chargé, tapez la commande suivante pour exécuter une machine Ubuntu avec une interface graphique LXDE et un serveur VNC :  

   ```bash
   docker run -p 6070:80 dorowu/ubuntu-desktop-lxde-vnc
   ```
3. Appuyez sur **Entrée** et attendez que l’image Docker se télécharge et s’exécute.

---

### **Étape 3 : Ouvrir l’aperçu Web**  
1. Une fois la machine lancée, cliquez sur **"Aperçu sur le Web"** en haut à droite du terminal Cloud Shell.  
2. Sélectionnez **"Modifier le port"** et entrez **6070** comme port.  
3. Cela ouvrira l’environnement Ubuntu LXDE directement dans un nouvel onglet du navigateur.
