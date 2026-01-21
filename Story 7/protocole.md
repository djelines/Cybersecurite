# Manipulation détaillée – FTK Imager (Acquisition disque)

## 🛠️ Outil utilisé

* **FTK Imager – Windows**

---

## Téléchargement

📥 **Source officielle uniquement** :
👉 [https://www.exterro.com/ftk-imager](https://www.exterro.com/ftk-imager)

Télécharger :

* **FTK Imager (Windows)**

---

## Lancement correct (preuve forensic)

* Clic droit sur `FTK Imager.exe`
* **Exécuter en tant qu’administrateur**

👉 Obligatoire pour accéder au **disque physique**.

---

## Création de l’image disque

Dans le menu principal :

```
File
 └── Create Disk Image
```

---

## Choix du type d’acquisition 

Dans la fenêtre **Select Source** :

Sélectionner :

* **Physical Drive**

Ne pas choisir :

* Logical Drive
* Contents of a Folder

Sinon, l’acquisition **n’est pas bit-à-bit**.

---

## Sélection du disque

Sélectionner :

```
\\.\PhysicalDrive0
```

(généralement le disque système)

Cliquer sur **Finish**.

---

## Choix du format (CRUCIAL)

Cliquer sur **Add…**

Choisir le format :

* **E01 (EnCase Evidence File)**

### Pourquoi E01 ?

* Format forensic reconnu
* Support des métadonnées
* Support du hachage et de la vérification d’intégrité

---

## Informations de preuve

Renseigner les champs (même fictifs) :

* **Case Number** : TP-Forensic-07
* **Examiner** : Nom Prenom
* **Description** : Disk acquisition

Élément très apprécié dans un rendu forensic.

---

## Choix de la destination

* Disque externe **ou** dossier sécurisé

Nom du fichier :

```
disk_image.E01
```

---

## Lancement de l’acquisition

* Cliquer sur **Start**
* ⏳ Attendre la fin de l’acquisition (durée variable)

---

## Validation finale

Le message suivant doit apparaître :

```
Image verified successfully
```

Avec affichage des empreintes :

* MD5
* SHA1


> « Une image disque bit‑à‑bit du disque physique a été réalisée à l’aide de FTK Imager, avec génération et vérification des empreintes MD5 et SHA1 afin de garantir l’intégrité de la preuve. »

---

## Alternative – Simplification pour TP (VMware)

Si l’acquisition du disque principal n’est pas possible :

### Création d’un disque secondaire virtuel

1. VM → **Settings** → **Add**
2. **Hard Disk** → **New VMDK**
3. Taille : **20 Go**

### Initialisation sous Windows

* Démarrer la VM
* Ouvrir **Gestion des disques**
* Initialiser le disque
* Créer une partition
* Attribuer la lettre : `E:`

### Acquisition avec FTK Imager

* Destination :

```
E:\disk_image.E01
```

---

*Cette procédure garantit une acquisition disque conforme aux standards forensic et acceptable dans un contexte académique ou professionnel.*
