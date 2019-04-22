---
title: Déployer l’accès à distance avec l’authentification par mot de passe à usage unique
description: Cette rubrique fait partie du guide de déploiement des accès à distance avec authentification OTP dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b1b2fe70-7956-46e8-a3e3-43848868df09
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ecb4503c6f8cbc08f2175a33a41929491e4a2b7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59811990"
---
# <a name="deploy-remote-access-with-otp-authentication"></a>Déployer l’accès à distance avec l’authentification par mot de passe à usage unique

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

 Windows Server 2016 et Windows Server 2012 combinent DirectAccess, routage et Remote Access Service \(RRAS\) VPN dans un rôle d’accès à distance unique.   

## <a name="BKMK_OVER"></a>Description du scénario  
Dans ce scénario, un accès à distance serveur avec DirectAccess activé est configuré pour authentifier les utilisateurs des clients DirectAccess avec deux\-mot de passe à usage unique facteur \(OTP\) authentification, en plus des actifs standard Informations d’identification de l’annuaire.  
  
## <a name="prerequisites"></a>Prérequis  
Avant de déployer ce scénario, prenez connaissance des conditions requises suivantes qui ont leur importance :  
  
-   [Déployer un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md) doit être déployé avant de déployer le secret à usage unique.  
  
-   Les Clients Windows 7 doivent utiliser DCA 2.0 pour prendre en charge le secret à usage unique.  
  
-   Le mot de passe à usage unique ne prend pas en charge les modifications de code confidentiel.  
  
-   Une infrastructure à clé publique doit être déployée.  
  
    Pour plus d’informations, voir : [Mini-module de Guide de laboratoire de test : Infrastructure à clé publique base pour Windows Server 2012.](https://social.technet.microsoft.com/wiki/contents/articles/7862.test-lab-guide-mini-module-basic-pki-for-windows-server-2012.aspx)  
  
-   Modification des stratégies en dehors de la console de gestion DirectAccess ou les applets de commande Windows PowerShell n’est pas pris en charge.  
  
## <a name="in-this-scenario"></a>Dans ce scénario  
Ce scénario d’authentification par mot de passe à usage unique inclut plusieurs étapes :  
  
1.  [Déployer un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Un seul serveur d’accès à distance doit être déployé avant de configurer le secret à usage unique. La planification et le déploiement d’un serveur unique incluent la conception et la configuration d’une topologie de réseau, la planification et le déploiement de certificats, la configuration des services DNS et Active Directory, la configuration des paramètres de serveur d’accès à distance, le déploiement des clients DirectAccess et la préparation des serveurs intranet.  
  
2.  [Planifier l’accès à distance avec l’authentification OTP](https://docs.microsoft.com/windows-server/remote/remote-access/ras/otp/plan/plan-remote-access-with-otp-authentication). Outre la planification requise pour un serveur unique, secret à usage unique requiert la planification d’une autorité de certification Microsoft \(autorité de certification\) et les modèles de certificats pour, ainsi qu’un rayon\-OTP serveur activé. Planification peut également inclure une condition requise pour les groupes de sécurité exempter des utilisateurs spécifiques de strong \(OTP ou carte à puce\) l’authentification. Pour plus d’informations concernant la configuration du secret à usage unique dans un multi\-environnement de forêt, consultez [configurer un déploiement à forêts multiples](../../ras/multi-forest/Configure-a-Multi-Forest-Deployment.md).  
  
3.  [Configurer DirectAccess avec l’authentification OTP](/configure/Configure-RA-with-OTP-Authentication.md). Déploiement de secret à usage unique se compose d’un nombre d’étapes de configuration, y compris la préparation de l’infrastructure pour l’authentification OTP, configuration du serveur de secret à usage unique, configuration des paramètres OTP sur le serveur d’accès à distance et la mise à jour des paramètres des clients DirectAccess.  
  
4.  [Troubleshoot an OTP Deployment] ((/troubleshoot/Troubleshoot-an-OTP-Deployment.md). Cette section décrit un nombre d’erreurs les plus courantes qui peuvent se produire lors du déploiement d’accès à distance avec l’authentification OTP.  
  
## <a name="BKMK_APP"></a>Applications pratiques  
OTP de sécurité à l’aide d’augmentation augmente la sécurité de votre déploiement de DirectAccess. Un utilisateur requiert des informations d’identification par mot de passe à usage unique afin d’obtenir l’accès au réseau interne. Un utilisateur doit fournir des informations d’identification de secret à usage unique via les connexions d’espace de travail disponibles dans les connexions réseau sur l’ordinateur client Windows 10 ou Windows 8, ou à l’aide de l’Assistant connectivité DirectAccess \(DCA\) sur les ordinateurs clients en cours d’exécution Windows 7. Le processus d’authentification par mot de passe à usage unique fonctionne comme suit :  
  
1.  Le client DirectAccess entre des informations d’identification de domaine pour accéder aux serveurs d’infrastructure DirectAccess \(via le tunnel d’infrastructure\).  Si aucune connexion au réseau interne n’est disponible, en raison d’un échec IKE spécifique, la connexion d’espace de travail sur l’ordinateur client notifie l’utilisateur que des informations d’identification sont requises. Sur les ordinateurs clients exécutant Windows 7, un point de présence\-demandant des informations d’identification de carte à puce s’affiche.  
  
2.  Une fois que les informations d’identification de secret à usage unique ont été entrées, elles sont envoyées via SSL au serveur d’accès à distance, avec une demande pour une courte\-certificat d’ouverture de session de carte à puce à terme.  
  
3.  Le serveur d’accès à distance initie la validation des informations d’identification de secret à usage unique avec le rayon\-en fonction du serveur de secret à usage unique.  
  
4.  Si l’opération réussit, le serveur d’accès à distance signe la demande de certificat à l’aide de son certificat d’autorité d’inscription et la renvoie à l’ordinateur client DirectAccess  
  
5.  L’ordinateur client DirectAccess transfère la demande de certificat signé à l’autorité de certification et stocke le certificat inscrit pour une utilisation par le SSP Kerberos\/AP.  
  
6.  L’ordinateur client utilise ce certificat pour effectuer de manière transparente l’authentification Kerberos par carte à puce standard.  
  
## <a name="BKMK_NEW"></a>Fonctionnalités et rôles inclus dans ce scénario  
Le tableau suivant répertorie les fonctionnalités et rôles requis pour ce scénario :  
  
|Rôle\/fonctionnalité|Prise en charge de ce scénario|  
|---------|-----------------|  
|*Rôle de gestion de l’accès à distance*|Ce rôle est installé et désinstallé à l’aide de la console du Gestionnaire de serveur. Ce rôle englobe à la fois DirectAccess, ce qui était auparavant une fonctionnalité de Windows Server 2008 R2 et routage et accès à distance qui était auparavant un service de rôle sous la stratégie de réseau et les Services d’accès \(NPAS\) server rôle. Le rôle Accès à distance est constitué de deux composants :<br /><br />1.  DirectAccess, routage et accès à distance \(RRAS\) VPN DirectAccess et VPN sont gérés ensemble dans la console de gestion de l’accès à distance.<br />2.  Les fonctionnalités de routage de routage de RRAS-RRAS sont gérées dans la console Routage et accès distant héritée.<br /><br />Le rôle Accès à distance dépend des fonctionnalités de serveur suivantes :<br /><br />-Internet Information Services \(IIS\) serveur Web - cette fonctionnalité est nécessaire pour configurer le serveur d’emplacement réseau, utiliser l’authentification OTP et configurer la sonde web par défaut.<br />-Windows Database-Used interne pour la gestion des comptes locale sur le serveur d’accès à distance.|  
|Fonctionnalité des outils de gestion de l’accès à distance|Cette fonctionnalité est installée comme suit :<br /><br />-Il est installé par défaut sur un serveur d’accès à distance lorsque le rôle accès à distance est installé et prend en charge de l’interface utilisateur de console Administration à distance.<br />-Il peut éventuellement être installé sur un serveur n'exécutant pas le rôle de serveur d’accès à distance. Dans ce cas, elle est utilisée pour la gestion à distance d’un ordinateur d’accès à distance qui exécute DirectAccess et le réseau privé virtuel (VPN).<br /><br />La fonctionnalité des outils de gestion de l’accès à distance est constituée des éléments suivants :<br /><br />-Accès à distance GUI et outils de ligne de commande<br />-Module d’accès distant pour Windows PowerShell<br /><br />Les dépendances incluent :<br /><br />-Console de gestion de stratégie de groupe<br />-Kit d’Administration de Connection Manager RAS \(CMAK\)<br />-Windows PowerShell 3.0<br />-Infrastructure et des outils de gestion graphique|  
  
## <a name="BKMK_HARD"></a>Configuration matérielle requise  
La configuration matérielle requise pour ce scénario comprend les éléments suivants :  
  
-   Un ordinateur qui répond à la configuration matérielle requise pour Windows Server 2016 ou Windows Server 2012.  
  
-   Pour tester le scénario, au moins un ordinateur exécutant Windows 10, Windows 8 ou Windows 7 configuré comme un client DirectAccess est requis.  
  
-   Un serveur de mot de passe à usage unique prenant en charge PAP sur RADIUS.  
  
-   Un jeton logiciel ou matériel de mot de passe à usage unique.  
  
## <a name="BKMK_SOFT"></a>Configuration logicielle requise  
Plusieurs conditions sont requises pour ce scénario :  
  
1.  Configuration logicielle requise pour un déploiement sur un seul serveur. Pour plus d’informations, consultez [déployer un serveur DirectAccess unique avec des paramètres avancés](../../directaccess/single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).  
  
2.  En plus de la configuration logicielle requise pour un seul serveur, il existe un nombre d’OTP\-des exigences spécifiques :  
  
    1.  Autorité de certification pour IPsec d’authentification dans un déploiement OTP que DirectAccess doit être déployé à l’aide d’IPsec machines des certificats émis par une autorité de certification. L’authentification IPsec utilisant le serveur d’accès à distance comme proxy Kerberos n’est pas prise en charge dans un déploiement du mot de passe à usage unique. Une autorité de certification interne est requise.  
  
    2.  Autorité de certification pour Microsoft de l’authentification-A OTP AC d’entreprise \(en cours d’exécution sur Windows 2003 Server ou version ultérieure\) est requise pour émettre le certificat de client secret à usage unique. L’autorité de certification utilisée pour émettre des certificats pour l’authentification IPsec peut être utilisée. Le serveur de l’autorité de certification doit être disponible via le premier tunnel d’infrastructure.  
  
    3.  Sécurité au groupe exempter des utilisateurs à partir d’une authentification forte, un groupe de sécurité Active Directory contenant ces utilisateurs est requis.  
  
    4.  Client\-configuration requise côté-pour Windows 10 et Windows 8 les ordinateurs clients, l’Assistant connectivité réseau \(NCA\) service sert à détecter si les informations d’identification de secret à usage unique sont requises. Si elles sont, le Gestionnaire multimédia DirectAccess vous invite à entrer des informations d’identification.  L’application NCA est incluse dans le système d’exploitation, et aucune installation ou le déploiement n’est nécessaire. Pour les ordinateurs clients Windows 7, l’Assistant connectivité DirectAccess \(DCA\) 2.0 est requis. Il peut être téléchargé dans le [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=29039).  
  
    5.  Notez les points suivants :  
  
        1.  L’authentification OTP peut être utilisée en parallèle avec carte à puce et Module de plateforme sécurisée \(TPM\)\-en fonction de l’authentification. L’activation de l’authentification par mot de passe à usage unique dans la console de gestion de l’accès à distance permet également l’utilisation de l’authentification par carte à puce.  
  
        2.  Au cours de l’accès à distance configuration, les utilisateurs dans une sécurité spécifiée groupe peut être exempté de deux\-facteur d’authentification et ainsi s’authentifier avec le nom d’utilisateur\/mot de passe uniquement.  
  
        3.  Les modes de nouveau code confidentiel et de code de jeton suivant de mot de passe à usage unique ne sont pas pris en charge  
  
        4.  Dans un déploiement multisite de l’accès à distance, les paramètres de mot de passe à usage unique sont globaux et permettent l’identification pour tous les points d’entrée. Si plusieurs serveurs RADIUS ou d’autorité de certification sont configurés pour le mot de passe à usage unique, ils sont classés par chaque serveur d’accès à distance en fonction de leur disponibilité et proximité.  
  
        5.  Lorsque vous configurez le secret à usage unique dans un multiple de l’accès à distance\-environnement de forêt, les autorités de certification OTP doivent appartenir à la forêt de ressources uniquement, et l’inscription de certificats doit être configurée dans les approbations de forêt. Pour plus d’informations, consultez [AD CS : L’inscription de certificats inter-forêts avec Windows Server 2008 R2](https://technet.microsoft.com/library/ff955842.aspx).  
  
        6.  Les utilisateurs qui utilisent un jeton KEY FOB OTP doivent insérer le code confidentiel suivi par le code de jeton \(sans aucun séparateur\) dans la boîte de dialogue DirectAccess OTP. Les utilisateurs qui utilisent le jeton PIN PAD OTP doivent insérer uniquement le code de jeton dans la boîte de dialogue.  
  
        7.  Lorsque le WEBDAV est activé, le mot de passe à usage unique doit être désactivé.  
  
## <a name="KnownIssues"></a>Problèmes connus  
Les problèmes décrits ci-après sont connus et surviennent souvent lors de la configuration d’un scénario de mot de passe à usage unique :  
  
-   Accès à distance utilise un mécanisme de détection pour vérifier la connectivité à RADIUS\-en fonction des serveurs de secret à usage unique. Dans certains cas cela peut provoquer une erreur sur le serveur à mot de passe à usage unique. Pour éviter ce problème, procédez comme suit sur le serveur à mot de passe à usage unique :  
  
    -   Créez un compte d’utilisateur qui correspond au nom d’utilisateur et au mot de passe configurés sur le serveur d’accès à distance pour le mécanisme de détection. Le nom d’utilisateur ne doit pas définir un utilisateur Active Directory.  
  
        Par défaut, le nom d’utilisateur sur le serveur d’accès à distance est DAProbeUser et le mot de passe DAProbePass. Ces paramètres par défaut peuvent être modifiés à l’aide des valeurs suivantes dans le registre du serveur d’accès à distance :  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\RadiusProbeUser  
  
        -   HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\DirectAccess\\OTP\\ RadiusProbePass  
  
-   Si vous modifiez le certificat racine IPsec dans un déploiement DirectAccess configuré et en cours d’exécution, la fonctionnalité de mot de passe à usage unique cesse de fonctionner. Pour résoudre ce problème, sur chaque serveur DirectAccess, à l’invite Windows PowerShell, exécutez la commande : `iisreset`  
  
