# Analyse dynamique du malware

### Modifications système

* Création d’un dossier `C:\WindSystm`

  * Contient :

    * Le malware complet
    * Des fichiers de logs
* Création de fichiers `.pf` dans `C:\Windows\Prefetch`
* Création ou modification de clés de registre
* Modification du fichier `Res.exe`

---

### Processus détectés

Les processus suivants doivent être observés lors de l’exécution :

* `Res.exe`
* `conhost.exe` (invite de commande invisible)
* `cmd.exe` (invite de commande visible)
* `xcopy.exe` (lancé depuis `cmd.exe`)

---

### Activité réseau

* Connexions réseau observables (DNS / HTTP / TCP)
* Trafic capturé et analysé

---

## Environnement d’analyse

> L’analyse doit être réalisée **exclusivement dans une sandbox VMware isolée**.

Objectif : **capturer en temps réel** les activités :

* Réseau
* Fichiers / Registre
* Processus

---

## Outils nécessaires

* **Wireshark** – Analyse du trafic réseau
* **Procmon (Process Monitor)** – Activité fichiers, registre et processus
* **Process Hacker 2** – Arborescence des processus, mémoire et connexions

---

## Phase 1 : Préparation de l’environnement

> Les outils de surveillance doivent être lancés **AVANT** l’exécution du malware.

### 1. Process Hacker 2

* Lancer **en tant qu’Administrateur**
* Action :

  * Le laisser ouvert pour surveiller l’apparition de nouveaux processus
  * Les nouveaux processus apparaissent en **vert**

---

### 2. Wireshark

* Ouvrir Wireshark
* Sélectionner la carte réseau active (Ethernet ou Wi-Fi)
* **Ne pas démarrer la capture immédiatement**

  * Si une capture est déjà active : cliquer sur le bouton rouge (Stop)

#### Filtres (optionnel)

* Si l’IP cible est connue :

  ```
  ip.addr == [IP_de_la_VM]
  ```
* Sinon, laisser vide pour capturer tout le trafic

---

### 3. Procmon (Process Monitor)

* Ouvrir Procmon
* Cliquer sur l’icône **Filter** (entonnoir)

#### Filtre recommandé

Si le programme analysé est connu :

* `Process Name` **is** `Res.exe`
* Action : **Include → Add**

---

## Phase 2 : Exécution (Top départ)

Suivre **strictement cet ordre** afin de synchroniser les logs :

1. **Démarrer Wireshark** (aileron de requin bleu)
2. **ACTION CIBLE : Lancer `Res.exe`**
3. Attendre quelques secondes après la fin de l’exécution
4. Observer les fenêtres des outils

---

## Phase 3 : Analyse et extraction des données

Les informations collectées seront utilisées pour le **rapport d’analyse dynamique**.

---

### A. Analyse réseau – Wireshark

* Examiner la colonne **Protocol**
* Rechercher :

  * HTTP
  * DNS
  * TCP suspect

Noter :

* IP distantes
* Domaines contactés
* Protocoles utilisés

---

### B. Analyse système – Procmon

Utiliser les icônes de filtrage :

* **Registry Activity** :

  * Clés de registre modifiées ou créées (persistance)

* **File System Activity** :

  * Fichiers créés ou modifiés
  * Exemple : logs, copies du malware

* **Process / Thread Activity** :

  * Processus lancés et enfants

#### Points clés à relever

* Chemins des fichiers (`WriteFile`)
* Clés de registre (`RegSetValue`, `RegCreateKey`)

---

### C. Analyse mémoire & processus – Process Hacker 2

* Observer tous les processus actifs
* Examiner :

  * Processus enfants
  * Threads
  * Connexions réseau ouvertes

#### Analyse mémoire

* Clic droit sur le processus → **Memory Regions**
* Vérifier :

  * Injection de code
  * Régions mémoire suspectes (ex : section vide ~1100 octets)

---

## Bonus – Rôle des outils

### Procmon (Process Monitor)

* Véritable **journaliste du système Windows**
* Capture :

  * Fichiers
  * Registre
  * Processus
  * Threads

**Usage malware** :

* Détection de persistance
* Création de fichiers cachés
* Appels suspects (`VirtualProtect`, modifications mémoire)

Génère beaucoup de logs → filtrage indispensable

---

### Process Hacker 2

* Gestionnaire de tâches avancé

**Usage malware** :

* Détection de processus enfants
* Inspection mémoire
* Visualisation des connexions réseau par processus

---

## Conclusion

L’analyse dynamique permet de confirmer **le comportement réel du malware**, de valider les hypothèses issues de l’analyse statique et d’extraire des **preuves concrètes** exploitables pour la détection et la réponse à incident.

> Étape indispensable dans toute investigation malware professionnelle.
