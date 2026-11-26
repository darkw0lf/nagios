# nagios

Scripts pour la supervision **Nagios** : un plugin de détection de tentative de DDoS et un générateur automatique de configuration à partir d'un scan Nmap.

## `check_ddos.py` — plugin de détection de SYN flood

Compte les adresses IP distinctes ayant des connexions en état `SYN_RECV` (via `ss`) et renvoie un état Nagios selon les seuils fournis.

```bash
./check_ddos.py -w 200 -c 300
# OK - The host has 12 active connections
```

| Option | Rôle |
|---|---|
| `-w`, `--warning` | Seuil d'avertissement (WARNING, code 1) — obligatoire |
| `-c`, `--critical` | Seuil critique (CRITICAL, code 2) — obligatoire, supérieur à `-w` |
| `-V`, `--version` | Affiche la version |
| `-A`, `--author` | Affiche l'auteur |

Sur un hôte distant, exécutez-le via NRPE :

```
# /etc/nagios/nrpe.cfg
command[check_ddos]=/usr/lib/nagios/plugins/check_ddos.py -w 200 -c 300
```

Adapté du plugin `check_ddos` de DI-FCUL (licence GPL).

## `nmaptonagios.sh` — génération de la configuration Nagios

Découvre les hôtes actifs d'une plage IP avec Nmap, détecte leur système et leurs services, puis génère un fichier `.cfg` Nagios (hôtes et services) dans `/tmp`.

```bash
sudo ./nmaptonagios.sh 192.168.1.0/24
```

Nécessite `nmap` et doit être lancé en root (détection d'OS). Relisez le fichier généré avant de le copier dans `/etc/nagios/conf.d/`.

⚠️ Ne scannez que des réseaux dont vous avez la responsabilité ou une autorisation écrite.

## Licence

GPL (voir l'en-tête de chaque script).
