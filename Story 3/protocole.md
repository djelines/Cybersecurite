# Analyse statique de malware – Reverse Engineering

## Outil utilisé : PeStudio

**PeStudio** est un outil d’analyse statique spécialisé dans les exécutables Windows (PE – Portable Executable).

Il permet notamment :

* L’analyse des sections PE
* L’identification des imports suspects
* L’extraction de chaînes de caractères
* La détection d’indicateurs malveillants

---

## Installation de PeStudio

1. Télécharger **PeStudio** depuis le site officiel.
2. Extraire l’archive ZIP de l’outil.
3. Lancer `pestudio.exe` (aucune installation classique requise).

Il est recommandé d’effectuer cette étape dans une **machine virtuelle isolée**.

---

## Préparation du malware

1. Copier l’archive **ZIP contenant le malware** dans la machine virtuelle.

2. Vérifier que :

   * Les **dossiers partagés sont désactivés**
   * La machine n’a **pas d’accès Internet** (ou réseau isolé)

3. Décompresser le fichier ZIP du malware **sans l’exécuter**.

---

## Analyse statique avec PeStudio

1. Ouvrir **PeStudio**.
2. Glisser-déposer le fichier exécutable du malware dans l’interface.

### Analyse des sections PE

* Identifier les sections standards et non standards :

  * `.text`
  * `.data`
  * `.rdata`
  * Sections suspectes (nom anormal, taille inhabituelle)

Des sections inhabituelles peuvent indiquer de l’obfuscation ou du packing.

---

### Analyse des imports

* Examiner les bibliothèques importées (DLL) :

  * `kernel32.dll`
  * `user32.dll`
  * `advapi32.dll`
  * `wininet.dll`, `ws2_32.dll`

* Identifier les fonctions sensibles :

  * Gestion des processus
  * Accès réseau
  * Manipulation du registre
  * Persistance

---

### Analyse des chaînes de caractères

* Extraire les **strings ASCII et Unicode**
* Rechercher :

  * URLs
  * Adresses IP
  * Noms de fichiers
  * Clés de registre
  * Commandes système

Les chaînes fournissent souvent des indices directs sur le comportement du malware.

---

## Fonctions principales identifiées

À partir des imports, chaînes et sections, déterminer :

* Vol d’informations
* Communication avec un serveur C2
* Téléchargement de charges additionnelles
* Persistance au démarrage
* Élévation de privilèges

---

## Indicateurs de compromission (IOC)

Extraire et documenter les IOC suivants :

* Hashs du fichier (MD5, SHA-1, SHA-256)
* URLs ou domaines
* Adresses IP
* Noms de fichiers ou chemins suspects
* Clés de registre utilisées

Ces IOC pourront être utilisés pour la détection et la remédiation.

---

## Conclusion

L’analyse statique permet d’obtenir une **vision globale du malware** sans exécution. Elle constitue une étape essentielle avant toute analyse dynamique ou reverse engineering avancé.

> Cette méthodologie est conforme aux bonnes pratiques professionnelles en cybersécurité et reverse engineering.
