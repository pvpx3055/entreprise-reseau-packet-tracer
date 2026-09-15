# Projet réseau multi-sites avec Cisco Packet Tracer

## 1. Présentation du projet

Ce projet consiste à concevoir et configurer un réseau d’entreprise composé de trois sites :

* **Dakar**
* **Thiès**
* **Saint-Louis**

Le réseau utilise des VLAN, le routage inter-VLAN, des liaisons Ethernet et série, ainsi que le routage statique et le protocole dynamique **RIP version 2**.

## 2. Architecture du réseau

```text
                    Liaison série
             10.0.12.0/30
       Dakar R1 ---------------- Thiès R2
         |                            |
         |                            |
     VLAN 10/20                    LAN Thiès
         |                       192.168.30.0/24
      Switch
         |
       PCs

       Dakar R1 ---------------- Saint-Louis R3
             Liaison Ethernet
               10.0.13.0/30
                                      |
                                  LAN Saint-Louis
                                  192.168.40.0/24
```

## 3. Plan d’adressage IP

### Site de Dakar

| Équipement | Interface/VLAN         |       Adresse IP |   Passerelle |
| ---------- | ---------------------- | ---------------: | -----------: |
| PC1        | VLAN 10                | 192.168.10.10/24 | 192.168.10.1 |
| PC2        | VLAN 10                | 192.168.10.11/24 | 192.168.10.1 |
| PC3        | VLAN 20                | 192.168.20.10/24 | 192.168.20.1 |
| PC4        | VLAN 20                | 192.168.20.11/24 | 192.168.20.1 |
| R1         | Sous-interface VLAN 10 |  192.168.10.1/24 |            — |
| R1         | Sous-interface VLAN 20 |  192.168.20.1/24 |            — |

### Site de Thiès

| Équipement | Interface |       Adresse IP |   Passerelle |
| ---------- | --------- | ---------------: | -----------: |
| PC5        | LAN       | 192.168.30.10/24 | 192.168.30.1 |
| PC6        | LAN       | 192.168.30.11/24 | 192.168.30.1 |
| R2         | LAN       |  192.168.30.1/24 |            — |

### Site de Saint-Louis

| Équipement | Interface |       Adresse IP |   Passerelle |
| ---------- | --------- | ---------------: | -----------: |
| PC7        | LAN       | 192.168.40.10/24 | 192.168.40.1 |
| PC8        | LAN       | 192.168.40.11/24 | 192.168.40.1 |
| R3         | LAN       |  192.168.40.1/24 |            — |

### Liaisons entre les routeurs

| Liaison           | Routeur |   Adresse IP |
| ----------------- | ------- | -----------: |
| Dakar–Thiès       | R1      | 10.0.12.1/30 |
| Dakar–Thiès       | R2      | 10.0.12.2/30 |
| Dakar–Saint-Louis | R1      | 10.0.13.1/30 |
| Dakar–Saint-Louis | R3      | 10.0.13.2/30 |

## 4. VLAN utilisés à Dakar

| VLAN    | Nom            | Réseau          | Ports concernés |
| ------- | -------------- | --------------- | --------------- |
| VLAN 10 | ADMINISTRATION | 192.168.10.0/24 | PC1, PC2        |
| VLAN 20 | INFORMATIQUE   | 192.168.20.0/24 | PC3, PC4        |

Les ports connectés aux PC sont configurés en **mode access**. Le port du switch connecté au routeur R1 est configuré en **mode trunk** afin de transporter les VLAN 10 et 20.

## 5. Routage inter-VLAN

Le routeur R1 utilise des sous-interfaces :

* `Fa0/0.10` pour le VLAN 10
* `Fa0/0.20` pour le VLAN 20

La commande `encapsulation dot1Q` permet d’associer chaque sous-interface à son VLAN. Cela permet aux ordinateurs des VLAN 10 et 20 de communiquer entre eux.

## 6. Routage entre les sites

Deux méthodes sont utilisées :

### Routage statique

Des routes sont configurées manuellement avec la commande :

```bash
ip route réseau masque prochain-saut
```

### Routage dynamique avec RIP

Le protocole **RIP version 2** permet aux routeurs d’échanger automatiquement les réseaux connus.

```bash
router rip
version 2
network réseau
```

## 7. Tests de fonctionnement

Les tests suivants permettent de vérifier le réseau :

* Ping entre PC1 et PC2 : communication dans le même VLAN.
* Ping entre PC1 et PC3 : communication inter-VLAN.
* Ping entre Dakar et Thiès.
* Ping entre Dakar et Saint-Louis.
* Ping entre Thiès et Saint-Louis.
* Vérification des tables de routage.
* Vérification de l’état des interfaces et des VLAN.

## 8. Commandes de vérification principales

```bash
show ip interface brief
show ip route
show ip protocols
show vlan brief
show interfaces trunk
ping adresse-ip
traceroute adresse-ip
```

## 9. Objectifs du projet

Ce projet permet de comprendre et de pratiquer :

* La segmentation d’un réseau avec les VLAN.
* La configuration d’un trunk.
* Le routage inter-VLAN.
* L’adressage IPv4 et les réseaux `/30`.
* La communication entre plusieurs sites.
* Le routage statique.
* Le routage dynamique avec RIP version 2.
* Le dépannage et la vérification d’un réseau Cisco.
