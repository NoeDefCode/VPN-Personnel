# VPN-Personnel
Création d'un vpn personnel et sécurisé avec Wireguard avec ce tuto : https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-ubuntu-20-04
1. Installation de Wireguard sur Kali Linux
2. Création d'une clé privé, en changeant ses permisions pour qu'elle ne soit plus lisible par tout le monde, elle sera utilisé en référence pour une autre clé privé :
wg genkey | sudo tee /etc/wireguard/private.key
sudo chmod go= /etc/wireguard/private.key
3. Création d'une autre clé privé correspondante à la précédente :
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
4. Je compte utiliser Wireguard en Ipv6, je dois donc commencer par générer une adresse Ipv6. La manière pour y parvenir et d'abord de générer un 64-bit timestamp avec : date +%s%N
5. Récupération de mon identifiant unique de machine avec : cat /var/lib/dbus/machine-id
6. J'utilise ensuite ces deux suites de nombres pour les mélanger gtrâce à l'algorithme SHA 1- Algorithme.
7. J'obtiens alors un préfixe ULA unique et surtout stable pour chaque client, je l'utilise pour créer les adresses ipv6 pour le serveur et le premier client.
8. Maintenant je vais commencer à configurer mon serveur wireguard avec l'adresse ipv6 nouvellement crée :
![screen 1](https://github.com/user-attachments/assets/ddd9ef87-1a43-4a41-ab63-5bd7250e59b4)
Dans ce terminal, je rentre les informations de ma clé privé, ainsi que de l'adresse iov6 utilisé pour serveur.
9. Je vais maintenant ajuster les paramètres pour utiliser Wireguard sur le traffic internet :
j'utilise un ipv6, je met dans donc cette ligne en premier dans mes paramètres : 
![Screenshot_2025-04-23_06_22_27](https://github.com/user-attachments/assets/242dfb5c-6197-4339-a315-f0a3af7f786c)
Mon serveur Wireguard peut désormais transférer du traffic entrant du VPN virtuel sur d'autres serveurs, puis vers l'internet publics. Cela me permet d'acheminer tout le traffic web de mon homologue wireguard via l'adresse ip de mon serveur, et l'adresse ip public de mon client sera masqué. Grâce à cela, on peut presque dire que mon traffic est anonyme, cepandant, il n'est pas encore sécurisé. pour ce faire, je vais devoir ajuster les paramètres du Pare Feu.

10. Dans cette partie, j’ai configuré mon serveur WireGuard afin de permettre et de router correctement le trafic VPN à travers le pare-feu à l’aide d’UFW et d’iptables. Cela permet notamment d’activer la translation d’adresses (NAT) pour que les clients puissent accéder à Internet via le serveur.

Une étape essentielle était d’ajouter des règles « PostUp » et « PreDown » dans le fichier de configuration /etc/wireguard/wg0.conf pour autoriser le trafic entre l’interface VPN (wg0) et l’interface réseau publique (ex. eth0) via UFW et d’activer le masquerading avec iptables/ip6tables pour IPv4 et IPv6.

J’ai aussi ouvert les ports nécessaires (51820/udp pour WireGuard, et SSH pour l’accès distant), puis redémarré UFW pour appliquer les nouvelles règles.

Problème rencontré :
J’avais oublié d’installer UFW avant de lancer le serveur, ce qui empêchait WireGuard de démarrer correctement, car les commandes ufw route dans la configuration échouaient. Après l’installation d’UFW et l’application des règles, le service a pu fonctionner comme prévu.

Cette démarche assure que la sécurité et le routage du VPN sont pris en compte et évite les erreurs liées à l’absence du pare-feu.
