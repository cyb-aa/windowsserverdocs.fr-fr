---
title: Déployer l’accès à distance avec l’authentification par mot de passe à usage unique
description: Cette rubrique fait partie du guide déployer l’accès à distance avec l’authentification par mot de passe à usage unique dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d0de5f459e31e1dfac40e49cd6cc83de8722df4d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404426"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>Déployer l’accès à distance avec l’authentification par mot de passe à usage unique

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

 Windows Server 2016 et Windows Server 2012 combinent DirectAccess et le service Routage et accès à distance \(RRAS @ no__t-1 VPN en un seul rôle d’accès à distance.   

## <a name="BKMK_OVER"></a>Description du scénario  
Dans ce scénario, un serveur d’accès à distance sur lequel DirectAccess est activé est configuré pour authentifier les utilisateurs du client DirectAccess avec deux authentifications à usage unique @ no__t-0factor \(OTP @ no__t-2, en plus des informations d’identification de Active Directory standard.  
  
## <a name="prerequisites"></a>Prérequis  
Avant de déployer ce scénario, prenez connaissance des conditions requises suivantes qui ont leur importance :  
  
-   [Le déploiement d’un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) doit être déployé avant le déploiement du mot de passe à usage unique.  
  
-   Les clients Windows 7 doivent utiliser DCA 2,0 pour prendre en charge le mot de passe à usage unique.  
  
-   Le mot de passe à usage unique ne prend pas en charge les modifications de code confidentiel.  
  
-   Une infrastructure à clé publique doit être déployée.  
  
    Pour plus d’informations, voir : Mini-module [Test Lab Guide : Infrastructure à clé publique de base pour Windows Server 2012. ](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   La modification des stratégies en dehors de la console de gestion DirectAccess ou des applets de commande Windows PowerShell n’est pas prise en charge.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario d’authentification par mot de passe à usage unique inclut plusieurs étapes :  
  
1.  [Déployez un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Un seul serveur d’accès à distance doit être déployé avant de configurer le mot de passe à usage unique. La planification et le déploiement d’un serveur unique incluent la conception et la configuration d’une topologie de réseau, la planification et le déploiement de certificats, la configuration des services DNS et Active Directory, la configuration des paramètres de serveur d’accès à distance, le déploiement des clients DirectAccess et la préparation des serveurs intranet.  
  
2.  [Planifier l’accès à distance avec l’authentification par mot de passe à usage unique](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication). Outre la planification requise pour un serveur unique, le mot de passe à usage unique requiert la planification d’une autorité de certification Microsoft \(CA @ no__t-1 et des modèles de certificat pour le mot de passe à usage unique ; et un serveur de mot de passe à usage unique RADIUS @ no__t-2enabled. La planification peut également inclure une exigence pour les groupes de sécurité pour exempter des utilisateurs spécifiques d’une authentification forte \(OTP ou de carte à puce @ no__t-1. Pour plus d’informations sur la configuration du mot de passe à usage unique dans un environnement multino__t-0forest, consultez [configurer un déploiement à plusieurs forêts](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md).  
  
3.  [Configurer DirectAccess avec l’authentification par mot de passe à usage unique](/configure/Configure-RA-with-OTP-Authentication.md). Le déploiement par mot de passe à usage unique est constitué d’un certain nombre d’étapes de configuration, notamment la préparation de l’infrastructure pour l’authentification par mot de passe à usage unique, la configuration du serveur à usage unique, la configuration des paramètres de mot de passe à usage unique sur le serveur d’accès à distance  
  
4.  [Résoudre les problèmes liés à un déploiement OTP] ((/troubleshoot/Troubleshoot-an-OTP-Deployment.md). Cette section de résolution des problèmes décrit plusieurs des erreurs les plus courantes qui peuvent se produire lors du déploiement de l’accès à distance avec l’authentification par mot de passe à usage unique.  
  
## <a name="BKMK_APP"></a>Applications pratiques  
Renforcer la sécurité : l’utilisation du mot de passe à usage unique augmente la sécurité de votre déploiement DirectAccess. Un utilisateur requiert des informations d’identification par mot de passe à usage unique afin d’obtenir l’accès au réseau interne. Un utilisateur fournit des informations d’identification par mot de passe à usage unique via les connexions d’espace de travail disponibles dans les connexions réseau sur l’ordinateur client Windows 10 ou Windows 8, ou à l’aide de l’Assistant de connectivité DirectAccess \(DCA @ no__t-1 sur les ordinateurs clients exécutant Windows 7. Le processus d’authentification par mot de passe à usage unique fonctionne comme suit :  
  
1.  Le client DirectAccess entre les informations d’identification de domaine pour accéder aux serveurs d’infrastructure DirectAccess \(over le tunnel d’infrastructure @ no__t-1.  Si aucune connexion au réseau interne n’est disponible, en raison d’un échec IKE spécifique, la connexion d’espace de travail sur l’ordinateur client notifie l’utilisateur que des informations d’identification sont requises. Sur les ordinateurs clients exécutant Windows 7, un pop @ no__t-0UP demandant des informations d’identification de carte à puce s’affiche.  
  
2.  Une fois que les informations d’identification par mot de passe à usage unique ont été entrées, elles sont envoyées via SSL au serveur d’accès à distance, avec une demande de certificat d’ouverture de session de carte à puce Short @ no__t-0term.  
  
3.  Le serveur d’accès à distance initie la validation des informations d’identification par mot de passe à usage unique avec le serveur de mot de passe à usage unique RADIUS @ no__t-0based.  
  
4.  Si l’opération réussit, le serveur d’accès à distance signe la demande de certificat à l’aide de son certificat d’autorité d’inscription et la renvoie à l’ordinateur client DirectAccess  
  
5.  L’ordinateur client DirectAccess transfère la demande de certificat signée à l’autorité de certification et stocke le certificat inscrit pour une utilisation par le SSP Kerberos @ no__t-0AP.  
  
6.  L’ordinateur client utilise ce certificat pour effectuer de manière transparente l’authentification Kerberos par carte à puce standard.  
  
## <a name="BKMK_NEW"></a>Rôles et fonctionnalités inclus dans ce scénario  
Le tableau suivant répertorie les fonctionnalités et rôles requis pour ce scénario :  
  
|Rôle @ no__t-0Feature|Prise en charge de ce scénario|  
|---------|-----------------|  
|*Rôle gestion de l’accès à distance*|Ce rôle est installé et désinstallé à l’aide de la console du Gestionnaire de serveur. Ce rôle englobe à la fois DirectAccess, qui était auparavant une fonctionnalité de Windows Server 2008 R2, et les services de routage et d’accès à distance qui étaient auparavant un service de rôle sous le rôle de serveur \(NPAS @ no__t-1 de services de stratégie et d’accès réseau. Le rôle Accès à distance est constitué de deux composants :<br /><br />1.  Les services DirectAccess et de routage et d’accès à distance \(RRAS @ no__t-1 VPN-DirectAccess et VPN sont gérés ensemble dans la console de gestion de l’accès à distance.<br />2.  Routage RRAS : les fonctionnalités de routage RRAS sont gérées dans la console de routage et d’accès distant héritée.<br /><br />Le rôle Accès à distance dépend des fonctionnalités de serveur suivantes :<br /><br />-Internet Information Services \(IIS @ no__t-1 Web Server-cette fonctionnalité est requise pour configurer le serveur emplacement réseau, utiliser l’authentification par mot de passe à usage unique et configurer la sonde Web par défaut.<br />-Base de données interne Windows : utilisée pour la comptabilité locale sur le serveur d’accès à distance.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<br /><br />-Elle est installée par défaut sur un serveur d’accès à distance lorsque le rôle accès à distance est installé et prend en charge l’interface utilisateur de la console de gestion à distance.<br />-Il peut éventuellement être installé sur un serveur qui n’exécute pas le rôle de serveur d’accès à distance. Dans ce cas, elle est utilisée pour la gestion à distance d’un ordinateur d’accès à distance qui exécute DirectAccess et le réseau privé virtuel (VPN).<br /><br />La fonctionnalité des outils de gestion de l’accès à distance est constituée des éléments suivants :<br /><br />-Interface utilisateur graphique d’accès à distance et outils en ligne de commande<br />-Module d’accès à distance pour Windows PowerShell<br /><br />Les dépendances incluent :<br /><br />-Console de gestion des stratégies de groupe<br />-@No__t du kit d’administration du gestionnaire des connexions RAS-0CMAK @ no__t-1<br />-Windows PowerShell 3,0<br />-Outils et infrastructure de gestion graphique|  
  
## <a name="BKMK_HARD"></a>Configuration matérielle requise  
La configuration matérielle requise pour ce scénario comprend les éléments suivants :  
  
-   Un ordinateur qui répond à la configuration matérielle requise pour Windows Server 2016 ou Windows Server 2012.  
  
-   Pour tester le scénario, au moins un ordinateur exécutant Windows 10, Windows 8 ou Windows 7 configuré en tant que client DirectAccess est requis.  
  
-   Un serveur de mot de passe à usage unique prenant en charge PAP sur RADIUS.  
  
-   Un jeton logiciel ou matériel de mot de passe à usage unique.  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
Plusieurs conditions sont requises pour ce scénario :  
  
1.  Configuration logicielle requise pour un déploiement sur un seul serveur. Pour plus d’informations, consultez [déployer un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
2.  Outre la configuration logicielle requise pour un serveur unique, il existe un certain nombre de conditions requises pour le mot de passe à usage unique @ no__t-0specific :  
  
    1.  Autorité de certification pour l’authentification IPsec : dans un déploiement avec mot de passe à usage unique, DirectAccess doit être déployé à l’aide des certificats d’ordinateur IPsec émis par une autorité de certification L’authentification IPsec utilisant le serveur d’accès à distance comme proxy Kerberos n’est pas prise en charge dans un déploiement du mot de passe à usage unique. Une autorité de certification interne est requise.  
  
    2.  Autorité de certification pour l’authentification par mot de passe à usage unique : une autorité de certification d’entreprise Microsoft @no__t 0running sur Windows 2003 Server ou version ultérieure @ no__t-1 est requise pour émettre le certificat client OTP. L’autorité de certification utilisée pour émettre des certificats pour l’authentification IPsec peut être utilisée. Le serveur de l’autorité de certification doit être disponible via le premier tunnel d’infrastructure.  
  
    3.  Groupe de sécurité : pour exempter les utilisateurs de l’authentification forte, un groupe de sécurité Active Directory contenant ces utilisateurs est requis.  
  
    4.  Exigences du client @ no__t-0side : pour les ordinateurs clients Windows 10 et Windows 8, l’Assistant connectivité réseau \(NCA @ no__t-2 est utilisé pour détecter si les informations d’identification par mot de passe à usage unique sont requises. Si c’est le cas, le gestionnaire multimédia DirectAccess vous invite à entrer des informations d’identification.  NCA est inclus dans le système d’exploitation et aucune installation ni aucun déploiement n’est requis. Pour les ordinateurs clients Windows 7, l’Assistant de connectivité DirectAccess \(DCA @ no__t-1 2,0 est requis. Il peut être téléchargé dans le [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=29039).  
  
    5.  Notez les points suivants :  
  
        1.  L’authentification par mot de passe à usage unique peut être utilisée en parallèle avec la carte à puce et Module de plateforme sécurisée (TPM) \(TPM @ no__t-1 @ no__t-2based authentification. L’activation de l’authentification par mot de passe à usage unique dans la console de gestion de l’accès à distance permet également l’utilisation de l’authentification par carte à puce.  
  
        2.  Pendant la configuration de l’accès à distance, les utilisateurs d’un groupe de sécurité spécifié peuvent être exemptés de deux authentifications @ no__t-0factor, et donc s’authentifier avec le nom d’utilisateur @ no__t-1Password uniquement.  
  
        3.  Les modes de nouveau code confidentiel et de code de jeton suivant de mot de passe à usage unique ne sont pas pris en charge  
  
        4.  Dans un déploiement multisite de l’accès à distance, les paramètres de mot de passe à usage unique sont globaux et permettent l’identification pour tous les points d’entrée. Si plusieurs serveurs RADIUS ou d’autorité de certification sont configurés pour le mot de passe à usage unique, ils sont classés par chaque serveur d’accès à distance en fonction de leur disponibilité et proximité.  
  
        5.  Lors de la configuration du mot de passe à usage unique dans un environnement multino__t-0forest d’accès à distance, les autorités de certification à usage unique doivent provenir de la forêt de ressources uniquement, et l’inscription de certificats doit être configurée dans les approbations de forêt. Pour plus d’informations, consultez [AD CS : Inscription de certificats inter-forêts avec Windows Server 2008 R2 @ no__t-0.  
  
        6.  Les utilisateurs qui utilisent un jeton de mot de passe à usage unique FOB KEY doivent insérer le code confidentiel suivi du code de jeton \(without tous les séparateurs @ no__t-1 dans la boîte de dialogue de mot de passe à usage unique DirectAccess. Les utilisateurs qui utilisent le jeton PIN PAD OTP doivent insérer uniquement le code de jeton dans la boîte de dialogue.  
  
        7.  Lorsque le WEBDAV est activé, le mot de passe à usage unique doit être désactivé.  
  
## <a name="KnownIssues"></a>Problèmes connus  
Les problèmes décrits ci-après sont connus et surviennent souvent lors de la configuration d’un scénario de mot de passe à usage unique :  
  
-   L’accès à distance utilise un mécanisme de sondage pour vérifier la connectivité aux serveurs de mot de passe à usage unique RADIUS @ no__t-0based. Dans certains cas cela peut provoquer une erreur sur le serveur à mot de passe à usage unique. Pour éviter ce problème, procédez comme suit sur le serveur à mot de passe à usage unique :  
  
    -   Créez un compte d’utilisateur qui correspond au nom d’utilisateur et au mot de passe configurés sur le serveur d’accès à distance pour le mécanisme de détection. Le nom d’utilisateur ne doit pas définir un utilisateur Active Directory.  
  
        Par défaut, le nom d’utilisateur sur le serveur d’accès à distance est DAProbeUser et le mot de passe DAProbePass. Ces paramètres par défaut peuvent être modifiés à l’aide des valeurs suivantes dans le registre du serveur d’accès à distance :  
  
        -   HKEY @ no__t-0LOCAL @ no__t-1MACHINE @ no__t-2SOFTWARE @ no__t-3Microsoft @ no__t-4DirectAccess @ no__t-5OTP @ no__t-6RadiusProbeUser  
  
        -   HKEY @ no__t-0LOCAL @ no__t-1MACHINE @ no__t-2SOFTWARE @ no__t-3Microsoft @ no__t-4DirectAccess @ no__t-5OTP @ no__t-6 RadiusProbePass  
  
-   Si vous modifiez le certificat racine IPsec dans un déploiement DirectAccess configuré et en cours d’exécution, la fonctionnalité de mot de passe à usage unique cesse de fonctionner. Pour résoudre ce problème, sur chaque serveur DirectAccess, à une invite de commandes Windows PowerShell, exécutez la commande : `iisreset`  
  
