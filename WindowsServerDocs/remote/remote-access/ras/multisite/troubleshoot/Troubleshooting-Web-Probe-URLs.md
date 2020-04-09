---
title: Dépannage des URL de sonde Web
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 6dfffd1e-f4f4-43b6-9e3c-49015ce34338
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 181a609057d9735701a3185f03be96adb5751e47
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858262"
---
# <a name="troubleshooting-web-probe-urls"></a>Dépannage des URL de sonde Web

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique contient des informations de résolution des problèmes liés à la commande `Set-DAEntryPointDC`. Pour confirmer que l’erreur que vous avez reçue est liée à la définition du contrôleur de domaine du point d’entrée, recherchez l’ID d’événement 10065 dans le journal des événements Windows.  
  
## <a name="saving-server-gpo-settings"></a><a name="SaveGPOSettings"></a>Enregistrement des paramètres d’objet de stratégie de groupe de serveur  
**Erreur reçue**. Une erreur s’est produite lors de l’enregistrement des paramètres d’accès à distance dans l’objet de stratégie de groupe < GPO_name >.  
  
Pour corriger cette erreur, voir Enregistrement des paramètres de l’objet de stratégie de groupe serveur.  
  
## <a name="remote-access-is-not-configured"></a>Accès à distance non configuré  
**Erreur reçue**. L’accès à distance n’est pas configuré sur < server_name >. Spécifiez le nom d’un serveur qui appartient à un déploiement multisite.  
  
Ou  
  
L’accès à distance n’est pas configuré sur le serveur < server_name >. Spécifiez un ordinateur sur lequel DirectAccess est activé.  
  
**Cause**  
  
L’accès à distance n’est pas configuré sur l’ordinateur spécifié par le paramètre *ComputerName*.  
  
L’applet de commande `Set-DaEntryPointDC` est uniquement disponible sur les serveurs qui font partie d’un déploiement multisite configuré.  
  
**Solution**  
  
Exécutez la commande et assurez-vous de spécifier le paramètre *ComputerName* à l’aide du nom du serveur qui est déjà configuré dans le cadre du déploiement multisite.  
  
## <a name="multisite-is-not-enabled"></a>Multisite non activé  
**Erreur reçue**. Vous devez activer un déploiement multisite avant d’effectuer cette opération. Pour ce faire, utilisez l’applet de commande `Enable-DAMultiSite`.  
  
**Cause**  
  
Le déploiement multisite n’est pas activé sur le serveur spécifié par le paramètre *ComputerName*.  
  
L’applet de commande `Set-DaEntryPointDC` est uniquement disponible sur les serveurs qui font partie d’un déploiement multisite configuré.  
  
**Solution**  
  
Exécutez la commande et assurez-vous de spécifier le paramètre *ComputerName* à l’aide du nom du serveur qui est déjà configuré dans le cadre du déploiement multisite.  
  
## <a name="entry-point-and-domain-controller-not-provided-in-cmdlet"></a>Point d’entrée et contrôleur de domaine non fournis dans l’applet de commande  
L’applet de commande `Set-DaEntryPointDC` vous permet de modifier le contrôleur de domaine associé à des points entrées différents, par exemple, si un contrôleur de domaine particulier n’est plus disponible. Vous pouvez mettre à jour un point d’entrée spécifique afin d’utiliser un autre contrôleur de domaine, ou vous pouvez mettre à jour tous les points d’entrée qui utilisent un contrôleur de domaine spécifique afin d’utiliser un nouveau contrôleur de domaine. Dans le premier cas, vous devez utiliser le paramètre *EntryPointName* pour spécifier le point d’entrée à mettre à jour. Dans le second cas, vous devez utiliser le paramètre *ExistingDC* pour spécifier le contrôleur de domaine à remplacer. Vous ne pouvez spécifier qu’un seul de ces paramètres.  
  
**Erreur reçue**. Aucun paramètre requis n’a été spécifié. Fournissez le nom d’un point d’entrée ou d’un contrôleur de domaine existant.  
  
Ou  
  
Tous les paramètres requis sont absents de l’applet de commande `Set-DaEntryPointDC`.  
  
**Cause**  
  
Le paramètre *EntryPointName* ou *ExistingDC* n’a pas été spécifié ou ces deux paramètres l’ont été, pour l’applet de commande `Set-DaEntryPointDC`.  
  
**Solution**  
  
Exécutez la commande et assurez-vous de spécifier soit le paramètre *EntryPointName*, soit le paramètre *ExistingDC*.  
  
## <a name="could-not-locate-domain-controller"></a>Impossible de localiser le contrôleur de domaine  
**Erreur reçue**. Impossible de localiser automatiquement un nouveau contrôleur de domaine. Réessayez plus tard ou vérifiez les paramètres du contrôleur de domaine.  
  
**Cause**  
  
L’ordinateur spécifié à l’aide du paramètre *ComputerName* n’est pas accessible sur RPC ou le domaine ne contient pas de contrôleurs de domaine accessibles en écriture disponibles.  
  
**Solution**  
  
Assurez-vous que l’ordinateur distant est accessible sur RPC et qu’il existe un contrôleur de domaine accessible en écriture disponible pour le domaine. Si un contrôleur de domaine accessible en écriture est disponible pour le domaine, vous pouvez également spécifier son nom explicitement à l’aide du paramètre *NewDC*.  
  
## <a name="could-not-connect-to-domain-controller"></a>Impossible de se connecter au contrôleur de domaine  
  
-   **Problème 1**  
  
    **Erreur reçue**. Le contrôleur de domaine < domain_controller > est inaccessible. Vérifiez la connectivité réseau et la disponibilité du contrôleur de domaine.  
  
    **Cause**  
  
    Le contrôleur de domaine n’est pas accessible. Cela se produit uniquement lorsque l’administrateur spécifie un contrôleur de domaine dans les paramètres *NewDC* ou *ExistingDC*.  
  
    **Solution**  
  
    Vérifiez que le nom du contrôleur de domaine est correct. Si vous avez utilisé un nom court pour spécifier ce nom, réessayez en utilisant le nom de domaine complet correspondant.  
  
-   **Problème 2**  
  
    **Erreur reçue**. Impossible de contacter le contrôleur de domaine < domain_controller >.  
  
    **Cause**  
  
    Il peut exister un problème de réseau qui empêche l’accès au contrôleur de domaine spécifié dans le paramètre *NewDC* ou à tout autre contrôleur de domaine existant dans la configuration.  
  
    **Solution**  
  
    Vérifiez que le nom du contrôleur de domaine est correct, vérifiez qu’il existe, qu’il est en cours d’exécution, qu’il est accessible en écriture et qu’il existe une relation d’approbation entre le contrôleur de domaine et le domaine.  
  
-   **Problème 3**  
  
    **Erreur reçue**. Impossible d’accéder au contrôleur de domaine < domain_controller > pour %2 ! s !.  
  
    **Cause**  
  
    Pour assurer la cohérence de la configuration dans un déploiement multisite, il est important de veiller à ce que chaque objet de stratégie de groupe soit géré par un seul contrôleur de domaine. Lorsque le contrôleur de domaine qui gère l’objet de stratégie de groupe du serveur d’un point d’entrée n’est pas disponible, les paramètres de configuration de l’accès à distance ne peuvent pas être lus ou modifiés.  
  
    **Solution**  
  
    Suivez la procédure « pour modifier le contrôleur de domaine qui gère les objets de stratégie de groupe de serveur » décrit dans [2,4. Configurer des objets de stratégie de groupe](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
-   **Problème 4**  
  
    **Erreur reçue**. Impossible d’accéder au contrôleur de domaine principal dans le domaine < domain_name >.  
  
    **Cause**  
  
    Pour assurer la cohérence de la configuration dans un déploiement multisite, il est important de veiller à ce que chaque objet de stratégie de groupe soit géré par un seul contrôleur de domaine. Les objets de stratégie de groupes clients sont gérés sur le contrôleur de domaine principal. Si le contrôleur de domaine principal n’est pas disponible, il est impossible de lire ou de modifier les paramètres de configuration de l’accès à distance.  
  
    **Solution**  
  
    Suivez la procédure « pour transférer le rôle d’émulateur de contrôleur de domaine principal » décrite dans [2,4. Configurer des objets de stratégie de groupe](assetId:///b1960686-a81e-4f48-83f1-cc4ea484df43#ConfigGPOs).  
  
## <a name="read-only-domain-controller"></a>Contrôleur de domaine en lecture seule  
**Erreur reçue**. Le contrôleur de domaine < domain_controller > est en lecture seule. Spécifiez un contrôleur de domaine qui n’est pas en lecture seule.  
  
**Cause**  
  
Le contrôleur de domaine spécifié à l’aide du paramètre *NewDC* est en lecture seule.  
  
**Solution**  
  
Avec `Set-DAEntryPointDC`, le paramètre *NewDC* est utilisé pour mettre à jour le contrôleur de domaine associé à un point d’entrée particulier ou pour mettre à jour tous les points d’entrée associés à un contrôleur de domaine. Par conséquent, le nouveau contrôleur de domaine doit être accessible en lecture. Spécifiez un contrôleur de domaine accessible en lecture dans le paramètre *NewDC* et réessayez.  
  
## <a name="cannot-retrieve-gpo"></a>Impossible de récupérer un objet de stratégie de groupe  
  
-   **Problème 1**  
  
    **Erreur reçue**. Les objets de stratégie de groupe < GPO_name > sur le contrôleur de domaine < previous_domain_controller > ne peuvent pas être récupérés à partir du contrôleur de domaine < replacement_domain_controller >, car ils ne se trouvent pas dans le même domaine.  
  
    **Cause**  
  
    Le serveur d’accès à distance et le contrôleur de domaine ne sont pas dans le même domaine ; par conséquent, il est impossible de récupérer l’objet de stratégie de groupe.  
  
    **Solution**  
  
    Si vous avez essayé de mettre à jour un point d’entrée spécifique, assurez-vous que le nouveau contrôleur de domaine se trouve dans le même domaine que le serveur du point d’entrée. Si vous avez essayé de mettre à jour un contrôleur de domaine spécifique, assurez-vous que le nouveau contrôleur de domaine se trouve dans le même domaine que celui que vous essayez de remplacer.  
  
-   **Problème 2**  
  
    **Erreur reçue**. Les objets de stratégie de groupe < GPO_name > sur le contrôleur de domaine < previous_domain_controller > ne peuvent pas être récupérés à partir du contrôleur de domaine < replacement_domain_controller >. Attendez que la réplication de domaine se termine, puis réessayez.  
  
    **Cause**  
  
    Lorsque vous essayez de mettre à jour le contrôleur de domaine d’un point d’entrée, l’applet de commande essaie de lire l’objet de stratégie de groupe serveur à partir du nouveau contrôleur de domaine ; cependant, l’objet de stratégie de groupe est introuvable sur le nouveau contrôleur de domaine car il n’a pas encore été répliqué.  
  
    **Solution**  
  
    L’objet de stratégie de groupe serveur n’existe pas sur le nouveau contrôleur de domaine. Assurez-vous que les objets de stratégie de groupe se sont correctement répliqués sur le nouveau contrôleur de domaine, puis réessayez.  
  
-   **Problème 3**  
  
    **Erreur reçue**. Vous ne disposez pas des autorisations nécessaires pour accéder à l’objet de stratégie de groupe < GPO_name >.  
  
    **Cause**  
  
    Lorsque vous essayez de mettre à jour le contrôleur de domaine d’un point d’entrée, l’applet de commande essaie de lire l’objet de stratégie de groupe serveur à partir du nouveau contrôleur de domaine ; cependant, il n’est pas possible de lire l’objet de stratégie de groupe sur le nouveau contrôleur de domaine car vous ne possédez pas les autorisations adéquates.  
  
    **Solution**  
  
    L’objet de stratégie de groupe existe sur le contrôleur de domaine, mais il ne peut pas être lu. Vérifiez que vous possédez les autorisations requises et réessayez.  
  
## <a name="entry-point-not-part-of-multisite-deployment"></a>Point d’entrée non inclus dans le déploiement multisite  
**Erreur reçue**. Le point d’entrée < entry_point_name > ne fait pas partie du déploiement multisite. Donnez une autre valeur.  
  
**Cause**  
  
Le nom du point d’entrée que vous avez spécifié est introuvable.  
  
**Solution**  
  
Assurez-vous que le nom du point d’entrée est correct et que les objets de stratégie de groupe sont répliqués sur les contrôleurs de domaine requis, puis réessayez. Pour afficher le contrôleur de domaine attribué à chaque point d’entrée, utilisez `Get-DAEntryPointDC`.  
  
## <a name="remote-access-server-settings"></a>Paramètres du serveur d’accès à distance  
  
-   **Problème 1**  
  
    **Erreur reçue**. Impossible d’accéder au serveur < server_name > du point d’entrée < entry_point_name >.  
  
    **Cause**  
  
    Lorsque vous essayez de mettre à jour le contrôleur de domaine d’un point d’entrée, l’applet de commande essaie de le lire et de l’écrire depuis tous les serveurs d’accès à distance concernés. L’applet de commande n’a pas pu lire les données depuis un ou plusieurs serveurs d’accès à distance.  
  
    **Solution**  
  
    Assurez-vous que tous les serveurs d’accès à distance concernés sont en cours d’exécution et que vous possédez des autorisations d’administrateur local sur chacun d’eux, puis réessayez.  
  
-   **Problème 2**  
  
    **Erreur reçue**. Impossible d’enregistrer les paramètres dans le registre sur le serveur < server_name > dans le point d’entrée < entry_point_name >.  
  
    **Cause**  
  
    Lorsque vous essayez de mettre à jour le contrôleur de domaine d’un point d’entrée, l’applet de commande essaie de le lire et de l’écrire depuis tous les serveurs d’accès à distance concernés. L’applet de commande n’a pas pu écrire les données sur un ou plusieurs serveurs d’accès à distance.  
  
    **Solution**  
  
    Assurez-vous que tous les serveurs d’accès à distance concernés sont en cours d’exécution et que vous possédez des autorisations d’administrateur local sur chacun d’eux, puis réessayez.  
  
-   **Problème 3**  
  
    **Erreur reçue**. Les mises à jour d’objets de stratégie de groupe ne peuvent pas être appliquées sur < server_name >. Les modifications apportées ne prendront effet qu’après la prochaine actualisation de la stratégie.  
  
    **Cause**  
  
    Lorsque vous utilisez l’applet de commande `Set-DAEntryPointDC`, le paramètre *ComputerName* spécifié est un serveur d’accès à distance situé dans un point d’entrée autre que le dernier ajouté au déploiement multisite.  
  
    **Solution**  
  
    Vous pouvez voir tous les serveurs qui n’ont pas été mis à jour en vous référant à l’indication **État de configuration** dans **TABLEAU DE BORD** dans la console Gestion de l’accès à distance. Cette absence de mise à jour ne provoque pas de problèmes fonctionnels ; toutefois, vous pouvez exécuter `gpupdate /force` sur tous les serveurs qui n’ont pas été mis à jour afin que l’état de configuration soit immédiatement mis à jour.  
  
## <a name="problem-resolving-fqdn"></a>Problème de résolution de nom de domaine complet  
**Erreur reçue**. Impossible d’accéder au serveur < server_name > du point d’entrée < entry_point_name >.  
  
**Cause**  
  
Lors de l’obtention de la liste des serveurs DirectAccess à modifier, l’applet de commande n’a pas pu résoudre le nom de domaine complet de l’un des serveurs d’après son SID d’ordinateur.  
  
**Solution**  
  
Le point d’entrée spécifié dans le message d’erreur est associé à un contrôleur de domaine. Assurez-vous que le contrôleur de domaine est disponible pour le point d’entrée. Si l’ordinateur auquel le SID spécifié appartient a été supprimé du domaine, ignorez ce message, puis supprimez le serveur du déploiement multisite.  
  
## <a name="no-entry-points-to-update"></a>Aucun point d’entrée à mettre à jour  
**Avertissement reçu**. Les paramètres du contrôleur de domaine n’ont pas été modifiés. Si des modifications sont nécessaires, assurez-vous que les paramètres de l’applet de commande sont configurés correctement et que les objets de stratégie de groupe sont répliqués sur les contrôleurs de domaine nécessaires.  
  
**Cause**  
  
Lors de l’appel de l’applet de commande `Set-DaEntryPointDC` avec le paramètre *ExistingDC*, DirectAccess vérifie tous les points d’entrée et met à jour ceux qui sont associés au contrôleur de domaine spécifié. Toutefois, aucun point d’entrée n’utilise le paramètre *ExistingDC* spécifié.  
  
**Solution**  
  
Pour afficher la liste des points d’entrée et leurs contrôleurs de domaine associés, utilisez l’applet de commande `Get-DAEntryPointDC`. Si des modifications auraient dû être apportées, assurez-vous que les paramètres de l’applet de commande sont corrects et que les objets de stratégie de groupe sont répliqués sur les contrôleurs de domaine requis, puis réessayez.  
  


