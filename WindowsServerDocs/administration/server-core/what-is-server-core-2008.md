---
title: Qu’est-ce que Server Core 2008 ?
description: En savoir plus sur l’option d’installation Server Core dans Windows Server 2008
ms.prod: windows-server
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: 0a109d0bfc4fc09b5e8097059d68b728d17752a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383383"
---
# <a name="what-is-server-core-2008"></a>Qu’est-ce que Server Core 2008 ?
>S'applique à : Windows Server 2008

>[!NOTE]
>Ces informations s’appliquent à Windows Server 2008. Pour plus d’informations sur Server Core dans Windows Server, consultez [la page qu’est-ce que l’installation minimale de Windows Server](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core). 

L’option Server Core est une nouvelle option d’installation minimale qui est disponible lors du déploiement de l’édition standard, Enterprise ou Datacenter de Windows Server 2008. Server Core vous offre une installation minimale de Windows Server 2008 qui prend en charge l’installation de certains rôles de serveur, comme décrit plus loin dans ce chapitre. Comparez ceci avec l’option d’installation complète de Windows Server 2008, qui prend en charge l’installation de tous les rôles de serveur disponibles, ainsi que d’autres applications serveur Microsoft ou tierces, telles que Microsoft Exchange Server ou SAP. 

Avant d’aller plus loin, l’expression « option d’installation » doit être expliquée. En règle générale, lorsque vous achetez une copie de Windows Server 2008, vous achetez une licence d’utilisation de certaines éditions ou de vos unités de conservation de stock (SKU). Le tableau 1-1 répertorie les différentes éditions de Windows Server 2008 disponibles. Le tableau indique également les options d’installation (Full, Server Core, ou les deux) qui sont disponibles pour chaque édition.

**Tableau 1-1** Éditions de Windows Server 2008 et leur prise en charge des options d’installation

| Édition       | Complet          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 standard (x86 et x64)       | X | X        |
| Windows Server 2008 entreprise (x86 et x64)       | X | X        |
| Windows Server 2008 Datacenter (x86 et x64)        | X | X       |
| Windows Web Server 2008 (x86 et x64)       | X | X  |
| Windows Server 2008 pour les systèmes Itanium       | X |     |
| Windows HPC Server 2008 (x64 uniquement)       | X |   |
| Windows Server 2008 standard sans Hyper-V (x86 et x64) | X | X |
| Windows Server 2008 entreprise sans Hyper-V (x86 et x64)  | X | X |
| Windows Server 2008 standard sans Hyper-V (x86 et x64) | X | X |

Pour comprendre ce qu’est une « option d’installation », supposons que vous avez acheté une licence en volume qui vous permet d’installer une copie de Windows Server 2008 Enterprise Edition. Lorsque vous insérez votre support de licence en volume dans un système et commencez le processus d’installation, l’un des écrans que vous verrez, comme illustré dans la figure 1-1, vous propose un choix d’éditions et d’options d’installation.

![Sélection d’une option d’installation minimale pour l’installation](../media/what-is-server-core-2008/FIg1-1.png)

**Figure 1-1** Sélection d’une option d’installation minimale pour l’installation

Dans la figure 1-1, votre licence en volume (ou clé de produit, pour les supports de vente au détail) vous donne deux options d’installation que vous pouvez choisir entre : la deuxième option (une installation complète de Windows Server 2008 Enterprise) et la cinquième option (une installation Server Core de Windows Server 2008 Enterprise), avec les derniers sélectionnés dans cet exemple. 

## <a name="full-vs-server-core"></a>Complète et Server Core 
Depuis les premiers jours de la plate-forme Microsoft Windows, les serveurs Windows étaient essentiellement des serveurs « tout » qui comprenaient tous les types de fonctionnalités, dont vous n’avez peut-être jamais vraiment utilisé dans votre environnement réseau. Par exemple, lorsque vous avez installé Windows Server 2003 sur un système, les fichiers binaires pour le service de routage et d’accès à distance (RRAS) ont été installés sur votre serveur, même si vous n’avez pas besoin de ce service (bien que vous deviez toujours configurer et activer RRAS avant qu’il ne fonctionne). Windows Server 2008 améliore les versions antérieures en installant les fichiers binaires requis par un rôle serveur uniquement si vous choisissez d’installer ce rôle particulier sur votre serveur. Toutefois, l’option d’installation complète de Windows Server 2008 installe toujours de nombreux services et d’autres composants qui ne sont généralement pas nécessaires pour un scénario d’utilisation particulier. 

C’est la raison pour laquelle Microsoft a créé une deuxième option d’installation, Server Core, pour Windows Server 2008 : pour éliminer les services et autres fonctionnalités qui ne sont pas essentiels pour la prise en charge de certains rôles de serveur couramment utilisés. Par exemple, un serveur DNS (Domain Name System) n’a pas vraiment besoin de Windows Internet Explorer, car vous ne voudriez pas naviguer sur le Web à partir d’un serveur DNS pour des raisons de sécurité. Un serveur DNS n’a même pas besoin d’une interface utilisateur graphique, car vous pouvez gérer pratiquement tous les aspects du DNS à partir de la ligne de commande à l’aide de la puissante commande dnscmd. exe, ou à distance à l’aide du composant logiciel enfichable MMC (Microsoft Management Console) DNS.

Pour éviter cela, Microsoft a décidé de supprimer tous les éléments de Windows Server 2008 qui n’étaient pas absolument essentiels pour l’exécution de services réseau principaux comme Active Directory Domain Services (AD DS), DNS, le protocole DHCP (Dynamic Host Configuration Protocol), le fichier et l’impression, et un quelques autres rôles de serveur. Le résultat est la nouvelle option d’installation Server Core, qui peut être utilisée pour créer un serveur qui ne prend en charge qu’un nombre limité de rôles et de fonctionnalités. 

## <a name="the-server-core-gui"></a>Interface graphique utilisateur minimale
Lorsque vous avez terminé l’installation de Server Core sur un système et que vous vous connectez pour la première fois, vous avez un peu de surprise. La figure 1-2 montre l’interface utilisateur Server Core après la première ouverture de session.

![Interface utilisateur Server Core](../media/what-is-server-core-2008/Fig1-2.png)

**Figure 1-2** Interface utilisateur Server Core

Il n’y a pas de bureau ! Autrement dit, il n’y a pas d’interpréteur de commandes Windows Explorer, avec le menu Démarrer, la barre des tâches et les autres fonctionnalités que vous pouvez utiliser. Il vous suffit d’une invite de commandes, ce qui signifie que vous devez effectuer la majeure partie du travail de configuration d’une installation Server Core soit en tapant les commandes une par une, ce qui est lent, soit à l’aide de scripts et de fichiers de commandes, ce qui peut vous aider à accélérer et simplifier votre Configurati sur les tâches en les automatisant. Vous pouvez également effectuer des tâches de configuration initiales à l’aide de fichiers de réponses lorsque vous effectuez une installation sans assistance de Server Core. 

Pour les administrateurs qui sont des experts de l’utilisation d’outils en ligne de commande tels que netsh. exe, Dfscmd. exe et dnscmd. exe, la configuration et la gestion d’une installation Server Core peuvent être simples, voire amusantes. Pour ceux qui ne sont pas des experts, toutefois, tout n’est pas perdu. Vous pouvez toujours utiliser les outils MMC standard de Windows Server 2008 pour gérer une installation Server Core. Vous devez simplement les utiliser sur un autre système exécutant une installation complète de Windows Server 2008 ou Windows Vista avec Service Pack 1. 

Vous en apprendrez davantage sur la configuration et la gestion d’une installation Server Core dans les chapitres 3 à 6 de ce livre, tandis que les chapitres ultérieurs traitent de la gestion de rôles de serveur et d’autres composants spécifiques. Pour en savoir plus sur les différents outils en ligne de commande Windows et sur leur utilisation, vous pouvez consulter deux bonnes ressources :
* La section Référence de commande de la bibliothèque technique Windows Server 2008 () 
* *Le Pocket Consultant de l’administrateur en ligne de commande Windows* de William R. Stanek (Microsoft Press, 2008) 

Le tableau 1-2 répertorie les principales applications GUI, ainsi que leurs exécutables, qui sont disponibles dans une installation Server Core.

**Tableau 1-2** Applications GUI disponibles dans une installation Server Core

| Application GUI | Exécutable avec chemin d’accès |
| -------------   | -------------       | 
| Invite de commandes | %WINDIR%\System32\Cmd.exe |
| Outil de diagnostic Support Microsoft | %WINDIR%\System32\MSdt.exe |
| Bloc-notes | %WINDIR%\System32\Notepad.exe |
| Éditeur du Registre | %WINDIR%\System32\Regedt32.exe |
| Informations système | %WINDIR%\System32\MSinfo32.exe |
| Gestionnaire des tâches | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

C’est une simple liste ! Voici maintenant la liste des éléments d’interface utilisateur qui ne sont pas inclus dans Server Core :
* L’interpréteur de commandes de l’Explorateur Windows (Explorer. exe) et toutes les fonctionnalités de prise en charge, telles que les thèmes 
* Toutes les consoles MMC 
* Tous les utilitaires du panneau de configuration, à l’exception des options régionales et linguistiques (Intl. cpl) et de la date et de l’heure (timedate. cpl) 
* Tous les moteurs de rendu langage HTML (HTML), y compris Internet Explorer et HTML Help 
* Messagerie Windows 
* Lecteur Windows Media 
* La plupart des accessoires, tels que Paint, calculatrice et WordPad

La .NET Framework n’est pas non plus présente dans Server Core, ce qui signifie qu’il n’y a pas de prise en charge de l’exécution de code managé sur une installation Server Core. Seul le code natif (code écrit à l’aide d’interfaces de programmation d’applications (API) Windows) peut s’exécuter sur Server Core. En résumé, toutes les applications GUI qui dépendent de l' .NET Framework ou de l’interpréteur de commandes Explorer. exe ne s’exécutent pas sur Server Core. 

>[!NOTE]
>Windows PowerShell nécessitant le .NET Framework, vous ne pouvez pas installer Windows PowerShell sur Server Core. Toutefois, vous pouvez gérer une installation Server Core à distance à l’aide de Windows PowerShell, tant que vous n’utilisez que des commandes WMI PowerShell.

## <a name="supported-server-roles"></a>Rôles serveur pris en charge 
Une installation Server Core comprend uniquement un nombre limité de rôles de serveur par rapport à une installation complète de Windows Server 2008. Le tableau 1-3 compare les rôles disponibles pour les installations complètes et les installations minimales de Windows Server 2008 Enterprise Edition. 

**Tableau 1-3** Comparaison des rôles serveur pour les installations complètes et minimales de Windows Server 2008 Enterprise Edition

| Rôle serveur  | Disponible en installation complète  | Disponible dans Server Core  |
| ------------- | :-------------: | :------------: |
| Services de certificats Active Directory (AD CS)  | X |  |
| Services de domaine Active Directory (AD DS)  | X  | X |
| Active Directory Federation Services (AD FS)  | X  |  |
| Services AD LDS (Active Directory Lightweight Directory Services)  | X  | X |
| Services AD RMS (Active Directory Rights Management Services)  | X  |  |
| Serveur d’applications  | X  |  |
| Serveur DHCP  | X  | X |
| Serveur DNS  | X  | X |
| Serveur de télécopie  | X  |  |
| Services de fichiers  | X  | X |
| Hyper-V  | X | X |
| Services de stratégie et d'accès réseau  | X  |  |
| Services d’impression  | X  | X |
| Services de diffusion multimédia en continu  | X  | X |
| Services Terminal Server  | X  |  |
| Services UDDI  | X  |  |
| Serveur Web (IIS) | X | X |
| Services de déploiement Windows  | X |  |

Alors que les rôles disponibles pour Server Core sont généralement les mêmes, quelle que soit l’architecture (x86 ou x64) et l’édition du produit, il existe quelques exceptions :
* Le rôle Hyper-V (virtualisation) est disponible uniquement si vous avez acheté Windows Server 2008 avec un support produit Hyper-V (Hyper-V est disponible uniquement pour les versions x64). Si vous n’avez pas besoin de ce rôle, vous pouvez acheter Windows Server 2008 sans le support produit Hyper-V à la place. 
* Le rôle Services de fichiers sur l’édition standard est limité à une racine système de fichiers DFS autonome (DFS) et ne prend pas en charge la réplication entre fichiers (DFS-R). 
* Avant de pouvoir installer le rôle de Media Services de streaming sur Server Core, vous devez télécharger et installer le package autonome Microsoft Update (fichier. msu) approprié pour l’architecture de votre serveur (x86 ou x64) à partir du centre de téléchargement Microsoft.
* Le rôle serveur Web (IIS) ne prend pas en charge ASP.NET. Cela est dû au fait que le .NET Framework n’est pas pris en charge sur Server Core, ce qui limite ce que vous pouvez faire avec un serveur Web Server Core. 

## <a name="supported-optional-features"></a>Fonctionnalités facultatives prises en charge
Une installation Server Core ne prend également en charge qu’un sous-ensemble limité des fonctionnalités disponibles sur une installation complète de Windows Server 2008. Le tableau 1-4 compare les fonctionnalités disponibles pour les installations complètes et minimales de Windows Server 2008 Enterprise Edition.

**Tableau 1-4** Comparaison des fonctionnalités pour les installations complètes et minimales de Windows Server 2008 Enterprise Edition

| Fonctionnalité  | Disponible en installation complète  | Disponible dans Server Core  |
| ------------- | :-------------: | :------------: |
| Fonctionnalités de .NET Framework 3,0  | X  |  |
| Chiffrement de lecteur BitLocker  | X  | X |
| Extensions du serveur BITS  | X  |  |
| Kit d'administration du Gestionnaire des connexions Microsoft  | X |  |
| Expérience utilisateur  | X |  |
| Clustering de basculement  | X  | X |
| Gestion des stratégies de groupe  | X  |  |
| Client d'impression Internet  | X  |  |
| Serveur de noms de stockage Internet  | X  |  |
| Moniteur de port LPR  | X  |  |
| Message Queuing  | X  |  |
| E/s multivoie  | X  | X |
| Équilibrage de la charge réseau  | X  | X |
| Protocole PNRP (Peer Name Resolution Protocol)  | X  |  |
| Expérience audio-vidéo haute qualité Windows  | X |  |
| Assistance à distance  | X  |  |
| Compression différentielle à distance | X  |  |
| Outils d’administration de serveur distant  | X  |  |
| Gestionnaire de stockage amovible | X  | X |
| Proxy RPC sur HTTP | X  |  |
| Services TCP/IP simples  | X  |  |
| Serveur SMTP  | X  |  |
| Services SMNP  | X  | X  |
| Gestionnaire de stockage pour réseau SAN  | X  |  |
| Sous-système pour les applications UNIX | X | X  |
| Client Telnet  | X | X  |
| Serveur Telnet  | X   |  |
| Client TFTP  | X   |  |
| Base de données interne Windows  | X  |  |
| Windows PowerShell  | X  |  |
| Service d’activation de produit Windows  | X   |  |
| Fonctionnalités de Sauvegarde Windows Server  | X  | X  |
| Gestionnaire de ressources système Windows  | X  |  |
| Serveur WINS  | X | X |
| Service de réseau local sans fil | X  |  |

Là encore, il y a quelques points que vous devez connaître concernant les fonctionnalités disponibles sur Server Core :
* Certaines fonctionnalités peuvent nécessiter du matériel spécial pour fonctionner correctement (ou du tout) sur Server Core. Ces fonctionnalités incluent Chiffrement de lecteur BitLocker, le clustering de basculement, les e/s multivoie, l’équilibrage de charge réseau et le stockage amovible. 
* Le clustering de basculement n’est pas disponible dans l’édition standard.

## <a name="server-core-architecture"></a>Architecture Server Core
En examinant plus en détail Server Core, examinons brièvement l’architecture d’une installation Server Core de Windows Server 2008 en la comparant à celle d’une installation complète. Tout d’abord, n’oubliez pas que Server Core n’est pas une version différente de Windows Server 2008, mais simplement une option d’installation que vous pouvez sélectionner lors de l’installation de Windows Server 2008 sur un système. Cela implique ce qui suit :
* Le noyau d’une installation Server Core est le même que celui qui se trouve sur une installation complète de la même architecture matérielle (x86 ou x64) et de l’édition. 
* Si un fichier binaire est présent dans une installation Server Core, une installation complète de la même architecture matérielle (x86 ou x64) et de l’édition possède la même version de ce fichier binaire particulier (avec deux exceptions abordées plus loin). 
* Si un paramètre particulier (par exemple, une exception de pare-feu spécifique ou le type de démarrage d’un service particulier) dispose d’une configuration par défaut sur une installation Server Core, ce paramètre est configuré exactement de la même façon sur une installation complète du même architecture matérielle (x86 ou x64) et édition.

La figure 1-3 montre une vue simplifiée de l’architecture d’une installation complète et d’une installation Server Core de Windows Server 2008. La ligne en pointillés indique l’architecture de Server Core, tandis que le diagramme entier représente l’architecture d’une installation complète. 

Le diagramme illustre l’architecture modulaire de Windows Server 2008, avec Server Core construit sur un sous-ensemble des principales fonctionnalités du système d’exploitation. Pour la même architecture matérielle et la même édition, chaque fichier présent sur une nouvelle installation de Server Core est également présent sur une installation complète, à l’exception de deux fichiers spéciaux (Scregedit. wsf et oclist. exe), qui sont présents uniquement sur Server Core. Ces fichiers spéciaux ont été inclus sur Server Core pour simplifier la configuration initiale d’une installation Server Core et l’ajout ou la suppression de rôles et de composants facultatifs. 

![Les architectures de Server Core et des installations complètes](../media/what-is-server-core-2008/Fig1-3.png)

**Figure 1-3** Les architectures de Server Core et des installations complètes

## <a name="driver-support"></a>Prise en charge des pilotes
Le diagramme architectural de Server Core présenté dans la figure 1-3 est évidemment simplifié. une chose qu’il n’affiche pas est la différence de prise en charge des pilotes de périphériques entre l’installation minimale et l’installation complète. Une installation complète de Windows Server 2008 contient des milliers de pilotes intégrés pour différents types d’appareils, ce qui vous permet d’installer des produits sur une large gamme de configurations matérielles différentes. (Les systèmes d’exploitation clients tels que Windows Vista incluent encore plus de pilotes pour la prise en charge des appareils tels que les appareils photo numériques et les scanneurs qui ne sont normalement pas utilisés avec les serveurs.) 

Si un nouveau périphérique est connecté à (ou installé dans) une installation complète de Windows Server 2008, le sous-système Plug-and-Play (PnP) vérifie d’abord si un pilote intégré pour l’appareil est présent. Si un pilote intégré compatible est trouvé, le sous-système PnP installe automatiquement le pilote et l’appareil fonctionne. Sur une installation complète de Windows Server 2008, une notification contextuelle de la bulle peut s’afficher, indiquant que le pilote a été installé et que l’appareil est prêt à être utilisé. 

Sur une installation Server Core, le processus d’installation du pilote est le même (le sous-système PnP est présent sur Server Core) avec deux qualifications. Tout d’abord, Server Core comprend uniquement un nombre minimal de pilotes intégrés et uniquement pour les types d’appareils suivants :
* Pilote vidéo standard Video Graphics Array (VGA) 
* Pilotes pour les périphériques de stockage 
* Pilotes pour cartes réseau

Notez que pour chacune des trois catégories d’appareils présentées ici, Server Core comprend les mêmes pilotes intégrés que ceux qui se trouvent dans une installation complète correspondante (pour la même architecture matérielle). 

En outre, lorsque le sous-système PnP installe automatiquement un pilote pour un nouvel appareil, il le fait en mode silencieux ; aucune notification de la fenêtre contextuelle ne s’affiche. Pourquoi pas? Étant donné qu’il n’y a aucune interface utilisateur graphique sur Server Core, il n’y a pas de barre des tâches. il n’y a donc pas de zone de notification 

Que faire lorsque vous ajoutez le rôle Services d’impression à une installation minimale et que vous souhaitez installer une imprimante ? Vous ajoutez le pilote d’imprimante manuellement au serveur : Server Core n’a pas de pilotes d’impression intégrés.

## <a name="service-footprint"></a>Empreinte de service
Étant donné que Server Core est une installation minimale, son encombrement est plus faible par rapport à une installation complète correspondante de la même architecture matérielle et de l’édition. Par exemple, environ 75 services système sont installés par défaut sur une installation complète de Windows Server 2008, dont environ 50 sont configurés pour le démarrage automatique. En revanche, Server Core n’a que des 70 services installés par défaut, et moins de 40 au démarrage automatique. 

Le tableau 1-5 répertorie les services installés par défaut sur une installation Server Core, avec le mode de démarrage pour et le compte utilisé par chaque service.

**Tableau 1-5** Services système installés par défaut sur Server Core

| Nom du service  | Display name  | Mode de démarrage  | Compte  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | Expérience de l’application  | Auto | LocalSystem |
| AppMgmt  | Gestion des applications  | Manuel | LocalSystem |
| BFE | Moteur de filtrage de base  | Auto | Local |
| BITS | Service de transfert intelligent en arrière-plan (BITS)  | Auto | LocalSystem |
| Visiteur | Explorateur d’ordinateurs  | Manuel | LocalSystem |
| CertPropSvc | Propagation du certificat  | Manuel | LocalSystem |
| COMSysApp  | Application système COM+  | Manuel | LocalSystem |
| CryptSvc  | Services de chiffrement  | Auto | Service réseau |
| DcomLaunch  | Lanceur de processus serveur DCOM  | Auto | LocalSystem |
| Dhcp  | Client DHCP  | Auto | Local |
| Dnscache | Client DNS  | Auto | Service réseau |
| DPS  | Service de stratégie de diagnostic  | Auto | Local |
| Événements | Journal des événements Windows  | Auto | Local |
| EventSystem  | Système d’événement COM+  | Auto | Local |
| FCRegSvc  | Service d’inscription de la plateforme Microsoft Fibre Channel  | Manuel | Local |
| gpsvc  | Client de stratégie de groupe  | Auto | LocalSystem |
| hidserv | Accès au périphérique de l’interface utilisateur  | Manuel | LocalSystem |
| hkmsvc  | Gestion des certificats et des clés d’intégrité  | Manuel | LocalSystem |
| IKEEXT  | Modules de génération de clés IKE et AuthIP  | Auto | LocalSystem |
| iphlpsvc  | Assistance IP  | Auto | LocalSystem |
| KeyIso | Isolation de clé CNG  | Manuel | LocalSystem |
| KtmRm  | Service KtmRm pour Distributed Transaction Coordinator  | Auto | Service réseau |
| LanmanServer  | Server  | Auto | LocalSystem |
| LanmanWorkstation  | Station de travail  | Auto | Local |
| lltdsvc  | Mappage de découverte de topologie de la couche de liaison  | Manuel | Local |
| lmhosts  | Assistance NetBIOS sur TCP/IP  | Auto | Local |
| MpsSvc  | Pare-feu Windows  | Auto | Local |
| MSDTC  | Distributed Transaction Coordinator  | Auto | Service réseau |
| MSiSCSI  | Service Initiateur iSCSI de Microsoft  | Manuel | LocalSystem |
| msiserver  | Windows Installer  | Manuel | LocalSystem |
| NAPAgent  | Agent de protection d'accès réseau  | Manuel | Service réseau |
| Netlogon  | Netlogon  | Manuel | LocalSystem |
| netprofm  | Service Liste des réseaux  | Auto | Local |
| NlaSvc  | Connaissance des emplacements réseau  | Auto | Service réseau |
| nsi  | Service Interface du magasin réseau  | Auto | Local |
| pla  | Journaux et alertes de performance  | Manuel | Local |
| PlugPlay  | Plug-and-play  | Auto | LocalSystem |
| PolicyAgent  | Agent de stratégie IPSec  | Auto | Service réseau |
| ProfSvc  | Service de profil utilisateur  | Auto | LocalSystem |
| ProtectedStorage  | Stockage protégé  | Manuel | LocalSystem |
| RemoteRegistry  | Accès à distance au Registre  | Auto | Local |
| RpcSs  | Appel de procédure distante (RPC)  | Auto | Service réseau |
| RSoPProv | Fournisseur d’un jeu de stratégie résultant  | Manuel | LocalSystem |
| sacsvr  | Application d’assistance de la Console d’administration spéciale  | Manuel | LocalSystem |
| SamSs  | Gestionnaire de comptes de sécurité  | Auto | LocalSystem |
| SCardSvr | Carte à puce  | Manuel | Local |
| Planification | Planificateur de tâches  | Auto | LocalSystem |
| SCPolicySvc | Stratégie de retrait de la carte à puce  | Manuel | LocalSystem |
| seclogon | Ouverture de session secondaire  | Auto | LocalSystem |
| SENS | Service de notification d’événements système  | Auto | LocalSystem |
| SessionEnv | Configuration des services Terminal Server  | Manuel | LocalSystem |
| slsvc  | Gestion de licences des logiciels | Auto | Service réseau |
| SNMPTRAP  | Interruption SNMP  | Manuel | Local |
| swprv  | Fournisseur de cliché instantané de logiciel Microsoft | Manuel | LocalSystem |
| TBS | Services de base du module de plateforme sécurisée  | Manuel | Local |
| TermService  | Services Terminal Server | Auto | Service réseau |
| TrustedInstaller | Programme d’installation pour les modules Windows  | Auto | LocalSystem |
| UmRdpService | Redirecteur de port du service d’importation des services Terminal Server  | Manuel | LocalSystem |
| vds | Disque virtuel  | Manuel | LocalSystem |
| VSS | Cliché instantané des volumes  | Manuel | LocalSystem |
| W32Time | Horloge Windows  | Auto | Local |
| WcsPlugInService  | Système de couleurs Windows  | Manuel | Local |
| WdiServiceHost  | Service hôte WDIServiceHost  | Manuel | Local |
| WdiSystemHost  | Hôte système de diagnostics  | Manuel | LocalSystem |
| Wecsvc | Collecteur d'événements de Windows  | Manuel | Service réseau |
| WinHttpAuto-ProxySvc  | Service de découverte automatique de Proxy Web pour les services HTTP Windows  | Auto | Local |
| Winmgmt | Windows Management Instrumentation | Auto | LocalSystem |
| WinRM  | Windows Remote Management (WS-Management) | Auto | Service réseau |
| wmiApSrv  | Carte adaptateur de performance WMI  | Manuel | LocalSystem |
| wuauserv | Windows Update | Auto | LocalSystem |