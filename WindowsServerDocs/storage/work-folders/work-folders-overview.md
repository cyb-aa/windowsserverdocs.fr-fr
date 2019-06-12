---
ms.assetid: c91c7196-ee0d-4856-8cfb-4c38494ccf1f
title: Vue d’ensemble de Dossiers de travail
ms.prod: windows-server-threshold
ms.technology: storage-work-folders
ms.topic: article
author: JasonGerend
manager: dougkim
ms.author: jgerend
ms.date: 06/07/2019
description: 'Vue d’ensemble de Dossiers de travail : rôle de serveur dans Windows Server qui fournit un moyen cohérent pour les utilisateurs d’accéder aux fichiers de travail à partir des PC et appareils.'
ms.openlocfilehash: 1313c49982cb85b5cce1e9a442ff0c622c6be272
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812568"
---
# <a name="work-folders-overview"></a>Vue d’ensemble de Dossiers de travail

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows 10, Windows 8.1, Windows 7

Cette rubrique concerne Dossiers de travail, un service de rôle pour les serveurs de fichiers exécutant Windows Server qui fournit un moyen cohérent pour les utilisateurs d'accéder à leurs fichiers de travail à partir de leurs PC et appareils.  
  
Si vous avez besoin pour télécharger ou utiliser des dossiers de travail sur Windows 10, Windows 7 ou un appareil Android ou iOS, consultez les rubriques suivantes :

- [Dossiers de travail pour Windows 10](https://support.microsoft.com/help/12370/windows-10-work-folders)
- [Dossiers de travail pour Windows 7 (téléchargement 64 bits)](https://www.microsoft.com/download/details.aspx?id=42558)
- [Dossiers de travail pour Windows 7 (téléchargement de 32 bits)](https://www.microsoft.com/download/details.aspx?id=42559)
- [Dossiers de travail pour iOS](https://itunes.apple.com/app/work-folders/id950878067)
- [Dossiers de travail pour Android](https://play.google.com/store/apps/details?id=com.microsoft.workfolders)

## <a name="role-description"></a>Description de rôle

 Les dossiers de travail permettent aux utilisateurs de stocker les fichiers de travail et d’y accéder sur leurs ordinateurs et appareils personnels (un concept souvent appelé « BYOD », ou « Apportez votre propre appareil »), en plus des PC de l’entreprise. Les utilisateurs bénéficient ainsi d’un emplacement pratique pour stocker les fichiers de travail et y accéder en tout lieu. Les organisations gardent la mainmise sur les données d’entreprise en stockant les fichiers sur des serveurs de fichiers gérés de manière centralisée et en spécifiant éventuellement des stratégies de périphérique utilisateur (par exemple, mots de passe de chiffrement et d’écran de verrouillage).  
  
 Dossiers de travail peut être déployé avec des déploiements existants des dossiers Redirection de dossiers, Fichiers hors connexion et de base. Dossiers de travail stocke les fichiers de l’utilisateur dans un dossier sur le serveur appelé *partage de synchronisation*. Vous pouvez spécifier un dossier qui contient déjà des données utilisateur, ce qui vous permet d’adopter Dossiers de travail sans migrer de serveurs ni de données et sans supprimer immédiatement votre solution existante.  
  
## <a name="practical-applications"></a>Cas pratiques

 Les administrateurs peuvent utiliser Dossiers de travail pour fournir aux utilisateurs un accès à leurs fichiers de travail, tout en conservant le stockage centralisé et en contrôlant les données de l’organisation. Exemples de certains cas pratiques spécifiques pour Dossiers de travail :  
  
-   Fournir un point d’accès unique aux fichiers de travail à partir des appareils et ordinateurs personnels et professionnels d’un utilisateur  
  
-   Accéder aux fichiers de travail en mode hors connexion, puis synchroniser avec le serveur de fichiers central lorsque le PC ou l'appareil à proximité dispose d’une connectivité Internet ou intranet  
  
-   Déployer des déploiements existants des dossiers Redirection de dossiers, Fichiers hors connexion et de base  
  
-   Utiliser des technologies de gestion du serveur de fichiers existantes, telles que la classification de fichiers et des quotas de dossiers pour gérer les données utilisateur  
  
-   Spécifier les stratégies de sécurité pour demander aux PC et appareils de l’utilisateur de chiffrer Dossiers de travail et d’utiliser un mot de passe d’écran de verrouillage  
  
-   Utiliser le clustering de basculement avec Dossiers de travail pour fournir une solution à haute disponibilité  
  
## <a name="important-functionality"></a>Fonctionnalités importantes

 Dossiers de travail comprend les fonctionnalités suivantes.  
  
| Fonctionnalité | Disponibilité | Description |  
| ------------------- | ------------------ | ----------------- |  
| Service de rôle Dossiers de travail dans le Gestionnaire de serveur | Windows Server 2019, Windows Server 2016 ou Windows Server 2012 R2 | Les Services de fichiers et stockage permettent de configurer les partages de synchronisation (dossiers qui stockent des fichiers de travail de l’utilisateur), de surveiller Dossiers de travail et de gérer les partages de synchronisation et l’accès utilisateur |
| Applets de commande Dossiers de travail | Windows Server 2019, Windows Server 2016 ou Windows Server 2012 R2 | Module Windows PowerShell qui contient des applets de commande complètes pour la gestion des serveurs Dossiers de travail |  
| Intégration de Dossiers de travail à Windows | Windows 10<br /><br /> Windows 8.1<br /><br /> Windows RT 8.1<br /><br /> Windows 7 (téléchargement requis) | Dossiers de travail offre les fonctionnalités suivantes sur les ordinateurs Windows :<br /><br /> -   Un élément Panneau de configuration, qui configure et surveille Dossiers de travail<br />-   L’intégration de l’Explorateur de fichiers, qui permet d’accéder facilement aux fichiers de Dossiers de travail<br />-   Un moteur de synchronisation qui transfère les fichiers vers et depuis un serveur de fichiers central lors de l’optimisation des performances système et de l'autonomie de la batterie |
| Application Dossiers de travail pour les appareils | Android<br /><br /> Apple iPhone et iPad® | Application qui permet aux appareils populaires d'accéder aux fichiers dans Dossiers de travail |  
  
## <a name="new-and-changed-functionality"></a>Fonctionnalités nouvelles et modifiées
  
Le tableau suivant décrit certaines des principales modifications apportées à Dossiers de travail.  
  
| Fonctionnalité/fonction | Nouveauté ou mise à jour ? | Description |
| ---------------------------- | --------------------- | ----------------- |
| Prise en charge du proxy d’application Azure AD | Ajouté à Windows 10 version 1703, Android, iOS | Les utilisateurs distants peuvent accéder en toute sécurité à leurs fichiers sur le serveur Dossiers de travail à l’aide du proxy d'application Azure AD. |
| Accélération de la réplication des modifications | Mises à jour dans Windows 10 et Windows Server 2016 | Pour Windows Server 2012 R2, lorsque les modifications apportées aux fichiers sont synchronisées avec le serveur Dossiers de travail, les clients ne sont pas informés des modifications et attendent jusqu’à 10 minutes pour obtenir la mise à jour. Lorsque vous utilisez Windows Server 2016, le serveur de dossiers de travail informe immédiatement les clients Windows 10 et les modifications de fichiers sont synchronisées immédiatement. Cette fonctionnalité est une nouveauté de Windows Server 2016 et nécessite un client Windows 10. Si vous utilisez un client plus ancien ou si le serveur Dossiers de travail exécute Windows Server 2012 R2, le client continue à interroger le système toutes les 10 minutes, afin d’obtenir les modifications. |  
| Intégré dans la Protection des informations Windows (WIP) | Ajouté à Windows 10, version 1607 | Si un administrateur déploie WIP, Dossiers de travail peut appliquer la protection des données en chiffrant les données sur le PC. Le chiffrement utilise une clé associée à l’ID de l’entreprise, qui peut être réinitialisée à distance à l’aide d’un package de gestion des appareils mobiles pris en charge comme Microsoft Intune. |  
| Intégration de Microsoft Office | Ajouté à Windows 10, version 1511 | Dans Windows 8.1. vous pouvez accéder à Dossiers de travail depuis les applications Office en cliquant ou en appuyant sur Ce PC, puis en accédant à l’emplacement Dossiers de travail sur votre PC. Dans Windows 10, vous pouvez accéder encore plus facilement à Dossiers de travail en l’ajoutant à la liste des emplacements affichés par Office lors de l'enregistrement ou de l'ouverture de fichiers. Pour plus d’informations, voir [Dossiers de travail dans Windows 10](https://windows.microsoft.com/windows-10/work-folders-in-windows-10) et [Résolution des problèmes en utilisant Dossiers de travail comme emplacement dans Microsoft Office](https://social.technet.microsoft.com/wiki/contents/articles/32881.troubleshooting-using-work-folders-as-a-place-in-microsoft-office.aspx). |  
  
## <a name="software-requirements"></a>Configuration logicielle requise

La fonctionnalité Dossiers de travail présente la configuration logicielle requise suivante pour les serveurs de fichiers et votre infrastructure réseau :  
  
-   Un serveur exécutant Windows Server 2019, Windows Server 2016 ou Windows Server 2012 R2 pour l’hébergement de synchronisation des partages avec des fichiers utilisateur  
  
-   Un volume formaté avec le système de fichiers NTFS pour le stockage des fichiers utilisateur  
  
-   Pour appliquer les stratégies de mot de passe sur les PC Windows 7, vous devez utiliser des stratégies de mot de passe de stratégie de groupe. Vous devez également exclure les PC Windows 7 des stratégies de mot de passe de Dossiers de travail (si vous les utilisez).

-   Un certificat de serveur pour chaque serveur de fichiers qui hébergera Dossiers de travail. Ces certificats doivent provenir d’une autorité de certification approuvée par vos utilisateurs, dans l’idéal une autorité de certification publique.

-   (Facultatif) Une forêt Active Directory Domain Services avec les extensions de schéma dans Windows Server 2012 R2 pour prendre en charge automatiquement les références des PC et des appareils sur le serveur de fichiers approprié lors de l’utilisation de plusieurs serveurs de fichiers.  
  
Pour permettre aux utilisateurs d’effectuer la synchronisation sur Internet, il existe des exigences supplémentaires :  
  
-   La possibilité de rendre un serveur accessible à partir d’Internet en créant des règles de publication dans le proxy inverse ou la passerelle réseau de votre organisation  
  
-   (Facultatif) Un nom de domaine enregistré publiquement et la possibilité de créer d’autres enregistrements DNS publics pour le domaine  
  
-   (Facultatif) Une infrastructure des services de fédération Active Directory (AD FS) lors de l’utilisation de l’authentification AD FS  
  
La fonctionnalité Dossiers de travail présente la configuration logicielle requise suivante pour les ordinateurs clients :  
  
-   Les PC et les appareils doivent fonctionner sous l'un des systèmes d'exploitation suivants :  
  
    -   Windows 10  
  
    -   Windows 8.1  
  
    -   Windows RT 8.1  
  
    -   Windows 7  
  
    -   Android 4.4 KitKat et versions ultérieures  
  
    -   iOS 10.2 et versions ultérieures  
  
-   Les PC Windows 7 doivent exécuter l'une des éditions suivantes de Windows :  
  
    -   Windows 7 Professionnel  
  
    -   Windows 7 Édition Intégrale  
  
    -   Windows 7 Entreprise  
  
-   Les PC Windows 7 doivent être joints au domaine de votre organisation (ils ne peuvent pas être joints à un groupe de travail).  
  
-   Un espace libre suffisant sur un lecteur local au format NTFS pour stocker tous les fichiers utilisateur dans Dossiers de travail, plus 6 Go supplémentaires d’espace libre si Dossiers de travail se trouve sur le lecteur système, son emplacement par défaut. Dossiers de travail utilise l’emplacement suivant par défaut : **%USERPROFILE%\Dossiers de travail**  
  
     Toutefois, les utilisateurs peuvent modifier l’emplacement lors de l’installation (les cartes microSD et les lecteurs USB formatés avec le système de fichiers NTFS sont des emplacements pris en charge, même si la synchronisation s’arrête si les lecteurs sont supprimés).  
  
     La taille maximale pour les fichiers individuels est de 10 Go par défaut. Il n’existe pas de limite de stockage par utilisateur, même si les administrateurs peuvent utiliser la fonctionnalité des quotas du Gestionnaire de ressources du serveur de fichiers pour implémenter les quotas.  
  
-   La fonctionnalité Dossiers de travail ne prend pas en charge la restauration de l’état des ordinateurs virtuels clients. Effectuez plutôt des opérations de sauvegarde et de restauration à partir de l’ordinateur virtuel client en utilisant Sauvegarde d’image système ou une autre application de sauvegarde.  
  
## <a name="work-folders-compared-to-other-sync-technologies"></a>Comparaison de Dossiers de travail et d’autres technologies de synchronisation  

Le tableau suivant décrit la position des différentes technologies de synchronisation Microsoft et quand les utiliser.  
  
| | Dossiers de travail | Fichiers hors connexion | OneDrive Entreprise | OneDrive |
| - | ------------------ | ------------------- | -------------------------- | -------------- |
| **Résumé de la technologie** | Synchronise les fichiers stockés sur un serveur de fichiers avec les PC et appareils | Synchronise les fichiers stockés sur un serveur de fichiers avec les PC qui ont accès au réseau d’entreprise (peut être remplacé par Dossiers de travail) | Synchronise les fichiers stockés dans Office 365 ou SharePoint avec les PC et appareils à l’intérieur ou en dehors d’un réseau d’entreprise et fournit la fonctionnalité de collaboration de document | Synchronise les fichiers personnels stockés dans OneDrive avec les PC, les ordinateurs Mac et les appareils |
| **Conçue pour fournir l’accès utilisateur pour les fichiers de travail** | Oui | Oui | Oui | Non |
| **Service cloud** | Aucune | Aucune | Office 365 | Microsoft OneDrive |
| **Serveurs du réseau interne** | Serveurs de fichiers exécutant Windows Server 2012 R2 ou Windows Server 2016 | Serveurs de fichiers | Serveur SharePoint (facultatif) | Aucune |
| **Clients pris en charge** | PC, iOS, Android | PC d'un réseau d’entreprise ou connectés via DirectAccess, VPN ou d’autres technologies d’accès à distance | PC, iOS, Android, Windows Phone | PC, ordinateurs Mac, Windows Phone, iOS, Android |
  
> [!NOTE]
>  Outre les technologies de synchronisation répertoriées dans le tableau précédent, Microsoft propose d'autres technologies de réplication, notamment la réplication DFS, qui est conçue pour la réplication de serveur à serveur et BranchCache, qui est conçu comme une technologie d’accélération WAN de succursale. Pour plus d'informations, voir [Vue d’ensemble des espaces de noms DFS et de la réplication DFS](https://technet.microsoft.com/library/jj127250(v=ws.11).aspx) et [Présentation de BranchCache](https://technet.microsoft.com/library/hh831696(v=ws.11).aspx) 
  
## <a name="server-manager-information"></a>Informations sur le Gestionnaire de serveur  

Dossiers de travail fait partie du rôle Services de fichiers et de stockage. Vous pouvez installer Dossiers de travail à l’aide de l’Assistant Ajout de rôles et de fonctionnalités ou l'applet de commande `Install-WindowsFeature`. Les deux méthodes exécutent les tâches suivantes :  
  
-   Ajout de la page **Dossiers de travail** à **Services de fichiers et de stockage** dans le Gestionnaire de serveur  
  
-   Installation du service de partages de synchronisation Windows, qui est utilisé par Windows Server pour héberger les partages de synchronisation  
  
-   Installation du module SyncShare Windows PowerShell pour gérer Dossiers de travail sur le serveur  
  
## <a name="interoperability-with-windows-azure-virtual-machines"></a>Interopérabilité avec les machines virtuelles Microsoft Azure

 Vous pouvez exécuter ce service de rôle Windows Server sur une machine virtuelle dans Microsoft Azure. Ce scénario a été testé avec Windows Server 2012 R2 et Windows Server 2016.  
  
Pour en savoir plus sur la prise en main des machines virtuelles Microsoft Azure, visitez le [site web Microsoft Azure](http://www.windowsazure.com/documentation/services/virtual-machines).  
  
## <a name="see-also"></a>Voir aussi

 Pour plus d’informations connexes, voir les ressources suivantes.  
  
| Type de contenu | Références |
| ------------------ | ---------------- |
| **Évaluation du produit** | -   [Dossiers de travail pour Android – lancé](https://blogs.technet.microsoft.com/filecab/2016/03/16/work-folders-for-android-released) (billet de blog)<br />-   [Dossiers de travail pour iOS : iPad application version](https://blogs.technet.com/b/filecab/archive/2015/01/16/work-folders-for-ios-ipad-app-release.aspx) (billet de blog)<br />-   [Présentation des dossiers de travail sur Windows Server 2012 R2](http://blogs.technet.com/b/filecab/archive/2013/07/09/introducing-work-folders-on-windows-server-2012-r2.aspx) (billet de blog)<br />-   [Introduction aux dossiers de travail](http://channel9.msdn.com/posts/Introduction-to-Work-Folders) (vidéo Channel 9)<br />-   [Utiliser des dossiers de déploiement de laboratoire de tests](http://blogs.technet.com/b/filecab/archive/2013/07/10/work-folders-test-lab-deployment.aspx) (billet de blog)<br />-   [Utiliser des dossiers pour Windows 7](http://blogs.technet.com/b/filecab/archive/2014/04/24/work-folders-for-windows-7.aspx) (billet de blog) |
| **Déploiement** | -   [Conception d’une implémentation de dossiers de travail](plan-work-folders.md)<br />-   [Déploiement de dossiers de travail](deploy-work-folders.md)<br />-   [Déploiement de dossiers de travail avec AD FS et Proxy d’Application Web (WAP)](deploy-work-folders-adfs-overview.md)<br />-   [Déploiement de dossiers de travail avec le Proxy d’Application Azure AD](https://blogs.technet.microsoft.com/filecab/2017/05/31/enable-remote-access-to-work-folders-using-azure-active-directory-application-proxy/)<br />- [Fichiers hors connexion (CSC) pour utiliser le Guide de Migration de dossiers](https://blogs.technet.microsoft.com/filecab/2016/08/12/offline-files-csc-to-work-folders-migration-guide/)<br />-   [Considérations relatives aux performances pour les déploiements de dossiers de travail](https://blogs.technet.com/b/filecab/archive/2013/11/01/performance-considerations-for-large-scale-work-folders-deployments.aspx)<br />-   [Dossiers de travail pour Windows 7 (téléchargement 64 bits)](https://www.microsoft.com/download/details.aspx?id=42558)<br />-   [Dossiers de travail pour Windows 7 (téléchargement de 32 bits)](https://www.microsoft.com/download/details.aspx?id=42559) |
| **Opérations** | -   [Application iPad de dossiers de travail : Forum aux questions](https://windows.microsoft.com/windows/work-folders-ipad-faq) (pour les utilisateurs)<br />-   [Gestion de certificat de dossiers de travail](https://blogs.technet.com/b/filecab/archive/2013/08/09/work-folders-certificate-management.aspx) (billet de blog)<br />-   [Surveillance des déploiements de dossiers de travail Windows Server 2012 R2](https://blogs.technet.com/b/filecab/archive/2013/10/15/monitoring-windows-server-2012-r2-work-folders-deployments.aspx) (billet de blog)<br />-   [Applets de commande SyncShare (dossiers de travail) dans Windows PowerShell](https://docs.microsoft.com/powershell/module/syncshare/?view=win10-ps)<br />-   [Carte de référence rapide de stockage et les applets de commande PowerShell de Services fichier pour l’édition de Windows Server 2012 R2 Preview](http://blogs.technet.com/b/filecab/archive/2013/07/30/storage-and-file-services-powershell-cmdlets-quick-reference-card-for-windows-server-2012-r2-preview-edition.aspx) |
| **Résolution des problèmes** | -   [Windows Server 2012 R2 – conflit de Port de résolution avec sites Web IIS et les dossiers de travail](https://blogs.technet.com/b/filecab/archive/2013/10/15/windows-server-2012-r2-resolving-port-conflict-with-iis-websites-and-work-folders.aspx) (billet de blog)<br />-   [Erreurs courantes dans les dossiers de travail](https://social.technet.microsoft.com/wiki/contents/articles/30578.common-errors-in-work-folders.aspx) |
| **Ressources de la communauté** | -   [Forum sur le stockage et les Services de fichiers](https://social.technet.microsoft.com/Forums/windowsserver/home?forum=winserverfiles)<br />-   [L’équipe du stockage chez Microsoft - Blog File Cabinet](http://blogs.technet.com/b/filecab/)<br />-   [Demander le Blog de l’équipe Directory Services](http://blogs.technet.com/b/askds/) |  
| **Technologies connexes** | -   [Stockage dans Windows Server 2016](../storage.md)<br>-   [Services de fichiers et stockage](https://technet.microsoft.com/library/hh831487(v=ws.11).aspx)<br />-   [File Server Resource Manager](https://technet.microsoft.com/library/hh831701(v=ws.11).aspx)<br />-   [La Redirection de dossiers, les fichiers hors connexion et les profils utilisateur itinérants](https://technet.microsoft.com/library/hh848267(v=ws.11).aspx)<br />-   [BranchCache](https://technet.microsoft.com/library/hh831696(v=ws.11).aspx)<br />-   [Espaces de noms DFS et réplication DFS](https://technet.microsoft.com/library/jj127250(v=ws.11).aspx) |
