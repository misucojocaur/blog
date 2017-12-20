+++
title = "Skywire and Viscript"
tags = [
    "Development",
    "Skywire",
    "Viscript",
]
bounty = 3
date = "2017-08-15"
categories = [
    "Skywire",
]
+++
## Introduction

### Viscript

[Viscript](https://github.com/skycoin/viscript) est une multiplate-forme CLI («  interface à ligne de commande ») et un lanceur d’applications et enfin pour la gestion des clusters. C’est une banque d'informations sur les signaux, comme un serveur de signal, pouvant ainsi gérer les clients signal, à savoir nœuds, et les composantes sur Skywire. On peut l’exécuter en mode GUI ou en mode « sans tête »

#### Viscript GUI Capture d’écran:

![screenshot](viscript.jpeg)

On peut ajouter des configurations de l’application en fichier config.yaml, à savoir meshnet-socks-server (« réseau maillé-SOCKS-serveur »):

```
  meshnet-socks-server:
    daemon: true
    desc: DESCRIPTION GOES HERE
    path: bin/meshnet/meshnet-run-socks-server
    default_args: []
    help: |
        [1] Text name of app, must be unique
        [2] Node address which app will be talked with. ex 101.202.34.56:9000
        [3] Port which socks server will use for connecting with target host. ex 8000

        Full Example Command:
            start meshnet-socks-server sockssrv0 101.202.34.56:9000 8001
```

Après avoir redémarré Viscript, on peut vérifier les applications qui doivent être démarrées à l’intermédiaire du Viscript par command apps.

Comme on peut le voir sur la capture d'écran on peut démarrer l’application en utilisant le raccourci `s` (`s apptracker 127.0.0.1:20000`)

.

Ensuite Viscript le démarre par une séquence ID unique, on peut ping(`ping`), vérifier l’usage de la ressource(`ru`) et fermer(`sd`) à l’aide de cette ID.

 

### Skywire

[Skywire](https://github.com/skycoin/skywire) est un réseau de pair-à-pair alternative qui enlève le contrôle des FSI pour le redonner aux utilisateurs. Il dispose de plusieurs composantes, Node Manager, les nœuds et les applications sont exécutés sur le réseau maillé comme le client VPN, serveur VPN, client SOCKS, serveur SOCKS, etc.

Toutes les composantes de Skywire reposent sur la banque d'informations sur les signaux tout comme un client signal. Par conséquent elles peuvent être lancées, gérées et fermées à travers Viscript.

## Architecture

#### Schéma de l'architecture

------

 

------

 

```

                                   +-----------+-------------+

           +---------------^-----+ |     VPN   |  SOCKS    |

           |  géré par    |       +-----------+-------------+

           |               <-----+ |          node           |

           v               |       +-------------------------+

                           <-----+ |       node manager      |

+-------------------+      |       +-------------------------+

|      viscript     |      +-----+ |        messenger        |

+-------------------+--------------+-------------------------+

|                        signal                              |

+------------------------------------------------------------+

|                         réseau                                |

+------------------------------------------------------------+

```

 

------

 

Il y a des applications côté client et côté serveur pour chaque service, tel que le client VPN et le serveur VPN. Elles fonctionnent sur le réseau maillé de Skywire. Comme nous le savons, Skycoin est la monnaie de Skywire gagnée le moment où l’utilisateur transfère du trafic et fournit des ressources de réseau. De même, quand l’utilisateur consomme de ressources du réseau ou du matériel médiatique, elle ou il dépense Skycoin. Une fois le comptage et les règlements mis en place, Skywire génèrera les monnaies pour faire opérer le réseau.

Node, Node Manager et Messenger sont les composantes clés du réseau maillé Skywire. Node est un nœud maillé de pair-à-pair. Les applications de service accèderont à Node et leur trafic sera transféré par Node. Node Manager gère les itinéraires entre les nœuds du réseau maillé. Messenger permet aux utilisateurs se regrouper à l’aide de clés publiques. Ils sont les fondements du réseau maillé de Skywire.

## Résumé

Viscript et Skywire sont encore en cours de développement. Toutefois des étapes importantes ont été franchies dans l’écosystème Skycoin. De plus on profite et on libérera le plein potentiel d’un Internet indépendant à venir !

#### Sky-Messenger capture d’écran:

![screenshot](messenger.png)
