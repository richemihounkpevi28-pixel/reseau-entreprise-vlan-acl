# Réseau d'entreprise sécurisé — Cisco Packet Tracer

## Présentation
Simulation du réseau d'une petite entreprise avec trois VLAN, du routage inter-VLAN et une ACL qui bloque l'accès des invités au serveur interne.

## Objectifs techniques
- Créer et nommer trois VLAN.
- Affecter les ports access aux bons VLAN.
- Configurer des trunks 802.1Q.
- Configurer le routage inter-VLAN avec des sous-interfaces (router-on-a-stick).
- Appliquer et tester une ACL IPv4.
- Vérifier la configuration avec les commandes `show` et `ping`.

## Équipements
- Router0
- Switch0 et Switch1
- PC0, PC1, PC2 et PC3
- Server0

## VLAN et adressage IP

| VLAN | Service | Réseau | Passerelle |
|---|---|---|---|
| 10 | Administration | `192.168.10.0/24` | `192.168.10.1` |
| 20 | Employés | `192.168.20.0/24` | `192.168.20.1` |
| 30 | Invités | `192.168.30.0/24` | `192.168.30.1` |

| Appareil | Adresse IP | Masque | Passerelle |
|---|---|---|---|
| PC0 (VLAN 10) | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` |
| PC1 (VLAN 20) | `192.168.20.10` | `255.255.255.0` | `192.168.20.1` |
| PC2 (VLAN 30) | `192.168.30.10` | `255.255.255.0` | `192.168.30.1` |
| PC3 (VLAN 20) | `192.168.20.11` | `255.255.255.0` | `192.168.20.1` |
| Server0 (VLAN 10) | `192.168.10.100` | `255.255.255.0` | `192.168.10.1` |

## ACL de sécurité

ACL 130 appliquée en entrée sur la sous-interface du VLAN 30 :

```text
access-list 130 deny ip 192.168.30.0 0.0.0.255 host 192.168.10.100
access-list 130 permit ip any any

interface gigabitEthernet 0/0.30
 ip access-group 130 in
```

La première règle bloque le trafic IP provenant du réseau Invités à destination du serveur `192.168.10.100`. La seconde autorise le reste du trafic qui atteint cette ACL.

**Limite connue :** cette ACL ne bloque pas tous les accès des invités aux autres ressources internes ; elle cible uniquement l'adresse IP du serveur.

## Vérifications

Commandes utilisées :
```text
show vlan brief
show interfaces trunk
show ip interface brief
show access-lists
```

Tests réalisés :
- PC0 vers le serveur : ping réussi.
- PC1 vers le serveur : ping réussi.
- PC2 vers le serveur : ping échoué après application de l'ACL.

Le premier ping peut perdre un paquet pendant la résolution ARP. Relancer le test permet de vérifier le résultat une fois les adresses MAC apprises.

## Ouvrir le projet
1. Ouvrir Cisco Packet Tracer.
2. Choisir **File → Open**.
3. Sélectionner `reseau-entreprise-vlan-acl.pkt`.

## Preuves
Ajouter tes propres captures dans le dossier `preuves/` et utiliser ces noms :
- `01-topologie.png`
- `02-vlan.png`
- `03-trunks.png`
- `04-routage-acl.png`
- `05-tests-securite.png`

Les captures et le fichier `.pkt` ne sont pas inclus dans ce kit : copie-les toi-même dans le dossier.

## Compétences travaillées
VLAN, ports access, trunk 802.1Q, adressage IPv4, passerelles, routage inter-VLAN, ACL IPv4 et diagnostic avec Cisco IOS.

## Améliorations possibles
- Restreindre les invités à un réseau externe simulé.
- Ajouter une ACL qui bloque tous les réseaux internes aux invités.
- Activer un service HTTP sur Server0.
- Documenter les ports exacts d'après le câblage final.
