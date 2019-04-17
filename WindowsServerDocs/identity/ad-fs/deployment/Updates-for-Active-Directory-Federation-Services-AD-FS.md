---
ms.assetid: ed3206b4-bbfc-4bc7-a43c-981b0544a50d
title: "Mises à jour requises pour Active Directory Federation Services (ADFS)"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 434bf65c9f5ec977318b5efcdeebd19a5f1ed475
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="required-updates-for-active-directory-federation-services-ad-fs-and-web-application-proxy-wap"></a>Mises à jour requises pour Active Directory Federation Services (ADFS) et Web Application Proxy (WAP)

>S’applique à: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 SP1

À compter d’octobre 2016, toutes les mises à jour pour tous les composants de Windows Server sont publiées uniquement via Windows Update (WU).  Il existe aucun correctif plus ou téléchargements individuels.
Cela s’applique à Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 et Windows Server 2008 R2 SP1.

Cette page répertorie les packages de déploiement de vos centres d’intérêt particulier pour AD FS et WAP, ainsi que la liste d’historique des mises à jour de correctifs logiciels recommandés pour AD FS et WAP.

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2016"></a>Mises à jour pour AD FS et WAP dans Windows Server 2016
Mises à jour pour Windows Server 2016 sont fournis chaque mois via Windows Update et sont cumulatifs. Le package de mise à jour répertorié ci-dessous est recommandé pour tous les serveurs AD FS et WAP 2016 et inclut toutes les mises à jour précédemment requises, ainsi que les derniers correctifs.

|BASE DE CONNAISSANCES N ° |Description|Date de sortie
|----- | ----- |-----
|[4041688 (Build du système d’exploitation 14393.1794)](https://support.microsoft.com/kb/4041688)|Ce correctif résout un problème qui par intermittence dirige de façon incorrecte demandes AD autorité au fournisseur d’identité incorrect en raison du comportement de mise en cache incorrect. Cela peut avoir un impact fonctionnalités telles que l’authentification par plusieurs facteurs d’authentification. </br></br>Ajouté la possibilité de contrôle d’intégrité de la connexion AAD au rapport d’intégrité du serveur ADFS avec une fidélité appropriée (à l’aide de l’audit verbose) sur des batteries de serveurs WS2012R2 et WS2016ADFS mixtes.</br></br>Corrige un problème où lors de la mise à niveau de 2012R2 ADFS de batterie de serveurs à ADFS2016, l’applet de commande powershell pour augmenter le niveau de comportement de la batterie de serveurs échoue avec un délai d’expiration lorsqu’il existe plusieurs approbations de partie de confiance.</br></br>Résolution d’un problème où ADFS entraîne des échecs d’authentification en modifiant la valeur du paramètre wct lors de la fédération les demandes aux autres serveur STS (Security Token).|Octobre2017|
|[4038801 (Build du système d’exploitation 14393.1737)](https://support.microsoft.com/kb/4038801)|Prise en charge de journal OIDC à l’aide de LDPs fédérées. Cela permettra de "Scénarios de plein écran", où plusieurs utilisateurs peuvent être enregistrées en série en un seul périphérique où il est fédération avec une LDP.</br></br>Résolu un problème où CEP/CES en fonction des certificats ne fonctionnent pas avec les comptes de service administré de groupe de WinHello.</br></br>Résout un problème dans lequel la base de données interne Windows (WID) sur les serveurs Windows Server2016ADFS échoue synchroniser certains paramètres, tels que les colonnes des tables IdentityServerPolicy.Scopes et IdentityServerPolicy.Clients ApplicationGroupId) en raison d’une clé étrangère contrainte. Ces échecs de synchronisation peut provoquer la revendication différent, revendication expériences fournisseur et application entre les serveurs ADFS principales vers le site secondaire. En outre, si le rôle principal WID est déplacé vers un nœud secondaire, les groupes d’application ne sera plus facile à gérer dans la gestion ADFS UX.</br></br>Cette mise à jour résout un problème où l’authentification à plusieurs facteurs ne fonctionne pas correctement avec des périphériques mobiles qui utilisent des définitions de culture personnalisée|Septembre2017|
|[4034661 (Build du système d’exploitation 14393.1613)](https://support.microsoft.com/kb/4034661)|Résout un problème où l’adresse IP de l’appelant est nog enregistré par les événements dans le journal des événements de sécurité de 4.0ADFS 411 \ serveurs ADFS de Windows Server2016 RS1 même après avoir activé "les audits des succès" et "les audits des échecs".</br></br>Ce correctif résout un problème avec Azure Multi facteur d’authentification (MFA) lorsqu’un serveur ADFX est configuré pour utiliser un HTTP Proxy.</br></br>"Résolution d’un problème dans lequel présenter un certificat expiré ou révoqué pour le serveur Proxy ADFS ne retourne une erreur à l’utilisateur".|Août2017|
|[4034658 (Build du système d’exploitation 14393.1593)](https://support.microsoft.com/kb/4034658)|Résolution d’ActiveDirectory2016 serveur FS pour prendre en charge l’inscription de certificats de l’authentification Multifacteur pour Windows Hello pour entreprises pour les déploiements de site|Août2017| 
|[4025334 (Build du système d’exploitation 14393.1532)](https://support.microsoft.com/kb/4025334)|Résolution d’un problème dans lequel le Gestionnaire de jeton PkeyAuth peut échouer l’authentification si la demande pkeyauth contient des données incorrectes. L’authentification doit continuer sans effectuer l’authentification des appareils|Juillet2017|
|[4022723 (Build du système d’exploitation 14393.1378)](https://support.microsoft.com/kb/4022723)|[Le Proxy d’Application web] Valeur de propriété de configuration DisableHttpOnlyCookieProtection n’est pas sélectionnée par WAP2016dans un déploiement mixte 2012R2/2016 </br></br>[Le Proxy d’Application web] Impossible d’obtenir un jeton d’accès utilisateur à partir d’ADFS dans les scénarios EAS Pre-auth.</br></br>ADFS2016: WSFED déconnexion conduit à une exception|Juin2017 
|[3213986](https://support.microsoft.com/kb/3213986)|Mise à jour cumulative pour Windows Server 2016 pour les systèmes x64 64 (KB3213986)| Janvier 2017

## <a name="updates-for-ad-fs-and-wap-in-windows-server-2012-r2"></a>Mises à jour pour AD FS et WAP dans Windows Server 2012 R2
Vous trouverez ci-dessous la liste des correctifs cumulatifs correctifs et mises à jour qui ont été publiées pour Active Directory Federation Services (ADFS) dans Windows Server 2012 R2.

|BASE DE CONNAISSANCES N ° |Description|Date de sortie
|----- | ----- |-----
|[4041685](https://support.microsoft.com/kb/4041685)|Résolution d’un problème d’ADFS où MSISConext les cookies dans les en-têtes de demande peuvent éventuellement la limite de taille des en-têtes de dépassement de capacité et provoquer l’échec d’authentification avec le code d’état HTTP 400 "Défectueux demande – en-tête trop Long".</br></br>Corrige un problème dans lequel ADFS ne peuvent plus ignorer "prompt = connexion" lors de l’authentification. Une option "Désactivé" a été ajoutée pour restaurer les scénarios où l’authentification non-mot de passe est utilisée.|Version préliminaire d’octobre 2017 de mise à jour cumulative|
|[4019217](https://support.microsoft.com/kb/4019217)|Les clients à l’aide de broker à jetons ne fonctionnent pas lorsque vous utilisez un serveur Server2012R2 ADFS de dossiers de travail|Correctif cumulatif de la version d’évaluation mai2017| 
|[4015550](https://support.microsoft.com/kb/4015550)|Résolu un problème avec ADFS ne pas l’authentification des utilisateurs externes et ADFS WAP aléatoirement échouent transférer la demande|Correctif cumulatif d’avril 2017| 
|[4015547](https://support.microsoft.com/kb/4015547)|Résolu un problème avec ADFS ne pas l’authentification des utilisateurs externes et ADFS WAP aléatoirement échouent transférer la demande|Mise à jour de sécurité avril2017| 
|[4012216](https://support.microsoft.com/kb/4009970)|MS17-019 cette mise à jour de sécurité résout un problème dans ActiveDirectory Federation Services (ADFS). Cette vulnérabilité pourrait permettre la divulgation d’informations si une personne malveillante envoie une demande spécialement vers un serveur ADFS, ce qui permet la personne malveillante de lire les informations sensibles sur le système cible.|Correctif cumulatif de mise à jour de mars2017| 
|[3179574](https://support.microsoft.com/kb/3179574)|Résolution du problème de mise à jour du mot de passe extranet AD FS. |Correctif cumulatif de mise à jour août 2016
|[3172614](https://support.microsoft.com/kb/3172614)|Invite introduite = connexion [prennent en charge](https://technet.microsoft.com/en-us/windows-server-docs/identity/ad-fs/overview/ad-fs-faq#BKMK_7), résolu un problème avec la console de gestion AD FS et le paramètre AlwaysRequireAuthentication. |Correctif cumulatif de mise à jour de juillet 2016
|[3163306](https://support.microsoft.com/kb/3163306)|Active Directory Federation Services (ADFS) 3.0 ne peut pas se connecter à des magasins d’attributs Active accès protocole LDAP (Lightweight) qui sont configurés pour utiliser Secure Sockets Layer (SSL) le port 636 ou 3269 dans la chaîne de connexion. |Correctif cumulatif de mise à jour de juin 2016
|[3148533](https://support.microsoft.com/kb/3148533)|Échec de l’authentification de secours l’authentification Multifacteur via le Proxy d’ADFS dans Windows Server 2012 R2 |Mai 2016
|[3134787](https://support.microsoft.com/kb/3134787)|Adresse IP du client pour les scénarios de verrouillage de compte dans Windows Server 2012 R2 ne contiennent pas les journaux AD FS |Février 2016
|[3134222](https://support.microsoft.com/kb/3134222)|MS16-020: Mise à jour de sécurité pour Active Directory Federation Services déni d’adresse de service: 9 février 2016|Février 2016
|[3105881](https://support.microsoft.com/kb/3105881)|Ne peut pas accéder aux applications lorsque l’authentification des appareils est activée dans le serveur de base de Windows Server 2012 R2 AD FS|Octobre 2015
|[3092003](https://support.microsoft.com/kb/3092003)|Page est chargée de manière répétée et l’authentification échoue lorsque les utilisateurs utilisent l’authentification Multifacteur dans AD FS de Windows Server 2012 R2|Août 2015
|[3080778](https://support.microsoft.com/kb/3080778)|AD FS n’appelle pas OnError lors de la carte de l’authentification Multifacteur lève une exception dans Windows Server 2012 R2|Juillet 2015
|[3075610](https://support.microsoft.com/kb/3075610)|Relations d’approbation sont perdues sur le serveur AD FS secondaire une fois que vous ajoutez ou supprimez un fournisseur de revendications dans Windows Server 2012 R2|Juillet 2015
|[3070080](https://support.microsoft.com/kb/3070080)|Découverte de domaine d’accueil ne fonctionne ne pas correctement pour les revendications Non prenant en charge de confiance|Juin 2015
|[3052122](https://support.microsoft.com/kb/3052122)|Mise à jour ajoute la prise en charge des revendications d’ID composées de jetons AD FS dans Windows Server 2012 R2|Mai 2015
|[3045711](https://support.microsoft.com/kb/3045711)|MS15-040: Une vulnérabilité dans Active Directory Federation Services pourrait permettre la divulgation d’informations|Avril 2015
|[3042127](https://support.microsoft.com/kb/3042127)|«HTTP 400 - demande incorrecte» erreur lorsque vous ouvrez une boîte aux lettres partagé par le biais de WAP dans Windows Server 2012 R2|Mars 2015
|[3042121](https://support.microsoft.com/kb/3042121)|Protection de relecture de jetons AD FS pour les jetons d’authentification Proxy d’Application Web dans Windows Server 2012 R2|Mars 2015
|[3035025](https://support.microsoft.com/kb/3035025)|Correctif pour le mot de passe de mise à jour de fonctionnalité afin que les utilisateurs ne doivent pas utiliser l’appareil inscrit dans Windows Server 2012 R2|Janvier 2015
|[3033917](https://support.microsoft.com/kb/3033917)|AD FS ne peut pas traiter la réponse SAML dans Windows Server 2012 R2|Janvier 2015
|[3025080](https://support.microsoft.com/kb/3025080)|Échec de l’opération lorsque vous essayez d’enregistrer un fichier Office via le Proxy d’Application Web dans Windows Server 2012 R2|Janvier 2015
|[3025078](https://support.microsoft.com/kb/3025078)|Vous n'êtes pas invité pour le nom d’utilisateur à nouveau lorsque vous utilisez un nom d’utilisateur incorrect pour vous connecter à Windows Server 2012 R2|Janvier 2015
|[3020813](https://support.microsoft.com/kb/3020813)|Vous êtes invité pour l’authentification lorsque vous exécutez une application web dans Windows Server 2012 R2 AD FS|Janvier 2015
|[3020773](https://support.microsoft.com/kb/3020773)|Échecs de délai d’attente après le déploiement initial du service d’inscription de l’appareil dans Windows Server 2012 R2|Janvier 2015
|[3018886](https://support.microsoft.com/kb/3018886)|Vous êtes invité à entrer un nom d’utilisateur et un mot de passe deux fois lorsque vous accédez à serveur AD FS de Windows Server 2012 R2 à partir de l’intranet|Janvier 2015
|[3013769](https://support.microsoft.com/kb/3013769)|Windows Server 2012 R2 mise à jour cumulées|Décembre 2014
|[3000850](https://support.microsoft.com/kb/3000850)|Windows Server 2012 R2 mise à jour cumulées|Novembre 2014
|[2975719](https://support.microsoft.com/kb/2975719)|Windows Server 2012 R2 mise à jour cumulées|Août 2014
|[2967917](https://support.microsoft.com/kb/2967917)|Windows Server 2012 R2 mise à jour cumulées|Juillet 2014
|[2962409](https://support.microsoft.com/kb/2962409)|Windows Server 2012 R2 mise à jour cumulées|Juin 2014
|[2955164](https://support.microsoft.com/kb/2955164)|Windows Server 2012 R2 mise à jour cumulées|Mai 2014
|[2919355](https://support.microsoft.com/kb/2919355)|Windows Server 2012 R2 mise à jour cumulées|Avril 2014

## <a name="updates-for-ad-fs-in-windows-server-2012-ad-fs-21-and-ad-fs-20"></a>Mises à jour pour AD FS dans Windows Server 2012 (AD FS 2.1) et les services AD FS 2.0
Vous trouverez ci-dessous la liste des correctifs cumulatifs correctifs et mises à jour qui ont été publiées pour AD FS 2.0 et 2.1.

|BASE DE CONNAISSANCES N ° |Description|Date de sortie|S’applique à:
|----- | ----- |-----|-----
|[3197878](https://support.microsoft.com/kb/3197878)|L’authentification via le proxy échoue dans Windows Server 2012 (il s’agit de la version du correctif 3094446 générale)|Correctif cumulatif de qualité de novembre 2016|AD FS 2.1
|[3197869](https://support.microsoft.com/kb/3197869)|L’authentification via le proxy échoue dans Windows Server 2008 R2 SP1 (il s’agit de la version du correctif 3094446 générale)|Correctif cumulatif de qualité de novembre 2016|AD FS 2.0
|[3094446](https://support.microsoft.com/kb/3094446)|L’authentification via le proxy échoue dans Windows Server 2012 ou Windows Server 2008 R2 SP1|Septembre 2015|AD FS 2.0 et 2.1
|[3070078](https://support.microsoft.com/kb/3070078)|AD FS 2.1 lève une exception lorsque vous authentifiez par rapport à un certificat de chiffrement dans Windows Server 2012|Juillet 2015|AD FS 2.1
|[3062577](https://support.microsoft.com/kb/3062577)|MS15-062: Une vulnérabilité dans Active Directory federation services pourrait permettre une élévation de privilèges|Juin 2015|AD FS 2.0 / 2.1
|[3003381](https://support.microsoft.com/kb/3003381)|MS14-077: Une vulnérabilité dans Active Directory Federation Services pourrait permettre la divulgation d’informations: 14 avril 2015|Novembre 2014|AD FS 2.0 / 2.1
|[2987843](https://support.microsoft.com/kb/2987843)|Utilisation de la mémoire du serveur de fédération AD FS continue d’augmenter lorsque de nombreux utilisateurs ouvrent une session sur une application web dans Windows Server 2012|Juillet 2014|AD FS 2.1
|[2957619](https://support.microsoft.com/kb/2957619)|La partie de confiance dans AD FS est arrêté lorsqu’une demande est effectuée pour AD FS pour un jeton de délégation|Mai 2014|AD FS 2.1
|[2926658](https://support.microsoft.com/kb/2926658)|Déploiement de la batterie de serveurs SQL ADFS échoue si vous ne disposez pas des autorisations SQL|Octobre 2014|AD FS 2.1
|[2896713](https://support.microsoft.com/kb/2896713) ou [2989956](https://support.microsoft.com/kb/2989956)|Mise à jour est disponible pour corriger plusieurs problèmes après avoir installé la mise à jour de sécurité 2843638 sur un serveur AD FS|Novembre 2013</br></br>Septembre 2014|AD FS 2.0 / 2.1
|[2877424](https://support.microsoft.com/kb/2877424)|Mise à jour vous permet d’utiliser un certificat pour plusieurs partie de confiance approbations dans un AD FS batterie 2.1|Octobre 2013|AD FS 2.1
|[2873168](https://support.microsoft.com/kb/2873168)|CORRECTIF: Une erreur se produit lorsque vous utilisez un fournisseur CSP et HSM et ensuite configurez une approbation de fournisseur de revendications dans le correctif cumulatif 3 pour AD FS 2.0 sur Windows Server 2008 R2 Service Pack 1|Septembre 2013|AD FS 2.0
|[2861090](https://support.microsoft.com/kb/2861090)|Une virgule dans le nom du sujet d’un certificat de chiffrement provoque une exception dans Windows Server 2008 R2 SP1|Août 2013|AD FS 2.0
|[2843639](https://support.microsoft.com/kb/2843639)|[Sécurité] Une vulnérabilité dans Active Directory Federation Services pourrait permettre la divulgation d’informations|Novembre 2013|AD FS 2.1
|[2843638](https://support.microsoft.com/kb/2843638)|MS13-066: Description de la mise à jour de sécurité pour Active Directory Federation Services 2.0: 13 août 2013|Août 2013|AD FS 2.0
|[2827748](https://support.microsoft.com/kb/2827748)|Fichier federationmetadata.XML ne contient pas les informations de point de terminaison MEX pour les points de terminaison WS-Trust et WS-Federation dans Windows Server 2012|Mai 2013|AD FS 2.1
|[2790338](https://support.microsoft.com/kb/2790338)|Description du correctif cumulatif 3 pour Active Directory Federation Services (ADFS) 2.0|Mars 2013|AD FS 2.0




