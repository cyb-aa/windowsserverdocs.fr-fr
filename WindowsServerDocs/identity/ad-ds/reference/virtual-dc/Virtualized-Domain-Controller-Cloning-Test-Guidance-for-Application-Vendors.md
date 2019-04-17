---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: "Conseils de Test pour les fournisseurs d’applications de clonage de contrôleur de domaine virtualisés"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 72c4e818f82d3252c45776b26fb59e095893f2c7
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Conseils de Test pour les fournisseurs d’applications de clonage de contrôleur de domaine virtualisés

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Cette rubrique explique ce que les fournisseurs d’applications doivent prendre en compte pour s’assurer de que leur application continue de fonctionner comme prévu après la fin du processus de clonage du contrôleur de domaine virtualisé (DC). Elle couvre les aspects du processus de clonage que les fournisseurs d’applications vos centres d’intérêt et les scénarios qui peuvent justifier des tests supplémentaires. Les fournisseurs d’applications qui ont validé que leur application fonctionne sur les contrôleurs de domaine virtualisé qui ont été clonés sont invités à obtenir le nom de l’application dans le contenu de la Communauté en bas de cette rubrique, ainsi que d’un lien vers le site web de votre organisation où les utilisateurs peuvent en savoir plus sur la validation.  
  
## <a name="overview-of-virtualized-dc-cloning"></a>Vue d’ensemble de clonage de contrôleur de domaine virtualisé  
Le processus de clonage de contrôleur de domaine virtualisé est décrite en détail dans [Introduction aux Services de domaine Active Directory (AD DS) Virtualization (Level 100)](https://technet.microsoft.com/library/hh831734.aspx) et [virtualisés domaine contrôleur techniques de référence (niveau 300)](https://technet.microsoft.com/library/jj574214.aspx). Il s’agit du point de vue d’un fournisseur d’application, certaines considérations à prendre en compte lors de l’évaluation de l’impact du clonage à votre application:  
  
-   L’ordinateur d’origine n’est pas détruit. Il reste sur le réseau, l’interaction avec les clients. Contrairement à un changement de nom dans laquelle les enregistrements DNS de l’ordinateur d’origine sont supprimés, les enregistrements pour le contrôleur de domaine source d’origine restent.  
  
-   Pendant le processus de clonage, le nouvel ordinateur est initialement en cours d’exécution pour une courte période de temps sous l’identité de l’ancien ordinateur jusqu'à ce que le processus de clonage est lancé et apporte les modifications nécessaires. Les applications qui créent des enregistrements sur l’ordinateur hôte doivent garantir que l’ordinateur cloné ne remplace pas les enregistrements sur l’hôte d’origine pendant le processus de clonage.  
  
-   Le clonage est une fonctionnalité de déploiement spécifiques pour les contrôleurs de domaine virtualisés uniquement, pas une extension d’usage général pour cloner des autres rôles de serveur. Certains rôles de serveur ne sont pas pris en charge pour le clonage:  
  
    -   Dynamic Host Configuration Protocol (DHCP)  
  
    -   Services de certificats ActiveDirectory (ADCS)  
  
    -   ActiveDirectory Lightweight Directory Services (ADLDS)  
  
-   Dans le cadre du processus de clonage, l’ordinateur virtuel entier qui représente le contrôleur de domaine d’origine est copié, n’importe quel état de l’application sur cet ordinateur virtuel est également copié. Valider que l’application s’adapte à cette modification dans l’état de l’hôte local sur le contrôleur de domaine cloné, ou si aucune intervention n’est nécessaire, par exemple, un redémarrage du service.  
  
-   Dans le cadre de clonage, le nouveau contrôleur de domaine obtient une nouvelle identité d’ordinateur et dispositions lui-même en tant que réplica du contrôleur de domaine dans la topologie. Vérifie si l’application dépend de l’identité de l’ordinateur, telles que son nom, compte, SID et ainsi de suite. Est qu’il s’adaptent automatiquement à la modification de l’identité de l’ordinateur sur le clone? Si cette application met en cache les données, assurez-vous qu’il ne s’appuie pas sur les données d’identité d’ordinateur peuvent être mis en cache.  
  
## <a name="what-is-interesting-for-application-vendors"></a>Nouveautés intéressante pour les fournisseurs d’applications  
  
### <a name="customdccloneallowlistxml"></a>CustomDCCloneAllowList.xml  
Impossible de cloner un contrôleur de domaine qui exécute votre application ou service jusqu'à ce que l’application ou service est:  
  
-   Ajouté au fichier CustomDCCloneAllowList.xml à l’aide de l’applet de commande Get-ADDCCloningExcludedApplicationList Windows PowerShell  
  
- Ou -  
  
-   Retirez le contrôleur de domaine  
  
La première fois que l’utilisateur exécute l’applet de commande Get-ADDCCloningExcludedApplicationList, il renvoie une liste des services et applications qui sont exécutent sur le contrôleur de domaine, mais ne sont pas dans la liste par défaut des services et applications qui sont pris en charge pour le clonage. Par défaut, votre service ou une application ne sera pas être répertoriée. Pour ajouter votre service ou application à la liste des applications et services qui peuvent être en toute sécurité cloné, l’exécution de l’utilisateur applet de commande Get-ADDCCloningExcludedApplicationList à nouveau avec l’option - GenerateXML afin de l’ajouter au fichier CustomDCCloneAllowList.xml fichier. Pour plus d’informations, voir [étape 2: applet de commande Get-ADDCCloningExcludedApplicationList exécuter](https://technet.microsoft.com/library/hh831734.aspx#bkmk6_run_get_addccloningexcludedapplicationlist_cmdlet).  
  
### <a name="distributed-system-interactions"></a>Interactions avec le système distribué  
Généralement services isolés à l’ordinateur local réussite ou échouent lorsque participe le clonage. Services distribués ont à vous préoccuper de la présence de deux instances de l’ordinateur hôte sur le réseau simultanément pour une courte période de temps. Cela peut se manifester par une instance de service tente d’extraire des informations à partir d’un partenaire où le clone a inscrit en tant que le nouveau fournisseur d’identité. Ou les deux instances du service de peuvent pousser des informations dans la base de données AD DS en même temps avec des résultats différents. Par exemple, il n’est pas déterministe de l’ordinateur qui est communiqué avec lorsque les deux ordinateurs qui ont le service Windows Test Technologies (WTT) sont sur le réseau avec le contrôleur de domaine.  
  
Pour le service serveur DNS distribué, le processus de clonage évite avec soin de remplacer les enregistrements DNS du contrôleur de domaine source lorsque le contrôleur de domaine clone démarre avec une nouvelle adresse IP.  
  
Ne vous fiez pas sur l’ordinateur pour supprimer tous les de l’ancienne identité jusqu'à la fin du clonage. Une fois le nouveau contrôleur de domaine est promu à l’intérieur du nouveau contexte, sélectionnez fournisseurs sont exécutés pour nettoyer l’état de l’ordinateur supplémentaire de Sysprep. Par exemple, il est à ce stade les anciens certificats de l’ordinateur sont supprimés et les secrets de chiffrement qui peut accéder à l’ordinateur sont modifiés.  
  
Le facteur le plus important qui varie en fonction de la synchronisation du clonage est combien d’objets à répliquer à partir du contrôleur de domaine principal. Support plus ancien augmente le temps nécessaire pour le clonage terminé.  
  
Étant donné que votre service ou une application est inconnue, elle reste en cours d’exécution. Le processus de clonage ne modifie pas l’état des services non Windows.  
  
En outre, le nouvel ordinateur dispose d’une autre adresse IP à l’ordinateur d’origine. Ces comportements peuvent provoquer des effets à votre service ou application en fonction de la façon dont le service ou l’application se comporte dans cet environnement.  
  
## <a name="additional-scenarios-suggested-for-testing"></a>Autres scénarios suggérés de test  
  
### <a name="cloning-failure"></a>Un échec du clonage  
Les fournisseurs de service doivent tester ce scénario, car le clonage échoue lors de l’ordinateur démarre dans Services de réparation Mode annuaire (DSRM), une forme de Mode sans échec. À ce stade l’ordinateur n'a pas terminé le clonage. Certains États a changé et certains États peuvent rester à partir du contrôleur de domaine d’origine. Tester ce scénario pour comprendre l’impact qu’elle peut avoir sur votre application.  
  
Pour provoquer un échec du clonage, essayez de cloner un contrôleur de domaine sans autorisation d’être cloné. Dans ce cas, l’ordinateur sera uniquement modifié les adresses IP tout en conservant la majeure partie de son état à partir du contrôleur de domaine d’origine. Pour plus d’informations sur l’octroi d’autorisation d’un contrôleur de domaine d’être cloné, voir [étape 1: accorder le contrôleur de domaine virtualisé source l’autorisation d’être cloné](https://technet.microsoft.com/library/hh831734.aspx#bkmk4_grant_source).  
  
### <a name="pdc-emulator-cloning"></a>Émulateur PDC le clonage  
Les fournisseurs de service et de l’applications doivent tester ce scénario, car il existe un autre redémarrage lors de l’émulateur PDC est cloné. En outre, la majorité de clonage est effectuée sous une identité temporaire pour autoriser le nouveau clone d’interagir avec l’émulateur de contrôleur de domaine principal durant le processus de clonage.  
  
### <a name="writable-versus-read-only-domain-controllers"></a>Accessible en écriture par rapport aux contrôleurs de domaine en lecture seule  
Les fournisseurs de service et de l’applications doivent tester le clonage à l’aide du même type de contrôleur de domaine (autrement dit, sur un contrôleur de domaine accessible en écriture ou en lecture seule) que le service est planifié pour s’exécuter sur.  
  


