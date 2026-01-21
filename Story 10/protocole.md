## Chronologie de la fuite

## Construction de la chronologie des événements (Registry Explorer)

> **Règle d’or** : tous les horodatages utilisés correspondent au **LastWrite Time des clés registre**. Ce sont les seules dates recevables en forensic Windows.

---

## Première installation de la clé USB (First Install)

**Chemin** :

```
SYSTEM
 └── ControlSet001
     └── Enum
         └── USBSTOR
             └── Disk&Ven_...&Prod_...
                 └── <UID>
```

Cliquer **une seule fois** sur le dossier correspondant à l’UID de la clé USB.

**Horodatage à relever** :

* LastWrite Time (en bas de Registry Explorer)

**Interprétation** :
Première installation connue de la clé USB sur le poste analysé.

---

## Reconnexion de la clé USB

**Chemin** :

```
SYSTEM
 └── ControlSet001
     └── Enum
         └── USB
             └── VID_xxxx&PID_yyyy
                 └── <InstanceID>
```

Cliquer sur le sous-dossier le plus récent correspondant au VID/PID de la clé.

**Horodatage à relever** :

* LastWrite Time

**Interprétation** :
Connexion ou reconnexion effective de la clé USB.

---

## Attribution de la lettre de lecteur

**Chemin** :

```
SYSTEM
 └── ControlSet001
     └── MountedDevices
```

Cliquer sur le dossier **MountedDevices** (pas sur les valeurs).

 **Horodatage à relever** :

* LastWrite Time

**Interprétation** :
Montage du volume USB et attribution d’une lettre (ex : E:).

---

## Accès utilisateur à la clé USB

**Ruche utilisateur requise : NTUSER.DAT**

**Chargement** :

* File → Load Hive → sélectionner `NTUSER.DAT`

**Chemin** :

```
NTUSER.DAT
 └── Software
     └── Microsoft
         └── Windows
             └── CurrentVersion
                 └── Explorer
                     └── MountPoints2
                         └── {GUID ou Lettre}
```

Cliquer sur le dossier correspondant à la clé USB.

**Horodatage à relever** :

* LastWrite Time

**Interprétation** :
Accès utilisateur effectif à la clé USB (exploration, ouverture).


---

### Conclusion 

> « L’analyse des horodatages des clés registre a permis d’établir une chronologie précise de l’utilisation de la clé USB, depuis sa première installation jusqu’à son accès utilisateur, permettant de corréler ces événements avec la période supposée de fuite de données. »
