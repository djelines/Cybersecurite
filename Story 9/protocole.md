# Identification de la clé USB utilisée

## Contexte technique

Sous Windows, les informations relatives aux périphériques USB sont stockées dans la **ruche SYSTEM** du registre.
L’identification du **numéro de série matériel** se fait principalement via la clé `USBSTOR`.

---

## Outil utilisé

* **Registry Explorer** (Eric Zimmerman)

---

## Emplacement principal – UID matériel (indépendant du poste)

Dans **Registry Explorer**, naviguer précisément vers :

```text
SYSTEM
 └── ControlSet001
     └── Enum
         └── USBSTOR
```

---

## Analyse de la clé USBSTOR

### Dossiers fabricant / modèle

Sous `USBSTOR`, observer des dossiers du type :

```text
Disk&Ven_Kingston&Prod_DataTraveler_2.0&Rev_1.00
Disk&Ven_SanDisk&Prod_Cruzer_Blade&Rev_1.26
```

Ces dossiers indiquent **le constructeur et le modèle**, **pas** le numéro de série.

---

### Extraction du numéro de série (UID)

À l’intérieur de chaque dossier fabricant, se trouve **un sous-dossier unique**, par exemple :

```text
4C530000281008116284&0
```

**Cette valeur correspond au numéro de série unique (UID) de la clé USB**.

Elle est :

* unique au périphérique
* indépendante du poste Windows
* exploitable pour l’attribution du matériel

---

## Vérification de cohérence

Cliquer sur le dossier du numéro de série et vérifier dans le panneau de droite :

* `FriendlyName`
* `DeviceDesc`

Si la valeur indique **USB Mass Storage Device**, l’identification est correcte.

---

## Emplacement alternatif (selon TP / OS)

Si aucune donnée n’apparaît dans `USBSTOR`, vérifier également :

```text
SYSTEM
 └── ControlSet001
     └── Enum
         └── USB
```

Certaines configurations pédagogiques ou pilotes USB stockent l’information ici.

---

## Identifiant USB dépendant du poste (complément)

> **Ce n’est PAS l’UID matériel**, mais un mapping local utile en corrélation.

### Emplacement : MountedDevices

```text
SYSTEM
 └── ControlSet001
     └── MountedDevices
```

### Contenu observé

* `\DosDevices\E:`
* `\??\Volume{GUID}`

Les données binaires associées permettent de :

* relier une **lettre de lecteur** (ex : E:)
* à un **volume USB précis**

Ces informations sont **dépendantes du poste Windows**.

---

## Chemins analysés (exemple TP)

* `SYSTEM: ControlSet001\Enum\USBSTOR`
* `SYSTEM: ControlSet001\MountedDevices`



> « Le numéro de série (UID) de la clé USB a été identifié dans la ruche SYSTEM, clé
> `HKLM\SYSTEM\ControlSet001\Enum\USBSTOR`.
> L’identifiant extrait est : **4C530000281008116284&0**. »

---

*L’identification du numéro de série USB permet d’attribuer formellement un périphérique à un événement ou un utilisateur, étape clé en investigation forensic.*
