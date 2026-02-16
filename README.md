# securite-mobile-lab1
# OBJECTIF 
L’objectif est de mettre en place un environnement contrôlé dédié à l’audit de sécurité mobile en utilisant la distribution spécialisée Mobexler. Cette plateforme centralise l’ensemble des outils nécessaires à l’analyse statique et dynamique, ainsi qu’aux tests d’intrusion sur les applications mobiles.

Dans un premier temps, nous avons réalisé la configuration et la validation de l’infrastructure réseau. Cette étape est essentielle afin de garantir que la machine d’attaque puisse communiquer avec les serveurs distants, assurer la résolution des noms de domaine (DNS) et interagir correctement avec les terminaux cibles.

Ensuite, nous avons appliqué des mesures visant à assurer la persistance et la sécurité de l’environnement, notamment par la création de points de restauration (Snapshots). Enfin, nous avons établi la communication avec le périphérique mobile via le protocole ADB (Android Debug Bridge), une étape préalable indispensable pour permettre l’extraction des données et l’analyse du comportement applicatif en temps réel.
# ETAPE 2 
L’importation de la machine virtuelle se fait via VirtualBox → File → Import Appliance, puis en sélectionnant le fichier .ova et en cliquant sur Import.

Après l’import, il faut configurer le réseau dans VM → Settings → Network en activant deux cartes : Adapter 1 en NAT pour assurer une connexion Internet stable (mises à jour et outils) et Adapter 2 en Host-Only Adapter pour créer un réseau de laboratoire isolé permettant la communication avec la cible Android.

Si l’option Host-Only n’apparaît pas, il faut la créer via VirtualBox → Tools → Network Manager → Host-Only Networks → Create. Le point de contrôle consiste à vérifier que les deux cartes (NAT + Host-Only) sont bien configurées.
<img width="1917" height="1025" alt="1" src="https://github.com/user-attachments/assets/abdafed3-bf84-4bda-b23d-63e480945927" />
<img width="1227" height="743" alt="2" src="https://github.com/user-attachments/assets/56a20566-ce85-4ec0-be73-2f16aedcf0da" />

# ETAPE 3
Une fois connecté, Mobexler démarre normalement et l’environnement est prêt à l’utilisation.
<img width="1624" height="891" alt="3" src="https://github.com/user-attachments/assets/6cc58caf-9a01-402e-a86f-8a3abe6472f7" />

# ETAPE 4
Vérification IP (ip address) : la machine hôte utilise l’adresse 192.168.10.129, nécessaire pour configurer correctement les payloads (ex : APK modifié avec MSFvenom).

Table de routage (ip route) : la route par défaut passe par 192.168.10.2 via ens33. Présence de docker0 (172.17.0.0/16) en état linkdown, indiquant qu’aucun conteneur n’est actif.

Test Internet (ping 8.8.8.8) : connexion externe fonctionnelle avec des temps de réponse stables (~24–25 ms).

Test DNS (ping google.com) : résolution des noms de domaine opérationnelle.

<img width="1920" height="865" alt="4" src="https://github.com/user-attachments/assets/b835ca9e-54fb-45d5-b5d4-4775f093b7a2" />

<img width="1913" height="947" alt="5" src="https://github.com/user-attachments/assets/b14045cc-a2d6-4938-99a6-4f83b3c4ab22" />

# ETAPE 5
Utilisation de la fonctionnalité Snapshot de VMware pour sauvegarder l’état actuel de la machine virtuelle.

Cette étape permet de figer une configuration stable (Clean Baseline) avant toute manipulation de malwares mobiles ou modification de fichiers système critiques.

Création du snapshot nommé CLEAN_BASELINE_TP1.

Description : importation réussie, interfaces réseau NAT + HostOnly opérationnelles, service ADB (Android Debug Bridge) prêt pour les tests sur terminaux mobiles.

Ce point de sauvegarde assure une réinitialisation rapide en cas d’erreur durant le lab.


<img width="1863" height="933" alt="6" src="https://github.com/user-attachments/assets/0cd4e4b8-27a8-4b68-9738-878a13cdc81f" />

<img width="1920" height="1080" alt="7" src="https://github.com/user-attachments/assets/3512c633-f25e-4f22-a52b-6f1bb80c61f4" />

# ETAPE 6  — Préparer la cible Android (OPTION 1)
A1. Activer Developer Options + USB debugging
Aller dans Paramètres → À propos, puis appuyer 7 fois sur Build number. Ensuite, dans Developer options, activer USB debugging.

A2. Connecter l’USB à la VM

Sur VirtualBox : Devices → USB → sélectionner l’appareil.

Sur VMware : connecter le périphérique USB à la machine virtuelle.

A3. Vérifier la connexion ADB dans Mobexler
S’assurer que le smartphone apparaît comme appareil connecté.

Résultat attendu
Le téléphone doit être détecté avec le statut device.

Dépannage

Si le statut est unauthorized : accepter la demande d’autorisation RSA sur le téléphone.

Si aucun appareil n’apparaît : vérifier le USB passthrough et redémarrer le service ADB.


![screen 8](https://github.com/user-attachments/assets/adad356a-dea1-46de-bbe3-dabaf67453a6)
![screen 9](https://github.com/user-attachments/assets/2f20e34f-eb92-492f-a410-41f51bfcaeef)


