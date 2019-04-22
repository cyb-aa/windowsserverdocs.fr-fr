---
title: Nouveautés de Server Core 2008 ?
description: En savoir plus sur l’option d’installation Server Core de Windows Server 2008
ms.prod: windows-server-threshold
ms.author: helohr
ms.date: 11/01/2017
ms.topic: article
author: Heidilohr
ms.openlocfilehash: c1ef71dbc589cfdeac63b46d720c4bdd0a44dbaa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815400"
---
#<a name="what-is-server-core-2008"></a>Nouveautés de Server Core 2008 ?
>S'applique à : Windows Server 2008

>[!NOTE]
>Ces informations s’appliquent à Windows Server 2008. Pour plus d’informations sur Server Core dans Windows Server, consultez [What ' s l’Installation minimale de Windows Server](https://docs.microsoft.com/windows-server/administration/server-core/what-is-server-core). 

L’option Server Core est une nouvelle option d’installation minimale qui est disponible lorsque vous déployez l’Édition Standard, Enterprise ou Datacenter de Windows Server 2008. Server Core vous offre une installation minimale de Windows Server 2008 qui prend en charge l’installation uniquement certains rôles de serveur, comme décrit plus loin dans ce chapitre. Comparez ceci avec l’option d’installation complète pour Windows Server 2008, qui prend en charge l’installation de tous les rôles de serveur disponibles et également autres Microsoft ou un serveur tiers applications, telles que Microsoft Exchange Server ou SAP. 

Avant d’aller plus loin, l’expression « option d’installation » doit être expliquées. Normalement, lorsque vous achetez une copie de Windows Server 2008, vous achetez une licence d’utilisation de certaines éditions ou les unités de stock (SKU). Tableau 1-1 répertorie les différentes éditions de Windows Server 2008 qui sont disponibles. Le tableau indique également les options d’installation (complète, Server Core ou les deux) sont disponibles pour chaque édition.

**Tableau 1-1** éditions de Windows Server 2008 et leur prise en charge des options d’installation
| Édition       | Complet          | Server Core  |
| ------------- | :-------------: | :------------: |
| Windows Server 2008 Standard (x86 et x64)       | X | X        |
| Windows Server 2008 Enterprise (x86 et x64)       | X | X        |
| Windows Server 2008 Datacenter (x86 et x64)        | X | X       |
| Windows Web Server 2008 (x86 et x64)       | X | X  |
| Windows Server 2008 For-processeur Itanium       | X |     |
| Windows HPC Server 2008 (x64 uniquement)       | X |   |
| Windows Server 2008 Standard sans Hyper-V (x86 et x64) | X | X |
| Windows Server 2008 entreprise sans Hyper-V (x86 et x64)  | X | X |
| Windows Server 2008 Standard sans Hyper-V (x86 et x64) | X | X |

Pour comprendre ce qu’est une option d’installation « » est, supposons que vous avez acheté une licence en volume qui vous permet d’installer une copie de Windows Server 2008 Enterprise Edition. Lorsque vous insérez votre média avec licence en volume dans un système et commencez le processus d’installation, un des écrans que vous le verrez, comme illustré dans la Figure 1-1, vous propose un choix des éditions et options d’installation.

![En sélectionnant une option d’installation Server Core à installer](../media/what-is-server-core-2008/FIg1-1.png)

**Figure 1-1** en sélectionnant une option d’installation Server Core à installer

Dans la Figure 1-1, votre licence en volume (ou clé de produit, de média version commerciale) vous offre deux options d’installation que vous pouvez choisir entre : la deuxième option (un total Installation de Windows Server 2008 Enterprise) et l’option cinquième (un Server Core Installation de Windows Server 2008 Enterprise), le dernier étant sélectionné dans cet exemple. 

##<a name="full-vs-server-core"></a>Visual Studio complet. Server Core 
Depuis le début de la plate-forme Microsoft Windows, les serveurs Windows ont été essentiellement « tout » serveurs incluant tous les types de fonctionnalités, certains d'entre eux vous utiliseriez en réalité jamais dans votre environnement réseau. Par exemple, lorsque vous avez installé Windows Server 2003 sur un système, les fichiers binaires pour le routage et accès distant (RRAS) ont été installés sur votre serveur même si vous n’aviez aucun besoin de ce service (bien que vous deviez toujours configurer et activer RRAS avant elle fonctionnerait). Windows Server 2008 améliore les versions antérieures en installant les fichiers binaires requis par un rôle de serveur uniquement si vous choisissez d’installer ce rôle particulier sur votre serveur. Toutefois, l’option d’installation complète de Windows Server 2008 installe toujours de nombreux services et autres composants qui ne sont généralement pas nécessaires pour un scénario d’utilisation particulier. 

C’est pourquoi Microsoft a créé une deuxième option d’installation, Server Core, pour Windows Server 2008 : pour éliminer tous les services et autres fonctionnalités qui ne sont pas indispensables pour la prise en charge de certains les rôles de serveur couramment utilisées. Par exemple, un serveur de système DNS (Domain Name) vraiment n’a pas besoin installé dessus, car vous ne voudriez parcourir le Web à partir d’un serveur DNS pour des raisons de sécurité de Windows Internet Explorer. Et un serveur DNS n’a même pas besoin une interface utilisateur graphique (GUI), étant donné que vous pouvez gérer pratiquement tous les aspects du serveur DNS à partir de la ligne de commande à l’aide de la commande Dnscmd.exe puissante, ou à distance à l’aide de la console DNS Microsoft Management Console (MMC)-composant logiciel enfichable.

Pour éviter ce problème, Microsoft a décidé de supprimer tous les éléments à partir de Windows Server 2008 qui n’était pas absolument indispensable pour l’exécution de services réseau de base telles que les Services de domaine Active Directory (AD DS), DNS, DHCP Dynamic Host Configuration Protocol (), fichier et impression, et un quelques autres rôles de serveur. Le résultat est la nouvelle option d’installation Server Core, qui peut être utilisée pour créer un serveur qui prend en charge uniquement un nombre limité de rôles et fonctionnalités. 

##<a name="the-server-core-gui"></a>L’interface utilisateur graphique de Server Core
Lorsque vous avez terminé l’installation Server Core sur un système et l’ouverture de session pour la première fois, vous l’avez un peu surprise. Figure 1-2 montre l’interface utilisateur de Server Core après la première ouverture de session.

![Interface utilisateur Server Core](../media/what-is-server-core-2008/Fig1-2.png)

**Figure 1-2** interface utilisateur de Server Core

Il n’existe aucun Bureau ! Autrement dit, il n’existe aucun shell de l’Explorateur Windows, avec les autres fonctionnalités que vous avez l’habitude de voir son menu Démarrer et barre des tâches. Il vous est une invite de commandes, ce qui signifie que vous devez effectuer la plupart du travail de configuration d’une installation Server Core soit en tapant les commandes une à la fois, qui est lente, ou à l’aide de scripts et fichiers de commandes, qui peuvent vous aider à accélérer et simplifier votre configurati sur les tâches en automatisant les. Vous pouvez également effectuer certaines tâches de configuration initiales à l’aide de fichiers de réponses lorsque vous effectuez une installation sans assistance de Server Core. 

Pour les administrateurs qui sont des experts dans l’aide des outils de ligne de commande Netsh.exe, Dfscmd.exe et Dnscmd.exe, configuration et gestion d’une installation Server Core peuvent être simple et même amusant. Pour ceux qui ne sont pas des experts, toutefois, tout n’est pas perdu. Vous pouvez toujours utiliser les outils MMC de Windows Server 2008 standard pour la gestion d’une installation Server Core. Vous devez simplement les utiliser sur un autre système en cours d’exécution soit une installation complète de Windows Server 2008 ou Windows Vista avec Service Pack 1. 

Vous en apprendrez davantage sur la configuration et gestion d’une installation de Server Core dans les chapitres 3 à 6 de ce livre, alors que les chapitres comment gérer les rôles de serveur spécifiques et d’autres composants. Pour en savoir plus sur les différents outils de ligne de commande Windows et comment les utiliser, il existe deux bonnes ressources à consulter :
* La section de référence des commandes de la bibliothèque technique Windows Server 2008 () 
* *Le Windows en ligne de commande Administrator's Pocket Consultant* par William R. Stanek (Microsoft Press, 2008) 

Le tableau 1-2 répertorie les applications principales de l’interface graphique utilisateur, ainsi que leurs exécutables qui sont disponibles dans une installation Server Core.

**Tableau 1-2** applications GUI disponibles dans une installation Server Core
| Application de l’interface graphique utilisateur | Fichier exécutable avec le chemin d’accès |
| -------------   | -------------       | 
| Invite de commandes | %WINDIR%\System32\Cmd.exe |
| Outil de Diagnostic de Support Microsoft | %WINDIR%\System32\MSdt.exe |
| Bloc-notes | %WINDIR%\System32\Notepad.exe |
| Éditeur du Registre | %WINDIR%\System32\Regedt32.exe |
| Informations système | %WINDIR%\System32\MSinfo32.exe |
| Gestionnaire des tâches | %WINDIR%\System32\Taskmgr.exe |
| Windows Installer | %WINDIR%\System32\MSiexec.exe |

C’est une très courte liste ! Voici maintenant une liste des éléments d’interface utilisateur qui ne sont pas inclus dans Server Core :
* Le shell de bureau de l’Explorateur Windows (Explorer.exe) et des fonctionnalités de prise en charge tels que les thèmes 
* Toutes les consoles MMC 
* Tous les utilitaires du Panneau de configuration, à l’exception des Options régionales et linguistiques (Intl.cpl) et la Date et heure (Timedate.cpl) 
* Tous les moteurs de rendu de langage HTML (Hypertext Markup), y compris Internet Explorer et l’aide HTML 
* Windows Mail 
* Lecteur Windows Media 
* La plupart des accessoires tels que Paint, Calculatrice et Wordpad

Le .NET Framework n’est pas également présent dans Server Core, ce qui signifie qu’il n’existe aucune prise en charge pour le code managé en cours d’exécution sur une installation Server Core. Que le code natif, le code écrit à l’aide des interfaces de programmation d’applications (API) Windows, peuvent s’exécuter sur Server Core. En résumé, toutes les applications de l’interface graphique utilisateur qui soit dépendent du .NET Framework ou sur le Explorer.exe shell ne s’exécute sur Server Core. 

>[!NOTE]
>Étant donné que Windows PowerShell nécessite le .NET Framework, vous ne pouvez pas installer PowerShell de Windows sur Server Core. Vous pouvez, toutefois, gérer une installation Server Core à distance en utilisant Windows PowerShell tant que vous utilisez uniquement les commandes PowerShell WMI.

##<a name="supported-server-roles"></a>Rôles de serveur pris en charge 
Une installation Server Core inclut uniquement un nombre limité de rôles de serveur par rapport à une installation complète de Windows Server 2008. Tableau 1-3 compare les rôles disponibles pour les installations complète et minimale de Windows Server 2008 Enterprise Edition. 

**Tableau 1-3** comparaison des rôles de serveur pour les installations complète et minimale de Windows Server 2008 Enterprise Edition
| Rôle serveur  | Disponible dans une installation complète  | Disponibles dans Server Core  |
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

Si les rôles disponibles pour Server Core sont généralement les mêmes, quel que soit l’architecture (x64 ou x86) et l’édition du produit, il existe quelques exceptions près :
* Le rôle Hyper-V (virtualisation) est disponible uniquement si vous avez acheté Windows Server 2008 avec le support du produit Hyper-V (Hyper-V est disponible uniquement pour x64 versions). Si vous n’avez pas besoin de ce rôle, vous pouvez acheter Windows Server 2008 sans support du produit Hyper-V à la place. 
* Le rôle Services de fichiers sur l’Édition Standard est limité à une racine de système de fichiers distribués (DFS) autonomes et ne prend pas en charge la réplication (DFS-R). 
* Avant d’installer le rôle Services de diffusion multimédia sur Server Core, vous devez télécharger et installer l’approprié Package autonome Microsoft (fichier .msu) pour l’architecture de votre serveur (x64 ou x86) à partir du Microsoft Download Center.
* Le rôle de serveur Web (IIS) ne prend pas en charge ASP.NET. Il s’agit, car le .NET Framework n’est pas pris en charge sur Server Core, ce qui limite ce que vous pouvez faire avec un serveur Web de Server Core. 

##<a name="supported-optional-features"></a>Prise en charge des fonctionnalités facultatives
Une installation Server Core prend également en charge uniquement un sous-ensemble limité des fonctionnalités disponibles sur une installation complète de Windows Server 2008. Tableau 1-4 compare les fonctionnalités disponibles pour les installations complète et minimale de Windows Server 2008 Enterprise Edition.

**Tableau 1-4** comparaison des fonctionnalités pour les installations complète et minimale de Windows Server 2008 Enterprise Edition

| Fonctionnalité  | Disponible dans une installation complète  | Disponibles dans Server Core  |
| ------------- | :-------------: | :------------: |
| Fonctionnalités de .NET framework 3.0  | X  |  |
| Chiffrement de lecteur BitLocker  | X  | X |
| Extensions du serveur BITS  | X  |  |
| Kit d'administration du Gestionnaire des connexions Microsoft  | X |  |
| Expérience utilisateur  | X |  |
| Clustering de basculement  | X  | X |
| Gestion des stratégies de groupe  | X  |  |
| Client d'impression Internet  | X  |  |
| Serveur  | X  |  |
| Moniteur de port LPR  | X  |  |
| Message Queuing  | X  |  |
| E/s à chemins multiples  | X  | X |
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
| Service d’Activation de produit Windows  | X   |  |
| Fonctionnalités de sauvegarde de Windows Server  | X  | X  |
| Gestionnaire de ressources système Windows  | X  |  |
| Serveur WINS  | X | X |
| Service de réseau local sans fil | X  |  |

Là encore, il existe quelques points que vous devez savoir sur concernant les fonctionnalités disponibles sur Server Core :
* Certaines fonctionnalités peuvent nécessiter un matériel spécial pour fonctionner correctement (ou tout) sur Server Core. Ces fonctionnalités incluent le chiffrement de lecteur BitLocker, de Clustering de basculement, d’e/s Multipath i/o, de l’équilibrage de charge réseau et de stockage amovible. 
* Le Clustering de basculement n’est pas disponible sur l’Édition Standard.

##<a name="server-core-architecture"></a>Architecture de Server Core
Un examen approfondi des Server Core, nous allons examiner brièvement l’architecture d’une installation Server Core de Windows Server 2008 en le comparant avec celui d’une installation complète. Tout d’abord, n’oubliez pas que Server Core n’est pas une version différente de Windows Server 2008, mais simplement une option d’installation que vous pouvez sélectionner lors de l’installation de Windows Server 2008 sur un système. Cela implique ce qui suit :
* Le noyau sur une installation Server Core est le même que celui trouvé sur une installation complète de la même architecture de matériel (x64 ou x86) et l’édition. 
* Si un fichier binaire est présent sur une installation Server Core, une installation complète de la même architecture matérielle (x64 ou x86) et l’édition a la même version de ce fichier binaire particulier (avec deux exceptions abordées plus tard). 
* Si un paramètre particulier (par exemple, une exception de pare-feu spécifiques ou le type de démarrage d’un service particulier) a une certaine configuration par défaut sur une installation Server Core, ce paramètre est configuré exactement de la même façon sur une installation complète de la même architecture matérielle (x64 ou x86) et l’édition.

Figure 1-3 montre une vue simplifiée de l’architecture d’une installation complète et une installation Server Core de Windows Server 2008. La ligne en pointillés indique l’architecture de Server Core, tandis que l’ensemble du diagramme représente l’architecture d’une installation complète. 

Le diagramme illustre l’architecture modulaire de Windows Server 2008, qui est construit sur un sous-ensemble des principales fonctionnalités de système d’exploitation de Server Core. Pour la même architecture matérielle et Édition, chaque fichier présent sur une nouvelle installation de Server Core est également présent sur une installation complète, à l’exception des deux fichiers spéciaux (Scregedit.wsf et Oclist.exe), qui sont présentes uniquement sur Server Core. Ces fichiers spéciaux ont été inclus sur Server Core pour simplifier la configuration initiale d’une installation Server Core et l’ajout ou suppression de rôles et composants optionnels. 

![Les architectures d’installations complète et minimale](../media/what-is-server-core-2008/Fig1-3.png)

**Figure 1-3** les architectures d’installations complète et minimale

##<a name="driver-support"></a>Prise en charge du pilote
Le diagramme architectural de Server Core dans la Figure 1-3 est simplifié à l’évidence ; une chose qu’il n’affiche pas est la différence dans les pilotes de périphérique pris en charge entre les installations complètes et minimale. Une installation complète de Windows Server 2008 contient des milliers de pilotes pour différents types d’appareils, qui vous permettent d’installer des produits sur un large éventail de différentes configurations matérielles. (Les systèmes d’exploitation clients tels que Windows Vista incluent d’autres pilotes pour prendre en charge des périphériques tels que les appareils photo numériques et des scanneurs qui ne sont normalement pas utilisés avec les serveurs). 

Si un nouveau périphérique est connecté (ou installé dans) une installation complète de Windows Server 2008, le sous-système de Plug-and-Play (PnP) vérifie d’abord si un pilote d’origine pour l’appareil est présent. Si un pilote d’origine compatible est trouvé, le sous-système Plug and Play installe le pilote et l’appareil puis s’exécute automatiquement. Sur une installation complète de Windows Server 2008, une notification de fenêtre contextuelle d’info-bulle peut s’afficher, indiquant que le pilote n’a pas été installé et que l’appareil est prêt à être utilisé. 

Sur une installation Server Core, le processus d’installation de pilote est la même (le sous-système Plug and Play est présent sur Server Core) avec les deux qualifications. Tout d’abord, Server Core inclut uniquement un nombre minimal de pilotes et uniquement pour les types d’appareils suivants :
* Un pilote vidéo vidéo graphique tableau VGA standard 
* Pilotes de périphériques de stockage 
* Pilotes de cartes réseau

Notez que, pour chacune des catégories de trois appareils illustrés ici, Server Core inclut les mêmes pilotes d’origine qui sont trouvent sur une installation complète correspondante (pour la même architecture matérielle). 

En outre, lorsque le sous-système Plug and Play installe automatiquement un pilote pour un nouvel appareil, il le fait en mode silencieux, aucune notification de fenêtre contextuelle d’info-bulle ne s’affiche. Pourquoi pas ? Il n’existe aucun GUI sur Server Core étant aucune barre des tâches, il n’existe aucune zone de notification sur la barre des tâches ! 

Par conséquent, que faire lorsque vous ajoutez le rôle Services d’impression pour une installation Server Core et que vous souhaitez installer une imprimante ? Vous ajoutez manuellement le pilote d’imprimante sur le serveur, Server Core n’a aucun pilote d’impression l’emploi.

##<a name="service-footprint"></a>Encombrement de service
Étant donné que Server Core est une installation minimale, il a un encombrement de service système qu’une installation complète correspondante de la même architecture matérielle et l’édition. Par exemple, environ 75 services système sont installés par défaut sur une installation complète de Windows Server 2008, dont environ 50 sont configurés pour un démarrage automatique. En revanche, Server Core a uniquement environ 70 services installés par défaut et moins de 40 de ces démarrer automatiquement. 

Tableau 1-5 répertorie les services qui sont installés par défaut sur une installation Server Core, avec le mode de démarrage et le compte utilisé par chaque service.

**Tableau 1-5** services système installés par défaut sur Server Core

| Nom du service  | Nom d’affichage  | Mode de démarrage  | Compte  |
| ------------- | ------------- | ------------ | ------------ |
| AeLookupSvc  | Expérience d’application  | Auto | LocalSystem |
| AppMgmt  | Gestion des applications  | Manuelle | LocalSystem |
| BFE | Moteur de filtrage de base  | Auto | LocalService |
| BITS | Service de transfert intelligent en arrière-plan (BITS)  | Auto | LocalSystem |
| Navigateur | Explorateur d’ordinateurs  | Manuelle | LocalSystem |
| CertPropSvc | Propagation de certificat  | Manuelle | LocalSystem |
| COMSysApp  | Application système COM +  | Manuelle | LocalSystem |
| CryptSvc  | Services de chiffrement  | Auto | Service de réseau |
| DcomLaunch  | Lanceur de processus DCOM  | Auto | LocalSystem |
| Dhcp  | Client DHCP  | Auto | LocalService |
| Dnscache | Client DNS  | Auto | Service de réseau |
| DPS  | Service de stratégie de diagnostic  | Auto | LocalService |
| Journal des événements | Journal des événements Windows  | Auto | LocalService |
| EventSystem  | Système d’événement COM +  | Auto | LocalService |
| FCRegSvc  | Service d’inscription de plateforme Microsoft Fibre Channel  | Manuelle | LocalService |
| gpsvc  | Client de stratégie de groupe  | Auto | LocalSystem |
| hidserv | Accès des appareils Interface humaine  | Manuelle | LocalSystem |
| hkmsvc  | Clé de l’intégrité et la gestion des certificats  | Manuelle | LocalSystem |
| IKEEXT  | Modules de génération de clés IKE et AuthIP  | Auto | LocalSystem |
| iphlpsvc  | Assistance IP  | Auto | LocalSystem |
| KeyIso | Isolation de clé CNG  | Manuelle | LocalSystem |
| KtmRm  | Service KtmRm pour Distributed Transaction Coordinator  | Auto | Service de réseau |
| LanmanServer  | Server  | Auto | LocalSystem |
| LanmanWorkstation  | Workstatione  | Auto | LocalService |
| lltdsvc  | Mappeur de détection de topologie de couche de liaison  | Manuelle | LocalService |
| lmhosts  | Assistance TCP/IP NetBIOS  | Auto | LocalService |
| MpsSvc  | Pare-feu Windows  | Auto | LocalService |
| MSDTC  | Distributed Transaction Coordinator  | Auto | Service de réseau |
| MSiSCSI  | Service initiateur Microsoft iSCSI  | Manuelle | LocalSystem |
| MSIServer  | Windows Installer  | Manuelle | LocalSystem |
| NAPAgent  | Agent de protection d'accès réseau  | Manuelle | Service de réseau |
| Netlogon  | Netlogon  | Manuelle | LocalSystem |
| netprofm  | Service liste des réseaux  | Auto | LocalService |
| NlaSvc  | Connaissance des emplacements réseau  | Auto | Service de réseau |
| nsi  | Service d’Interface réseau Store  | Auto | LocalService |
| Pla  | Alertes et les journaux de performances  | Manuelle | LocalService |
| PlugPlay  | Plug-and-Play  | Auto | LocalSystem |
| PolicyAgent  | Agent de stratégie IPSec  | Auto | Service de réseau |
| ProfSvc  | Service de profil utilisateur  | Auto | LocalSystem |
| ProtectedStorage  | Stockage protégé  | Manuelle | LocalSystem |
| RemoteRegistry  | Accès à distance au Registre  | Auto | LocalService |
| RpcSs  | Appel de procédure distante (RPC)  | Auto | Service de réseau |
| RSoPProv | Jeu de fournisseur de stratégie résultant  | Manuelle | LocalSystem |
| sacsvr  | Application d’assistance de la Console Administration spéciale  | Manuelle | LocalSystem |
| SamSs  | Gestionnaire de comptes de sécurité  | Auto | LocalSystem |
| SCardSvr | Carte à puce  | Manuelle | LocalService |
| Planification | Planificateur de tâches  | Auto | LocalSystem |
| SCPolicySvc | Stratégie de suppression de carte à puce  | Manuelle | LocalSystem |
| Seclogon | Ouverture de session secondaire  | Auto | LocalSystem |
| SENS | Service de Notification d’événement système  | Auto | LocalSystem |
| SessionEnv | Configuration des services Terminal Server  | Manuelle | LocalSystem |
| slsvc  | Gestion de licences des logiciels | Auto | Service de réseau |
| SNMPTRAP  | Interruption SNMP  | Manuelle | LocalService |
| swprv  | Fournisseur de clichés instantanés logiciels Microsoft | Manuelle | LocalSystem |
| TBS | Services de Base de module de plateforme sécurisée  | Manuelle | LocalService |
| TermService  | Services Terminal Server | Auto | Service de réseau |
| TrustedInstaller | Programme d’installation des Modules de Windows  | Auto | LocalSystem |
| UmRdpService | Redirecteur de Port du mode utilisateur des Services Terminal Server  | Manuelle | LocalSystem |
| vds | Disque virtuel  | Manuelle | LocalSystem |
| VSS | Cliché instantané de volume  | Manuelle | LocalSystem |
| W32Time | Horloge Windows  | Auto | LocalService |
| WcsPlugInService  | Système de couleurs Windows  | Manuelle | LocalService |
| WdiServiceHost  | Hôte de Service de diagnostic  | Manuelle | LocalService |
| WdiSystemHost  | Hôte de système de diagnostic  | Manuelle | LocalSystem |
| Wecsvc | Collecteur d'événements de Windows  | Manuelle | Service de réseau |
| WinHttpAuto-ProxySvc  | Service de découverte automatique de Proxy Web de WinHTTP  | Auto | LocalService |
| WinMgmt | Windows Management Instrumentation | Auto | LocalSystem |
| WinRM  | Gestion à distance de Windows (WS-Management) | Auto | Service de réseau |
| wmiApSrv  | Carte de Performance WMI  | Manuelle | LocalSystem |
| wuauserv | Windows Update | Auto | LocalSystem |