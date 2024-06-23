---
Title: blue
layout : default
---

![Blue](https://labs.hackthebox.com/storage/avatars/52e077ae40899ab8b024afd51cb29b1c.png)

# Synopsis

Selon Hack The Box (HTB), Blue est l'une des machines les plus simples à résoudre sur leur plateforme. Cependant, elle démontre à quel point l'exploit EternalBlue, utilisée dans plusieurs ransomware, peut être critique.

# Enumération

On commence par la recherche des ports disponibles avec l'outil Nmap
```shell
sudo nmap -Pn -T4 -sV 10.10.10.40 -o/-oX/-oG <nomfichier>.format
```
De préference utiliser un format de sortie qui puisse être rapidement triable quand tu vas faire du scripting dans ce cas cela n'est pas neséssaire car on va utlliser **metasploit** pour l'exploitation

![Premier_commande_nmap](../images/nmap_blue_command.png)

Cela nous donne le resultat suivant : 

![Premier_resultat_nmap](../images/resultat_commande_nmap.png)

Dans notre recherche sur les port ouverts, sur wikipedia on trouve que le port 445 pourrais avoir une vulnerabilité appelé "Ethernalblue".

La vulnerabilité EternalBlue profite du protocol NTtrans2 utilisé par le protocole SMBv1 en cas d'un message trop longue dans le BufferOverflow.

Cela permet à un attacant d'executer du code malveillant à un ordinateur distant, cette vulnerabilité à permis la creation du ransomware WannaCry

Pour confirmer si c'est qu'on dit est vrai on cherche la vulnerabilité à l'aide de nmap avec la commande suivante : 

```shell
sudo nmap -T4 -sV --script vuln 10.10.10.40
```
Et on confirme le resultat : 

![vulnerabilité](../images/recherche_de_la_vulnerabilité.png)





# Exploitation

Maintenant dans l'exploitation de cette vulnerabilité, on va utiliser un outil appelé metasploit un outils fait pour donner de l'information sur les divers vulnerabilités existantes et aussi pour l'execution d'un exploit ou payload de manière automatisée. Dans un futur proche, on pourrais construire nous même un programme pour l'execution automatique d'un vulnerabilité à l'aide de python mais on le laissera pour une autre ocasion concentrons nous dans l'exploitation.

avec metasploit on cherche la vulneravilité founis auparavant par nmap, et on cherche la vulnerabilité qu'on considère la plus pertinent

```shell
sudo msfconsole
search CVE-2017-0143
```
![search_vuln](../images/msfconsole_seach_vuln.png)

pour nous la plus pertinent est l'option 0 on demande de montrer les payloads disponnibles pour ethernalblue et on choisi la plus pertinent, d'un payload appelé reverse_TCP

```shell
use 0
show payloads
set payoad 14
```

# Elévation de « privilèges »