# amp-mirror — fichiers que les templates AMP ne peuvent pas retrouver

Miroir des archives et des sources dont dépendent les templates du dépôt
`Hellm4nbot/amp-templates`. Il existe pour une raison précise : certaines sources
sont **mutables**, et un serveur installé une semaine plus tôt devient impossible à
reconstruire quand elles bougent.

Dépôt volontairement séparé : AMP clone `amp-templates` à chaque *Fetch*, et un
clone prend les objets de toutes les branches. Cinquante mégaoctets de binaires à
côté des `.kvp` ralentiraient chaque rafraîchissement de templates pour des fichiers
qu'AMP n'utilise jamais.

## Contenu

```
nightcoop/   clients du mod (0.2.0-beta, deux variantes de version de jeu)
             + sources du hub au commit épinglé + ENet + MANIFEST.txt
rwmt/        Server-beta.zip et Multiplayer-beta.zip — release au tag ROULANT
nitrox/      Nitrox_1.8.1.0_linux_x64.zip
mirror-templates.txt   politique par template et toutes les empreintes
```

## Pourquoi ces fichiers et pas d'autres

- **RWMT, priorité haute.** La release s'appelle `continuous` : ses deux archives sont
  écrasées à chaque build. Le serveur et le client doivent correspondre au bit près,
  donc une version écoulée n'existe plus nulle part ailleurs.
- **Nitrox.** Tag immuable, mais projet bénévole dont l'archive est pointée en dur par
  le template. 23 Mo, le risque vaut la copie.
- **NightCoop.** Le projet ne publie aucun binaire Linux : sans les sources, l'Update du
  template échouerait si le dépôt amont disparaissait. Les clients doivent être
  identiques pour tous les joueurs d'une session.
- **Project Zomboid, Bannerlord (atelier Steam) : rien à garder.** Sources immuables, et
  SteamCMD retélécharge à volonté. **Bannerlord CDN : version consignée, archive non
  gardée** — 4,5 Go par release pour un template de secours.

## Utilisation

Récupérer un fichier sans cloner le dépôt :

```
curl -L -o Server-beta.zip https://raw.githubusercontent.com/Hellm4nbot/amp-mirror/main/rwmt/Server-beta.zip
```

Vérifier une empreinte avant de s'en servir :

```
sha256sum -c <<< "<sha256 du manifeste>  Server-beta.zip"
```

## Mise à jour

Le miroir doit correspondre à ce que le template déploie **aujourd'hui**. Un miroir en
retard est pire qu'aucun : il fait croire à une version qu'on ne peut pas utiliser.

À refaire quand un mod publie un nouveau build, ou quand le commit épinglé dans
`tools/nightcoop_hub_build.sh` change. Les empreintes se vérifient contre le `digest
sha256` que l'API GitHub publie pour chaque asset, jamais contre un simple calcul local.

Branches `main` et `amp3` : même contenu, pour rester aligné sur les deux branches du
dépôt de templates.

Les deux forges servent les mêmes commits : GitHub `Hellm4nbot/amp-mirror`, Codeberg
`hellm4nbot/amp-mirror`. Le dépôt Codeberg est privé à ce jour, donc seul GitHub est
téléchargeable sans authentification.
