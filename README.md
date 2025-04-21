# VPN-Personnel
Création d'un vpn personnel et sécurisé
1. Installation de Wireguard sur Kali Linux
2. Création d'une clé privé, en changeant ses permisionspour qu'elle ne soit plus lisible par tout le monde, qu sera utilisé en référence pour une autre clé privé :
wg genkey | sudo tee /etc/wireguard/private.key
sudo chmod go= /etc/wireguard/private.key
3. Création d'une autre clé privé correspondante à la précédente :
sudo cat /etc/wireguard/private.key | wg pubkey | sudo tee /etc/wireguard/public.key
4. 
