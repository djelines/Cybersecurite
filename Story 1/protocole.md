# Création d’une machine virtuelle Windows 11 (Analyse Malware)

## Prérequis

* VMware installé
* Fichier ISO de **Windows 11** téléchargé

## Étapes de création

1. Ouvrir **VMware**.
2. Créer une **nouvelle machine virtuelle**.
3. Lors de la création, sélectionner le **fichier ISO de Windows 11**.
4. Configurer le **disque dur** avec **au minimum 32 Go**.

## Configuration du matériel

1. Dans les paramètres matériels de la machine virtuelle :

   * Aller dans **Réseau**.
   * Changer le mode réseau de **NAT** vers **Segment LAN**.
   * Créer un **nouveau segment LAN** nommé :

     * `Analyse Malware`

2. Finaliser la création de la machine virtuelle.

## Paramètres de sécurité post-création

1. Faire un **clic droit** sur la machine virtuelle.
2. Sélectionner **Paramètres**.
3. Aller dans l’onglet **Options** (en haut).
4. Ouvrir **Dossiers partagés**.
5. Régler l’option sur **Désactivé**.

---

> Cette configuration est recommandée pour l’analyse de malwares afin de limiter les interactions avec le système hôte.
