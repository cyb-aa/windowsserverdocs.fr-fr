---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: Aide relative aux tests de clonage des contrôleurs de domaine virtualisés pour les fournisseurs d’applications
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0b2303bc837cdaf9f6e7ebd4b3ccbf6c66aa7ad2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879340"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Aide relative aux tests de clonage des contrôleurs de domaine virtualisés pour les fournisseurs d’applications

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique explique que les fournisseurs d’applications doivent prendre en compte afin de garantir que leur application continue à fonctionner comme prévu une fois le contrôleur de domaine virtualisé (DC) processus de clonage terminée. Il couvre les aspects du processus de clonage qui les fournisseurs d’applications qui vous intéresse et les scénarios qui peuvent justifier des tests supplémentaires. Fournisseurs d’applications qui ont validé que leur application fonctionne sur les contrôleurs de domaine virtualisés qui ont été clonés sont encouragés à répertorier le nom de l’application dans le contenu de la Communauté en bas de cette rubrique, ainsi qu’un lien vers votre site web entreprise où les utilisateurs plus d’informations sur la validation.  
  
## <a name="overview-of-virtualized-dc-cloning"></a>Vue d’ensemble du clonage de contrôleur de domaine virtualisé  
Le processus de clonage de contrôleur de domaine virtualisé est décrite en détail dans [Introduction à la virtualisation des Services de domaine Active Directory (AD DS) (niveau 100)](https://technet.microsoft.com/library/hh831734.aspx) et [de virtualiser les contrôleur de domaine technique Référence (niveau 300)](https://technet.microsoft.com/library/jj574214.aspx). Du point de vue d’un fournisseur d’application, il s’agit de quelques éléments à prendre en compte lors de l’évaluation de l’impact du clonage à votre application :  
  
-   L’ordinateur d’origine n’est pas détruit. Il reste sur le réseau, l’interaction avec les clients. Contrairement à un changement de nom dans laquelle les enregistrements DNS de l’ordinateur d’origine sont supprimés, les enregistrements d’origine pour le contrôleur de domaine source restent.  
  
-   Pendant le processus de clonage, le nouvel ordinateur est initialement en cours d’exécution pour une courte période de temps sous l’identité de l’ancien ordinateur jusqu'à ce que le processus de clonage est lancé et apporte les modifications nécessaires. Applications qui créent des enregistrements sur l’ordinateur hôte doivent garantir que l’ordinateur cloné ne remplace pas les enregistrements sur l’hôte d’origine pendant le processus de clonage.  
  
-   Le clonage est une fonctionnalité de déploiement spécifiques pour les contrôleurs de domaine virtualisés uniquement, pas une extension à usage général pour cloner des autres rôles de serveur. Certains rôles de serveur ne sont pas non plus pris en charge pour le clonage :  
  
    -   Protocole DHCP (Dynamic Host Configuration Protocol)  
  
    -   Services de certificats Active Directory (AD CS)  
  
    -   Services AD LDS (Active Directory Lightweight Directory Services)  
  
-   Dans le cadre du processus de clonage, la machine virtuelle entière qui représente le contrôleur de domaine d’origine est copiée, n’importe quel état de l’application sur cette machine virtuelle est également copié. Valider que l’application s’adapte à cette modification dans l’état de l’hôte local sur le contrôleur de domaine cloné, ou si aucune intervention n’est requise, par exemple, un redémarrage du service.  
  
-   Dans le cadre de clonage, le nouveau contrôleur de domaine obtient une nouvelle identité de l’ordinateur et dispositions lui-même en tant qu’un contrôleur de domaine réplica dans la topologie. Vérifie si l’application dépend de l’identité de l’ordinateur, telles que son nom, compte, SID et ainsi de suite. Il s’adaptent automatiquement à la modification de l’identité de l’ordinateur sur le clone ? Si cette application met en cache les données, assurez-vous qu’il ne repose pas sur les données d’identité de machine peuvent être mises en cache.  
  
## <a name="what-is-interesting-for-application-vendors"></a>Ce qui est intéressant pour les fournisseurs d’applications ?  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
Impossible de cloner un contrôleur de domaine qui exécute votre application ou service jusqu'à ce que l’application ou service est :  
  
-   Ajouté au fichier CustomDCCloneAllowList.xml à l’aide de l’applet de commande Get-ADDCCloningExcludedApplicationList Windows PowerShell  
  
- Ou -  
  
-   Supprimé à partir du contrôleur de domaine  
  
La première fois que l’utilisateur exécute l’applet de commande Get-ADDCCloningExcludedApplicationList, elle retourne une liste des services et applications qui sont exécutent sur le contrôleur de domaine, mais ne sont pas dans la liste par défaut des services et applications qui sont prises en charge pour le clonage. Par défaut, votre service ou application ne sera pas répertoriée. Pour ajouter votre service ou une application à la liste des applications et services qui peuvent être en toute sécurité cloné, les exécutions de l’utilisateur applet de commande Get-ADDCCloningExcludedApplicationList à nouveau avec l’option - GenerateXML pour l’ajouter au fichier CustomDCCloneAllowList.xml fichier. Pour plus d’informations, consultez [étape 2 : Exécutez l’applet de commande Get-ADDCCloningExcludedApplicationList](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet).  
  
### <a name="distributed-system-interactions"></a>Interactions de systèmes distribués  
Généralement services isolés à l’ordinateur local réussissent ou échouent lorsque participe de clonage. Services distribués ont besoin sur la présence de deux instances de l’ordinateur hôte sur le réseau simultanément pour une courte période de temps. Cela peut se manifester par une instance de service tente d’extraire des informations à partir d’un système partenaire où le clone a enregistré comme le nouveau fournisseur de l’identité. Ou les deux instances du service peuvent transmettre des informations à la base de données AD DS en même temps avec des résultats différents. Par exemple, il n’est pas déterministe de quel ordinateur sera communiquée avec lorsque deux ordinateurs qui ont le service de Windows Test Technologies (WTT) se trouvent sur le réseau avec le contrôleur de domaine.  
  
Pour le service serveur DNS distribué, le processus de clonage vous évite avec soin de remplacer les enregistrements DNS du contrôleur de domaine source lorsque le contrôleur de domaine clone démarre avec une nouvelle adresse IP.  
  
Ne vous fiez pas sur l’ordinateur pour supprimer tout l’ancienne identité jusqu'à la fin du clonage. Une fois le nouveau contrôleur de domaine est promu dans le nouveau contexte, sélectionnez Sysprep fournisseurs sont exécutés pour nettoyer l’état supplémentaire de l’ordinateur. Par exemple, il est à ce stade les anciens certificats de l’ordinateur sont supprimées et les secrets de chiffrement à l’ordinateur peut accéder sont modifiées.  
  
Le principal facteur qui fait varier le minutage du clonage est combien d’objets à répliquer à partir du contrôleur de domaine principal. Anciens médias augmente le temps nécessaire pour le clonage terminé.  
  
Étant donné que votre service ou une application est inconnue, il reste en cours d’exécution. Le processus de clonage ne modifie pas l’état des services de non Windows.  
  
En outre, le nouvel ordinateur a une adresse IP différente de l’ordinateur d’origine. Ces comportements peuvent entraîner des effets secondaires à votre service ou application en fonction de la manière dont le service ou l’application se comporte dans cet environnement.  
  
## <a name="additional-scenarios-suggested-for-testing"></a>Scénarios supplémentaires suggérées pour le test  
  
### <a name="cloning-failure"></a>Échec du clonage  
Les fournisseurs de services doivent tester ce scénario, car le clonage échoue lors de l’ordinateur démarre dans Services de réparation Mode annuaire (DSRM), un formulaire du Mode sans échec. À ce stade l’ordinateur non terminée le clonage. Un état peut avoir été modifiée et un état peut rester à partir du contrôleur de domaine d’origine. Tester ce scénario pour comprendre quel impact cela peut avoir sur votre application.  
  
Pour provoquer un échec de clonage, essayez de cloner un contrôleur de domaine sans lui accordant l’autorisation d’être cloné. Dans ce cas, l’ordinateur sera uniquement modifié les adresses IP tout en conservant la majorité de son état à partir du contrôleur de domaine d’origine. Pour plus d’informations sur l’octroi d’une autorisation de contrôleur de domaine d’être cloné, consultez [étape 1 : Accordez le contrôleur de domaine virtualisé source l’autorisation d’être cloné](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source).  
  
### <a name="pdc-emulator-cloning"></a>Le clonage d’émulateur PDC  
Les fournisseurs de service et d’application doivent tester ce scénario, car il existe un redémarrage supplémentaire lorsque l’émulateur PDC est cloné. En outre, la majorité de clonage est effectuée sous une identité temporaire pour autoriser le nouveau clone d’interagir avec l’émulateur PDC pendant le processus de clonage.  
  
### <a name="writable-versus-read-only-domain-controllers"></a>Accessible en écriture par rapport aux contrôleurs de domaine en lecture seule  
Les fournisseurs de service et d’application doivent tester clonage à l’aide du même type de contrôleur de domaine (autrement dit, sur un contrôleur de domaine accessible en écriture ou en lecture seule) que le service est planifié pour s’exécuter sur.  
  


