### Comprendre **Gobuster** et son Utilisation

#### Qu'est-ce que Gobuster ?

Gobuster est un outil en ligne de commande utilisé lors des tests de pénétration pour découvrir des répertoires, fichiers, sous-domaines DNS ou autres ressources sur un serveur web en les devinant grâce à une liste de mots (*wordlist*). Il permet d'identifier des répertoires ou fichiers cachés qui ne sont pas directement accessibles via les liens publics d’un site web.

c'est à nous de chercher ce fichier ou de rajouter des mots dans le fichier "wordlist.txt"
---

#### Exemple : Utilisation de Gobuster sur `watchseries.im`

J'ai utilisé Gobuster pour effectuer une énumération des répertoires sur le site `https://watchseries.im`. Voici la commande utilisée et une explication détaillée :

---

#### La Commande :

```bash : dans un terminal 
gobuster dir -u https://watchseries.im -w ~/Downloads/common.txt
```

**Explications :**

1. **`gobuster dir`** :
    - Indique le mode d’opération. Ici, le mode est l’énumération des répertoires (*directory enumeration*).
    - Gobuster va tester chaque mot dans la *wordlist* en l’ajoutant à l’URL cible pour vérifier si les répertoires ou fichiers existent.
2. **`-u https://watchseries.im`** :
    - Spécifie l'URL cible, qui est le site à analyser.
3. **`-w ~/Downloads/common.txt`** :
    - Spécifie le chemin vers la *wordlist*. Dans cet exemple, elle est située dans le dossier `~/Downloads` et s’appelle `common.txt`.
    - La *wordlist* contient une liste de noms possibles de répertoires ou fichiers que Gobuster va tester.

---

#### Résultats de Gobuster :

Voici les résultats obtenus :

```plaintext
/Contact              (Status: 200) [Size: 108714]
/Home                 (Status: 200) [Size: 280526]
/contact              (Status: 200) [Size: 108714]
/css                  (Status: 301) [Size: 173] [--> /css/]
/devtools             (Status: 200) [Size: 97]
/file                 (Status: 301) [Size: 175] [--> /file/]
/filter               (Status: 200) [Size: 193931]
/home                 (Status: 200) [Size: 280526]
/images               (Status: 301) [Size: 179] [--> /images/]
/js                   (Status: 301) [Size: 171] [--> /js/]
/logout               (Status: 302) [Size: 23] [--> /]
/movie                (Status: 200) [Size: 194255]
/terms                (Status: 200) [Size: 112368]
```

---

#### Interprétation des Résultats :

1. **Codes de statut HTTP :**
    - **200** : Succès ! Le répertoire ou fichier existe et est accessible.
    - **301** : Redirection permanente. Cela indique que le répertoire existe mais redirige vers un autre chemin (par exemple, `/css/`).
    - **302** : Redirection temporaire. Pointe temporairement vers une autre ressource (par exemple, `/logout` redirige vers `/`).
2. **Taille (Size)** :
    - La taille de la réponse (en octets). Cela peut aider à distinguer des contenus différents sous des chemins similaires (par exemple, `/Home` et `/home` ont la même taille, ce qui signifie qu'ils servent probablement le même contenu).
3. **Exemples de Découvertes :**
    - `/Contact` et `/contact` : Accessibles et renvoient le même contenu.
    - `/css` : Existe, mais redirige vers `/css/`, ce qui indique qu'il s'agit d'un répertoire.
    - `/devtools` : Révèle potentiellement des outils de développement exposés.

---

#### Ce que cela m'apprend :

1. **Utilité des codes de statut HTTP :**
    - **200** indique des ressources valides (répertoires ou fichiers) accessibles.
    - **301** signale des répertoires redirigeant vers un sous-chemin.
    - **302** peut révéler des configurations temporaires ou des points d'entrée possibles.
2. **Découvertes fréquentes :**
    - Outils d’administration (`/devtools`).
    - Fichiers ou répertoires statiques (`/css`, `/images`, `/js`).
    - Termes ou politiques (`/terms`).
    - Mécanismes de connexion/déconnexion (`/logout`).
3. **Améliorer les scans :**
    - Utiliser une *wordlist* plus large ou spécifique (comme celles de **SecLists**) pour des résultats plus complets.
    - Ajuster les paramètres de Gobuster (nombre de threads, délai d'attente, etc.).

---

#### Importance de Gobuster :

- Permet de découvrir des répertoires ou fichiers sensibles qui pourraient être exploités (exemple : `/backup`, `/admin`).
- Vérifie la configuration et la sécurité d’un serveur en recherchant des chemins exposés.
- Aide à identifier des ressources potentiellement utiles pour des tests plus approfondis.

Cet exemple m'a permis de comprendre comment utiliser Gobuster pour détecter des ressources cachées sur un serveur web.

