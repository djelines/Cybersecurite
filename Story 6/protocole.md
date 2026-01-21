# Acquisition de la mémoire vive (Forensic)

## Ressource pédagogique

* Vidéo de référence :
  [https://www.youtube.com/watch?v=VK3fvNFGAzE](https://www.youtube.com/watch?v=VK3fvNFGAzE)

---

## Outils utilisés

### Acquisition mémoire

* **DumpIt** – MAGNET Forensics

### Analyse mémoire

* **Volatility 3**

---

## Téléchargement de DumpIt

1. Accéder au site officiel MAGNET Forensics :
   [https://www.magnetforensics.com/fr/outils-gratuits/](https://www.magnetforensics.com/fr/outils-gratuits/)
2. Rechercher **"MAGNET DumpIt pour Windows"**
3. Cliquer sur **Télécharger / Get Free Tool**
4. Remplir le formulaire (email requis)
5. Télécharger DumpIt via le lien reçu par email ou redirection

> Outil officiel, fiable et adapté aux analyses forensic.

---

## Préparation de DumpIt

1. Dézipper l’archive téléchargée
2. Identifier l’exécutable `DumpIt.exe` (x86 / x64)
3. Copier `DumpIt.exe` :

   * sur une clé USB
   * ou dans un dossier local accessible sur le système cible

---

## Acquisition de la mémoire vive

> Cette étape doit être réalisée **avec les droits administrateur**.

### Étapes

1. Ouvrir l’invite de commandes :

   * Clic droit → **Exécuter en tant qu’administrateur**
2. Se placer dans le dossier contenant DumpIt :

```
cd C:\chemin\vers\DumpIt
```

3. Lancer l’outil :

```
DumpIt.exe
```

4. Confirmer l’acquisition :

```
Press Y to start
```

5. DumpIt effectue alors :

   * La capture complète de la RAM
   * La génération d’un fichier `MEMORY.DMP`
   * L’enregistrement dans le dossier courant

---

## Vérification du dump mémoire

### Critères de validation

* ✔ Taille cohérente avec la RAM installée
  (ex : 8 Go de RAM → ~8 Go de dump)
* ✔ Fichier nommé `MEMORY.DMP`
* ✔ Aucune erreur affichée à la fin

---

## Analyse mémoire avec Volatility 3

### Vérification de la validité du dump

```
python vol.py -f "C:\Users\cleme\Desktop\RAM\x64\MEMORY.DMP" windows.info
```

**Éléments à relever :**

* Version de Windows
* Architecture (x64)
* Build

> « Le plugin windows.info confirme que le dump mémoire est valide et exploitable, correspondant à un système Windows 64 bits. »

---

## Identification des processus actifs

### Processus actifs

```
python vol.py -f "C:\Users\cleme\Desktop\RAM\x64\MEMORY.DMP" windows.pslist
```

### Processus masqués ou terminés

```
python vol.py -f "C:\Users\cleme\Desktop\RAM\x64\MEMORY.DMP" windows.psscan
```

### Méthodologie

* `pslist` → processus actifs
* `psscan` → processus terminés ou masqués
* Présent dans `psscan` mais absent de `pslist` → **suspect potentiel**

> « L’analyse des processus via pslist et psscan a permis d’identifier les processus actifs et de détecter d’éventuels processus masqués ou terminés anormalement. »

---

## Détection de processus suspects

### Analyse des lignes de commande

```
python vol.py -f "C:\Users\cleme\Desktop\RAM\x64\MEMORY.DMP" windows.cmdline
```

### Indices à surveiller

* Nom proche d’un processus légitime (`svch0st.exe`)
* Chemin inhabituel (`AppData`, `Temp`)
* Absence de parent légitime

> « Certains processus présentent des caractéristiques atypiques suggérant une activité potentiellement malveillante. »

---

## Connexions réseau suspectes

### Analyse réseau

```
python vol.py -f "C:\Users\cleme\Desktop\RAM\x64\MEMORY.DMP" windows.netscan
```

**À relever :**

* IP distante
* Port
* PID
* Processus associé

> « Le plugin windows.netscan a permis d’identifier les connexions réseau actives et de les corréler aux processus correspondants. »


---

## Conclusion finale

> « Le dump de la mémoire vive a été réalisé avec succès à l’aide de DumpIt.
> L’analyse du dump à l’aide de Volatility 3 a permis d’identifier les processus actifs ainsi que les connexions réseau, répondant pleinement aux objectifs de l’US 6. »

---

*L’acquisition et l’analyse mémoire constituent une étape clé de toute investigation forensic avancée, permettant de révéler des artefacts invisibles sur le dis
