---
ms.assetid: fde99b44-cb9f-49bf-b888-edaeabe6b88d
title: Aide relative aux tests de clonage des contrôleurs de domaine virtualisés pour les fournisseurs d’applications
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4926fabe255f964b6d39e6c39c5e794a37423111
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77517464"
---
# <a name="virtualized-domain-controller-cloning-test-guidance-for-application-vendors"></a>Aide relative aux tests de clonage des contrôleurs de domaine virtualisés pour les fournisseurs d’applications

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique explique ce que les fournisseurs d’applications doivent prendre en compte pour garantir que leur application continue de fonctionner comme prévu après la fin du processus de clonage du contrôleur de domaine virtualisé. Il couvre les aspects du processus de clonage qui intéressent les fournisseurs d’applications et les scénarios susceptibles de justifier des tests supplémentaires. Les fournisseurs d’applications qui ont validé que leur application fonctionne sur les contrôleurs de domaine virtualisés qui ont été clonés sont encouragés à répertorier le nom de l’application dans le contenu de la communauté en bas de cette rubrique, ainsi qu’un lien vers votre site Web de l’organisation où les utilisateurs peuvent en savoir plus sur la validation.

## <a name="overview-of-virtualized-dc-cloning"></a>Vue d’ensemble du clonage de contrôleur de courant virtuel
Le processus de clonage des contrôleurs de domaine virtualisés est décrit en détail dans [Introduction à la Active Directory Domain Services (AD DS) virtualisation (niveau 100)](https://docs.microsoft.com/windows-server/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) et informations techniques de référence sur les [contrôleurs de domaine virtualisés (niveau 300)](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-). Du point de vue du fournisseur de l’application, les considérations à prendre en compte lors de l’évaluation de l’impact du clonage sur votre application sont les suivantes :

-   L’ordinateur d’origine n’est pas détruit. Il reste sur le réseau et interagit avec les clients. Contrairement à un changement de nom dans lequel les enregistrements DNS de l’ordinateur d’origine sont supprimés, les enregistrements d’origine du contrôleur de domaine source sont conservés.

-   Pendant le processus de clonage, le nouvel ordinateur est initialement exécuté pendant une courte période, sous l’identité de l’ancien ordinateur jusqu’à ce que le processus de clonage soit Initié et effectue les modifications nécessaires. Les applications qui créent des enregistrements sur l’hôte doivent s’assurer que l’ordinateur cloné ne remplace pas les enregistrements sur l’hôte d’origine pendant le processus de clonage.

-   Le clonage est une fonctionnalité de déploiement spécifique pour les contrôleurs de domaine virtualisés uniquement, et non pour un usage général pour cloner d’autres rôles de serveur. Certains rôles de serveur ne sont pas pris en charge pour le clonage :

    -   Protocole DHCP (Dynamic Host Configuration Protocol)

    -   Services de certificats Active Directory (AD CS)

    -   Services AD LDS (Active Directory Lightweight Directory Services)

-   Dans le cadre du processus de clonage, la totalité de la machine virtuelle qui représente le contrôleur de source d’origine est copiée, de sorte que tout état de l’application sur cette machine virtuelle est également copié. Vérifiez que l’application s’adapte à cette modification de l’état de l’hôte local sur le contrôleur de réseau cloné, ou si une intervention est nécessaire, comme un redémarrage du service.

-   Dans le cadre du clonage, le nouveau contrôleur de périphérique obtient une nouvelle identité d’ordinateur et provisionne lui-même en tant que contrôleur de périphérique de réplication dans la topologie. Vérifiez si l’application dépend de l’identité de l’ordinateur, par exemple son nom, son compte, son SID, etc. S’adapte-t-elle automatiquement à la modification de l’identité de l’ordinateur sur le clone ? Si cette application met en cache des données, assurez-vous qu’elle ne s’appuie pas sur les données d’identité de l’ordinateur qui peuvent être mises en cache.

## <a name="what-is-interesting-for-application-vendors"></a>Qu’est-ce qui est intéressant pour les fournisseurs d’applications ?

### <a name="customdccloneallowlistxml"></a>Fichier customdccloneallowlist. Xml
Un contrôleur de domaine qui exécute votre application ou service ne peut pas être cloné tant que l’application ou le service n’est pas :

-   Ajouté au fichier fichier customdccloneallowlist. XML à l’aide de l’applet de commande Windows PowerShell ADDCCloningExcludedApplicationList

\- Ou -

-   Supprimé du contrôleur de domaine

La première fois que l’utilisateur exécute l’applet de commande ADDCCloningExcludedApplicationList, il retourne une liste de services et d’applications qui s’exécutent sur le contrôleur de domaine, mais qui ne figurent pas dans la liste par défaut des services et applications pris en charge pour le clonage. Par défaut, votre service ou votre application ne seront pas listés. Pour ajouter votre service ou application à la liste des applications et des services qui peuvent être clonés en toute sécurité, l’utilisateur exécute à nouveau l’applet de commande ADDCCloningExcludedApplicationList à l’aide de l’option-GenerateXML afin de l’ajouter au fichier fichier customdccloneallowlist. Xml. Pour plus d’informations, consultez [étape 2 : exécuter l’applet de commande obtenir-ADDCCloningExcludedApplicationList](https://docs.microsoft.com/powershell/module/addsadministration/get-addccloningexcludedapplicationlist).

### <a name="distributed-system-interactions"></a>Interactions du système distribué
Généralement, les services isolés sur l’ordinateur local réussissent ou échouent lors de la participation au clonage. Les services distribués doivent se préoccuper de l’utilisation simultanée de deux instances de l’ordinateur hôte sur le réseau pendant une courte période. Cela peut se manifester sous la forme d’une instance de service tentant d’extraire des informations d’un système partenaire sur lequel le clone a été enregistré en tant que nouveau fournisseur de l’identité. Ou les deux instances du service peuvent envoyer des informations dans la base de données AD DS en même temps avec des résultats différents. Par exemple, il n’est pas déterministe pour quel ordinateur sera communiqué quand deux ordinateurs sur lesquels le service WTT (Windows Testing Technologies) se trouve sur le réseau avec le contrôleur de domaine.

Pour le service serveur DNS distribué, le processus de clonage évite soigneusement le remplacement des enregistrements DNS du contrôleur de domaine source lorsque le contrôleur de domaine clone commence par une nouvelle adresse IP.

Vous ne devez pas compter sur l’ordinateur pour supprimer l’ancienne identité jusqu’à la fin du clonage. Une fois que le nouveau contrôleur de domaine est promu dans le nouveau contexte, sélectionnez les fournisseurs Sysprep exécutés pour nettoyer l’État supplémentaire de l’ordinateur. Par exemple, c’est à ce stade que les anciens certificats de l’ordinateur sont supprimés et les secrets de chiffrement auxquels l’ordinateur peut accéder sont modifiés.

Le plus grand facteur qui varie le temps de clonage est le nombre d’objets à répliquer à partir du contrôleur de domaine principal. Un média plus ancien augmente le temps nécessaire pour effectuer le clonage.

Étant donné que votre service ou application est inconnu, il est laissé en cours d’exécution. Le processus de clonage ne modifie pas l’état des services non-Windows.

En outre, le nouvel ordinateur a une adresse IP différente de celle de l’ordinateur d’origine. Ces comportements peuvent entraîner des effets secondaires sur votre service ou application en fonction de la façon dont le service ou l’application se comporte dans cet environnement.

## <a name="additional-scenarios-suggested-for-testing"></a>Scénarios supplémentaires suggérés pour le test

### <a name="cloning-failure"></a>Échec de clonage
Les fournisseurs de services doivent tester ce scénario, car lorsque le clonage échoue, l’ordinateur démarre en mode de réparation des services d’annuaire (DSRM), une forme de mode sans échec. À ce stade, l’ordinateur n’a pas terminé le clonage. Certains États peuvent avoir changé et certains États peuvent être conservés à partir du contrôleur de domaine d’origine. Testez ce scénario pour comprendre l’impact qu’il peut avoir sur votre application.

Pour provoquer un échec de clonage, essayez de cloner un contrôleur de domaine sans lui accorder l’autorisation d’être cloné. Dans ce cas, l’ordinateur aura uniquement modifié les adresses IP et aura toujours la majeure partie de son état du contrôleur de domaine d’origine. Pour plus d’informations sur l’octroi de l’autorisation de clonage d’un contrôleur de domaine, voir [étape 1 : accorder au contrôleur de domaine virtualisé source l’autorisation d’être cloné](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/virtualized-domain-controller-deployment-and-configuration).

### <a name="pdc-emulator-cloning"></a>Clonage de l’émulateur PDC
Les fournisseurs de services et d’applications doivent tester ce scénario, car il y a un redémarrage supplémentaire lorsque l’émulateur de contrôleur de domaine principal est cloné. En outre, la majorité du clonage est effectuée sous une identité temporaire pour permettre au nouveau clone d’interagir avec l’émulateur de contrôleur de domaine principal pendant le processus de clonage.

### <a name="writable-versus-read-only-domain-controllers"></a>Contrôleurs de domaine accessibles en écriture et en lecture seule
Les fournisseurs de services et d’applications doivent tester le clonage en utilisant le même type de contrôleur de domaine (autrement dit, sur un contrôleur de domaine accessible en écriture ou en lecture seule) sur lequel le service est planifié pour s’exécuter.
