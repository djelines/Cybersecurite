# Analyse malware – Détection et comparaison

## Résultats de détection

### Verdict

Indique si le fichier analysé est considéré comme :

* **Malware** : fichier clairement malveillant
* **Suspect (PUP / PUA)** : programme potentiellement indésirable
* **Clean** : fichier sain

Le verdict est une synthèse des résultats fournis par les moteurs d’analyse.

---

### Classification

Décrit le **type de menace** identifié, par exemple :

* Ransomware
* Trojan Stealer
* Backdoor
* Loader
* Spyware

Cette information aide à comprendre l’objectif principal de l’attaque.

---

### Comportement observé

Correspond aux actions détectées lors de l’analyse dynamique ou heuristique, par exemple :

* Tentative de modification du registre Windows
* Connexion à une adresse IP ou un serveur distant
* Communication avec une infrastructure de commande et contrôle (C2)
* Chiffrement de documents
* Création de mécanismes de persistance

Le comportement permet d’évaluer l’impact réel sur le système.

---

### Score de détection

Représente le nombre de moteurs antivirus ayant identifié le fichier comme dangereux.

**Exemple** :

* `56 / 72` → 56 antivirus sur 72 détectent le fichier comme malveillant

Un score élevé augmente fortement le niveau de confiance dans la détection.

---

## Comparaison avec les bases existantes

Un fichier n’est jamais analysé isolément. Il est comparé à de vastes **bases de données de menaces connues**.

### Bases de signatures (Hashes)

* Comparaison des empreintes **SHA-256** ou **MD5** avec des bases telles que :

  * VirusTotal
  * MalwareBazaar
  * Hashlookup

Si le hash correspond exactement, le malware est déjà référencé et identifié.

---

### Bases de CVE (Vulnérabilités)

* Vérifie si le malware exploite une faille connue
* Comparaison avec le référentiel **CVE (Common Vulnerabilities and Exposures)**

Cela permet d’identifier la vulnérabilité utilisée pour l’infection ou l’escalade de privilèges.

---

### Bases de tactiques – MITRE ATT&CK

* Analyse des techniques employées par l’attaquant (ex : *Injection de code*)
* Mise en correspondance avec la matrice **MITRE ATT&CK**

Cette approche permet :

* D’identifier les tactiques, techniques et procédures (TTP)
* De relier le malware à une famille ou un groupe APT connu

---

## Section : Hachages (Empreintes numériques)

Les hachages sont des **identifiants uniques** d’un fichier. Une simple modification d’un bit modifie entièrement l’empreinte.

### MD5

* Hachage de 128 bits
* Très rapide et largement utilisé comme identifiant
* Obsolète pour la sécurité cryptographique

### SHA-1

* Empreinte de 160 bits
* Plus robuste que MD5
* Considéré comme faible aujourd’hui

### SHA-256

* Standard de référence actuel
* Le plus utilisé pour la recherche et l’identification de malwares
* Prioritaire dans les investigations de sécurité

### Vhash

* *VirusTotal Hash*
* Regroupe des fichiers structurellement similaires
* Utile pour identifier des variantes proches

### SSDEEP

* Hachage flou (*Fuzzy Hash*)
* Permet d’identifier des fichiers presque identiques
* Très utile pour détecter différentes versions d’un même malware

### TLSH

* *Trend Micro Locality Sensitive Hash*
* Hachage de similarité avancé
* Sert à identifier des familles ou des variantes de malwares

---

> Cette méthodologie est essentielle pour une analyse malware rigoureuse et la rédaction d’un rapport technique professionnel.
