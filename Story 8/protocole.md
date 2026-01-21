# Analyse croisée RAM / Disque & Reverse Engineering

## Outils utilisés

Cette analyse repose sur une combinaison d'outils forensiques et de reverse engineering pour corréler les données volatiles et persistantes :

* **Volatility 3** : Framework d'analyse de la mémoire vive (RAM).
* **FTK Imager** : Outil d'acquisition et d'analyse d'images disque.
* **Ghidra** : Outil de reverse engineering (désassemblage et décompilation).
* **Java JDK 17+** : Environnement nécessaire pour exécuter Ghidra.

---

## Prérequis et Installation

1.  **Récupération des preuves** :
    * Disposer du **dump de la mémoire vive** (capturé au moment de l'exécution du malware).
    * Disposer de l'**image disque bit-à-bit** (format `.E01`).

2.  **Installation des logiciels** :
    * Installer **Java JDK 17+**.
    * Télécharger et extraire **Ghidra**.
    * Configurer **Volatility 3** (Python).
    * Installer **FTK Imager**.

---

## Corrélation Processus (RAM) vs Fichier (Disque)

L'objectif est de vérifier si le code exécuté en mémoire est identique au fichier stocké sur le disque (détection d'injection ou d'unpacking).

### 1. Extraction du processus en RAM (Volatility)

Ouvrir un terminal (CMD) dans le dossier de Volatility.

1.  **Lister les processus** pour trouver le PID du malware :
    ```bash
    python vol.py -f [Chemin_Dump_RAM] windows.pslist
    ```

2.  Noter le **PID** du processus `Res.exe`.

3.  **Créer un dossier de sortie** :
    ```bash
    mkdir output
    ```

4.  **Extraire les dumpfiles** du processus ciblé :
    ```bash
    python vol.py -f [Chemin_Dump_RAM] -o output windows.dumpfiles --pid [PID_Res.exe]
    ```

5.  Identifier le fichier image extrait (format type `file.0x9838.....ImageSectionObject.Res.exe.img`).

6.  **Calculer le hash MD5** de ce fichier extrait :
    ```cmd
    certutil -hashfile [Chemin_Fichier_.img] MD5
    ```
    >  **Note :** Conserver ce hash pour la comparaison.

### 2. Extraction du hash sur Disque (FTK Imager)

1.  Ouvrir **FTK Imager**.
2.  Ajouter l'image disque : `File` > `Add Evidence Item` > `Image File` (sélectionner le fichier `.E01`).
3.  Naviguer dans l'arborescence (`Basic data partition` > `root`...) pour localiser `Res.exe`.
4.  Clic droit sur `Res.exe` > **Export File Hash List**.
5.  Ouvrir le CSV généré et récupérer la valeur dans la colonne **MD5**.

### 3. Analyse comparative

Comparer les deux hashs MD5 obtenus :

* **Hashs identiques** : Le processus en mémoire est le même que sur le disque.
* **Hashs différents** : Le code en mémoire a été modifié (**Unpacking** ou **Injection de code**) au moment de l'exécution.

---

## Analyse statique et comportementale avec Ghidra

### Configuration du projet

1.  Lancer `ghidrarun.bat`.
2.  Créer un nouveau projet : `File` > `New Project` > `Non Shared` > Nommer "Analyse Malware".
3.  Importer les fichiers (`I`) : Sélectionner `Env.exe` et `Res.exe`.
4.  Ouvrir les fichiers pour analyse (Double-clic > "Yes").

### Méthodologie d'analyse

1.  Dans la fenêtre **Symbol Tree** (à gauche), dérouler le dossier **Main**.
2.  Sélectionner **Functions**.
3.  Commencer l'analyse depuis le point d'entrée `entry`.

---

## Fonctions principales identifiées

L'analyse du code décompilé permet de déterminer les rôles distincts des deux exécutables.

### 📧 Analyse de `Env.exe` (Module d'Exfiltration)

* **Communication SMTP** : Utilisation du protocole SMTP pour l'envoi de mails.
* **Identifiants découverts** :
    * **Expéditeur** : adresse `@laposte.net`.
    * **Destinataire** : adresse `@gmail.com`.
* **Vol de données** : Lecture du fichier `log.txt` (qui contient les frappes clavier enregistrées).
* **Action finale** : Envoi automatique du contenu des logs vers l'adresse Gmail de l'attaquant.

### ⌨️ Analyse de `Res.exe` (Dropper & Keylogger)

* **Persistance** : Modifie le registre Windows pour assurer le démarrage automatique du malware.
* **Installation** : Copie les fichiers malveillants vers un dossier spécifique (`C:\WindSyst`).
* **Logique Keylogger** : Ce binaire contient la logique de surveillance clavier (`GetAsyncKeyState`) pour traquer chaque frappe de l'utilisateur.

---

## Conclusion

Cette méthodologie a permis de valider la présence d'une charge active en mémoire potentiellement "unpacked" et de comprendre la chaîne d'attaque complète :

1.  **Res.exe** assure l'infection, la persistance et la capture (Keylogger).
2.  **Env.exe** gère l'exfiltration des données volées par email.

> *Cette procédure combine l'analyse forensique et le reverse engineering pour une compréhension complète de la menace.*