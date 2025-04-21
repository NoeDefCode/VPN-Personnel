# VPN-Personnel
Création d'un vpn personnel et sécurisé avec Wireguard avec ce tuto : https://www.digitalocean.com/community/tutorials/how-to-set-up-wireguard-on-ubuntu-20-04
1. Installation de Wireguard sur Kali Linux
2. Création d'une clé privé, en changeant ses permisions pour qu'elle ne soit plus lisible par tout le monde, elle sera utilisé en référence pour une autre clé privé :
wg genkey | sudo tee /etc/wireguard/private.key
sudo chmod go= /etc/wireguard/private.key
3. Création d'une autre clé privé correspondante à la précédente :
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
4. Je compte utiliser Wireguard en Ipv6, je dois donc commencer par générer une adresse Ipv6 La manière manière pour commencer est de générer un 64-bit timestamp avec : date +%s%N
5. Récupération de mon identifiant unique de machine avec : cat /var/lib/dbus/machine-id
6. J'utilise ensuite ces deux suites de nombres pour les mélanger gtrâce à l'algorithme SHA 1- Algorithme.
7. 
