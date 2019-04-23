---
title: Étape 2 Plan l’Infrastructure Multisite
description: Cette rubrique fait partie du guide de déploiement de plusieurs serveurs d’accès distant dans un déploiement Multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 771df80fc3130b5c4c03bf628a95d67b7df04b36
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888200"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>Étape 2 Plan l’Infrastructure Multisite

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

L’étape suivante dans le déploiement de l’accès à distance dans une topologie multisite consiste à effectuer la planification de l’infrastructure multisite ; notamment, Active Directory, les groupes de sécurité et les objets de stratégie de groupe.  
## <a name="bkmk_2_1_AD"></a>2.1 planifier Active Directory  
Un déploiement multisite de l’accès à distance peut être configuré dans un nombre de topologies :  
  
-   **Site Active Directory unique, plusieurs points d’entrée**-dans cette topologie, vous avez un seul site Active Directory pour toute votre organisation avec des liens rapides intranet tout au long du site, mais vous avez plusieurs serveurs d’accès à distance déployés au sein de votre organisation, chaque agissant comme un point d’entrée. Un exemple géographique de cette topologie consiste à avoir un seul site Active Directory pour les États-Unis avec des points d’entrée sur la côte est des États-Unis et de la côte ouest.  
  
    ![Infrastructure multisite](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)  
  
-   **Plusieurs sites Active Directory, plusieurs points d’entrée**-dans cette topologie, vous avez deux ou plusieurs sites Active Directory avec un serveur d’accès à distance déployé en tant que point d’entrée pour chaque site. Chaque serveur d’accès à distance est associé avec le contrôleur de domaine Active Directory pour le site. Un exemple géographique de cette topologie est d’avoir un site Active Directory pour les États-Unis et l’autre pour l’Europe avec un point d’entrée unique pour chaque site. Notez que si vous avez plusieurs sites Active Directory vous n’avez pas besoin d’avoir un point d’entrée associé à chaque site. En outre, certains sites Active Directory peuvent avoir plusieurs points d’entrée associé.  
  
    ![Topologie multisite](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)  
  
Dans un point d’entrée multisite, vous pouvez configurer un serveur d’accès à distance unique, plusieurs serveurs d’accès à distance, ou un serveur d’accès à distance des clusters.   
  
### <a name="active-directory-best-practices-and-recommendations"></a>Recommandations et meilleures pratiques de active Directory  
Notez les recommandations et les contraintes pour le déploiement d’Active Directory dans un scénario multisite suivantes :  
  
1.  Chaque site Active Directory peut contenir un ou plusieurs serveurs d’accès à distance, ou un cluster de serveurs fonctionnent en tant que points d’entrée multisite pour les ordinateurs clients. Toutefois un site Active Directory n’est pas requis pour avoir un point d’entrée.  
  
2.  Un point d’entrée multisite peut uniquement être associé à un seul site Active Directory. Lorsque les ordinateurs clients exécutant Windows 8 se connectent à un point d’entrée spécifique, elles sont considérées comme appartenant au site Active Directory associé à ce point d’entrée.  
  
3.  Il est recommandé que chaque site Active Directory a un contrôleur de domaine. Le contrôleur de domaine peut être en lecture seule.  
  
4.  Si chaque site Active Directory contient un contrôleur de domaine, l’objet de stratégie de groupe pour un serveur dans le point d’entrée est géré par un des contrôleurs de domaine dans le site Active Directory associé au point de terminaison. S’il n’y a aucun contrôleur de domaine accessible en écriture dans ce site, l’objet de stratégie de groupe pour un serveur est géré sur un contrôleur de domaine accessible en écriture qui se trouve le plus proche pour le premier serveur d’accès à distance qui est configuré dans le point d’entrée. Le plus proche est déterminé par un lien de calcul du coût. Notez que dans ce scénario, après avoir apporté les modifications de configuration il peut y avoir un délai lors de la réplication entre le contrôleur de domaine que la gestion de l’objet de stratégie de groupe et le contrôleur de domaine en lecture seule dans le site du serveur Active Directory.  
  
5.  Stratégie de groupe de client et le serveur d’applications facultatif que GPO sont gérés sur le contrôleur de domaine en cours d’exécution en tant que l’émulateur de contrôleur de domaine principal (PDC). Cela signifie que le client stratégie de groupe n’est pas nécessairement gérés dans le site Active Directory contenant le point d’entrée pour les clients qui se connecter.  
  
6.  Si le contrôleur de domaine pour un site Active Directory est inaccessible, le serveur d’accès à distance se connecte à un contrôleur de domaine de remplacement dans le site (si disponible). Si ce n’est pas le cas, il se connecte au contrôleur de domaine pour un autre site pour récupérer un objet de stratégie de groupe mis à jour et pour authentifier les clients. Dans les deux cas, la console de gestion de l’accès à distance et les applets de commande PowerShell ne peut pas être utilisé pour récupérer ou modifier les paramètres de configuration jusqu'à ce que le contrôleur de domaine est disponible. Notez les points suivants :  
  
    1.  Si le serveur en cours d’exécution en tant que l’émulateur de contrôleur de domaine principal n’est pas disponible, vous devez désigner un contrôleur de domaine disponible qui a mis à jour des objets stratégie de groupe en tant que l’émulateur PDC.  
  
    2.  Si le contrôleur de domaine qui gère un serveur de stratégie de groupe n’est pas disponible, utilisez l’applet de commande Set-DAEntryPointDC PowerShell pour associer un nouveau contrôleur de domaine avec le point d’entrée. Le nouveau contrôleur de domaine doit avoir la stratégie de groupe à jour avant d’exécuter l’applet de commande.  
  
## <a name="bkmk_2_2_SG"></a>2.2 planifier des groupes de sécurité  
Au cours du déploiement d’un seul serveur avec des paramètres avancés, tous les ordinateurs clients l’accès au réseau interne via DirectAccess ont été collectées dans un groupe de sécurité. Dans un déploiement multisite, ce groupe de sécurité est utilisé pour les ordinateurs clients Windows 8 uniquement. Pour un déploiement multisite, les ordinateurs clients Windows 7 seront collectées dans des groupes de sécurité distincts pour chaque point d’entrée dans le déploiement multisite. Par exemple, si vous avez regroupé précédemment tous les ordinateurs clients dans le groupe DA_Clients, vous devez supprimer tous les ordinateurs Windows 7 à partir de ce groupe et les placer dans un groupe de sécurité différents. Par exemple, dans plusieurs sites d’Active Directory, entrée de plusieurs points de topologie, vous créez un groupe de sécurité pour le point d’entrée United States (DA_Clients_US) et l’autre pour le point d’entrée en Europe (DA_Clients_Europe). Placez tous les ordinateurs clients Windows 7 situés dans les adresses suivantes : http://www.Dell.com/service_contracts dans le groupe DA_Clients_US et n’importe quel Europe situé dans le groupe DA_Clients_Europe. Si vous n’avez pas de tous les ordinateurs clients Windows 7, il est inutile de planifier des groupes de sécurité pour les ordinateurs Windows 7.  
  
Groupes de sécurité requis sont les suivantes :  
  
-   Un groupe de sécurité pour tous les ordinateurs clients de Windows 8. Il est recommandé de créer un groupe de sécurité unique pour ces clients pour chaque domaine.  
  
-   Un groupe de sécurité unique contenant des ordinateurs clients de Windows 7 pour chaque point d’entrée. Il est recommandé de créer un groupe unique pour chaque domaine. Exemple : Domain1\DA_Clients_Europe ; Domain2\DA_Clients_Europe ; Domain1\DA_Clients_US ; Domain2\DA_Clients_US.  
  
## <a name="bkmk_2_3_GPO"></a>2.3 les objets de stratégie de groupe de plan de  
Les paramètres DirectAccess configurés au cours du déploiement de l’accès à distance sont rassemblés dans des objets stratégie de groupe. Votre déploiement mono-serveur déjà utilise la stratégie de groupe pour les clients DirectAccess, le serveur d’accès à distance et éventuellement des serveurs d’applications. Un déploiement multisite requiert une stratégie de groupe suivant :  
  
-   Un serveur de stratégie de groupe pour chaque point d’entrée.  
  
-   Un objet stratégie de groupe pour chaque domaine contenant des ordinateurs clients exécutant Windows 8.  
  
-   Un objet stratégie de groupe pour chaque point d’entrée et chaque domaine contenant des ordinateurs clients Windows 7.  
  
Stratégie de groupe peut être configurés comme suit :  
  
-   **Automatiquement**-vous pouvez spécifier que les GPO sont créés automatiquement par accès à distance. Un nom par défaut est spécifié pour chaque objet de stratégie de groupe et peut être modifié.  
  
-   **Manuellement**-vous pouvez utiliser des objets de stratégie de groupe qui ont été créés manuellement par l’administrateur Active Directory.  
  
> [!NOTE]  
> Une fois que DirectAccess est configuré pour utiliser des objets stratégie de groupe spécifique, il ne peut pas être configuré pour utiliser les différents objets de stratégie de groupe.  
  
### <a name="231-automatically-created-gpos"></a>2.3.1 stratégie de groupe créés automatiquement  
Notez les points suivants quand vous utilisez des objets de stratégie de groupe créés automatiquement :  
  
-   Les objets de stratégie de groupe créés automatiquement sont appliqués en fonction du paramètre d'emplacement et du paramètre de cible du lien, de la façon suivante :  
  
    -   Pour le serveur de stratégie de groupe, les paramètres de l’emplacement et le lien pointent vers le domaine contenant le serveur d’accès à distance.  
  
    -   Pour le client de stratégie de groupe, la cible du lien est définie à la racine du domaine dans lequel l’objet de stratégie de groupe a été créé. Un objet de stratégie de groupe est créé pour chaque domaine qui contient les ordinateurs clients, et l’objet de stratégie de groupe sera lié à la racine de chaque domaine. .  
  
-   Pour les GPO créés automatiquement, administrateur du serveur pour appliquer les paramètres DirectAccess l’accès à distance requiert les autorisations suivantes :  
  
    -   Autorisations pour les domaines requis de création de GPO.  
  
    -   Autorisations de lien pour toutes les racines de domaine client sélectionnées.  
  
    -   Autorisation de créer le filtre WMI pour les GPO est requise si DirectAccess a été configuré pour fonctionner sur les ordinateurs portables uniquement.  
  
    -   Autorisations de liaison pour les racines des domaines associés avec les points d’entrée (les domaines de la stratégie de groupe serveur)  
  
    -   Nous recommandons à l'administrateur de l'accès à distance de posséder des autorisations de lecture des objets de stratégie de groupe pour chaque domaine requis. Cela permet un accès à distance vérifier que les objets stratégie de groupe avec des noms dupliqués n’existent pas lors de la création de stratégie de groupe pour le déploiement multisite.  
  
    -   Autorisations de réplication Active Directory sur des domaines associés à des points d’entrée. Il s’agit, car lorsque vous ajoutez initialement des points d’entrée, le serveur de stratégie de groupe pour le point d’entrée est créé sur le contrôleur de domaine le plus proche de ce point d’entrée. Toutefois, étant donné que la création de liens est pris en charge sur l’émulateur de contrôleur de domaine principal uniquement, l’objet de stratégie de groupe doit être répliquée à partir du contrôleur de domaine sur lequel il a été créé pour le contrôleur de domaine en cours d’exécution en tant que l’émulateur de contrôleur de domaine principal avant de créer le lien.  
  
Notez que si les autorisations appropriées pour la réplication et liaison d’objets n’existent pas, un avertissement est émis. L’accès à distance continue de fonctionner mais la réplication et la liaison ne sont pas lieu. Si un avertissement est émis de lien liens ne sont pas créés automatiquement, même une fois que les autorisations sont en place. L'administrateur doit alors créer les liens manuellement.  
  
### <a name="232-manually-created-gpos"></a>2.3.2 créé manuellement la stratégie de groupe  
Notez les points suivants quand vous utilisez des objets de stratégie de groupe créés manuellement :  
  
-   Stratégie de groupe suivants doit être créées manuellement pour le déploiement multisite :  
  
    -   **GPO de serveur**- un serveur de l’objet stratégie de groupe pour chaque point d’entrée (dans le domaine dans lequel se trouve le point d’entrée). Cet objet de stratégie de groupe est appliquée sur chaque serveur d’accès à distance dans le point d’entrée.  
  
    -   **Client de l’objet de stratégie de groupe (Windows 7)**- un objet stratégie de groupe pour chaque point d’entrée et chaque domaine contenant des ordinateurs clients Windows 7 qui seront connecte à des points d’entrée dans le déploiement multisite. Par exemple Domain1\DA_W7_Clients_GPO_Europe ; Domain2\DA_W7_Clients_GPO_Europe ; Domain1\DA_W7_Clients_GPO_US ; Domain2\DA_W7_Clients_GPO_US. Si aucun ordinateur client de Windows 7 ne se connectent à des points d’entrée, les GPO ne sont pas nécessaires.  
  
-   Il n’est pas nécessaire de créer des ordinateurs client de stratégie de groupe pour Windows 8 supplémentaires. Un objet de stratégie de groupe pour chaque domaine contenant des ordinateurs clients a déjà été créé lorsque le serveur d’accès à distance unique a été déployé. Dans un déploiement multisite, ces objets stratégie de groupe client fonctionnera comme les clients de stratégie de groupe pour Windows 8.  
  
-   Stratégie de groupe doit exister avant de cliquer sur le bouton de validation dans les Assistants de déploiement multisite.  
  
-   De plus, une recherche de lien vers l'objet de stratégie de groupe est effectuée dans tout le domaine. Si l'objet de stratégie de groupe n'est pas lié dans le domaine, alors un lien est automatiquement créé à la racine du domaine. Si les autorisations requises pour créer le lien ne sont pas disponibles, un avertissement est émis.  
  
-   Quand vous utilisez manuellement créés stratégie de groupe pour appliquer les paramètres DirectAccess l’administrateur de serveur d’accès à distance requiert des autorisations complètes (modification, suppression, modification de la sécurité) sur la stratégie de groupe créés manuellement.  
  
Notez que si les autorisations appropriées pour la réplication et liaison d’objets n’existent pas, un avertissement est émis. L’accès à distance continue de fonctionner mais la réplication et la liaison ne sont pas lieu. Si un avertissement est émis de lien liens ne sont pas créés automatiquement, même une fois que les autorisations sont en place. L'administrateur doit alors créer les liens manuellement.  
  
### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 la gestion des GPO dans un environnement de plusieurs contrôleurs de domaine  
Chaque objet de stratégie de groupe est géré par un contrôleur de domaine spécifique, comme suit :  
  
-   Le serveur de stratégie de groupe est géré par un des contrôleurs de domaine dans le site Active Directory associés au serveur ou si les contrôleurs de domaine dans ce site sont en lecture seule, par un contrôleur de domaine accessible en écriture le plus proche pour le premier serveur dans l’entrée de point.  
  
-   Client de stratégie de groupe est gérés par le contrôleur de domaine en cours d’exécution en tant que l’émulateur PDC.  
  
Si vous souhaitez manuellement modifier note des paramètres de stratégie de groupe les éléments suivants :  
  
-   Pour le serveur de stratégie de groupe, afin d’identifier le contrôleur de domaine qui est associé à un point d’entrée spécifique, exécutez l’applet de commande PowerShell `Get-DAEntryPointDC -EntryPointName <name of entry point>`.  
  
-   Par défaut, lorsque vous apportez des modifications avec des applets de commande PowerShell mise en réseau ou de la console de gestion de stratégie de groupe, le contrôleur de domaine agissant en tant que l’émulateur de contrôleur de domaine principal est utilisé.  
  
-   Si vous modifiez les paramètres sur un contrôleur de domaine qui n’est pas le contrôleur de domaine associé avec le point d’entrée (pour le serveur de stratégie de groupe) ou l’émulateur de contrôleur de domaine principal (pour le client de stratégie de groupe), notez les points suivants :  
  
    1.  Avant de modifier les paramètres, assurez-vous que le contrôleur de domaine est répliqué avec une stratégie de groupe à jour, et [les paramètres de stratégie de groupe de sauvegarde](https://go.microsoft.com/fwlink/?LinkID=257928), avant d’apporter des modifications. Si l’objet de stratégie de groupe n’est pas mis à jour, des conflits de fusion lors de la réplication peuvent se produire, se traduisant par une configuration incorrecte de l’accès à distance.  
  
    2.  Après avoir modifié les paramètres, vous devez attendre la réplication vers le contrôleur de domaine qui est associé à la stratégie de groupe des modifications. N’apportez pas de modifications supplémentaires à l’aide de la console de gestion de l’accès à distance ou les applets de commande PowerShell pour l’accès à distance jusqu'à ce que la réplication est terminée. Si un objet de stratégie de groupe est modifié sur deux différents contrôleurs de domaine avant que la réplication soit terminée, les conflits de fusion peuvent se produire, se traduisant par une configuration incorrecte  
  
-   Vous pouvez également modifier le paramètre par défaut à l’aide du **modifier un contrôleur de domaine** boîte de dialogue zone dans la console Gestion de stratégie de groupe ou à l’aide de la **Open-NetGPO** applet de commande PowerShell, afin que les modifications effectuées à l’aide de la console ou les applets de commande de mise en réseau d’utiliser le contrôleur de domaine que vous spécifiez.  
  
    1.  Pour ce faire, dans la console Gestion de stratégie de groupe, cliquez sur le domaine ou le conteneur sites et sur **modifier le contrôleur de domaine**.  
  
    2.  Pour ce faire, dans PowerShell, spécifiez le paramètre DomainController pour l’applet de commande Open-NetGPO. Par exemple, pour activer les profils privés et publics dans le pare-feu Windows sur un objet de stratégie de groupe nommé _Europe domain1\DA_Server_GPO à l’aide d’un contrôleur de domaine nommé europe-DC.corp.contoso.com, procédez comme suit :  
  
        ```  
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
        Save-NetGPO -GpoSession $gpoSession  
        ```  
  
#### <a name="modifying-domain-controller-association"></a>Modifier l’association de contrôleur de domaine  
Pour assurer la cohérence de la configuration dans un déploiement multisite, il est important de veiller à ce que chaque objet de stratégie de groupe soit géré par un seul contrôleur de domaine. Dans certains scénarios, il peut être nécessaire pour attribuer un autre contrôleur de domaine pour un objet de stratégie de groupe :  
  
-   **Maintenance du contrôleur de domaine et les temps d’arrêt**-lorsque le contrôleur de domaine qui gère un objet de stratégie de groupe n’est pas disponible, il peut être nécessaire pour gérer l’objet de stratégie de groupe sur un contrôleur de domaine différent.  
  
-   **Optimisation de la distribution de configuration**-une fois les modifications d’infrastructure de réseau, il peut être nécessaire pour gérer le GPO de serveur de point d’entrée sur un contrôleur de domaine dans le même site Active Directory comme point d’entrée.   
  
## <a name="bkmk_2_4_DNS"></a>2.4 planifier DNS  
Notez les points suivants lorsque vous planifiez un déploiement multisite DNS :  
  
1.  Les ordinateurs clients utilisent l’adresse ConnectTo pour se connecter au serveur d’accès à distance. Chaque point d’entrée dans votre déploiement nécessite une autre adresse ConnectTo. Chaque adresse ConnectTo du point d’entrée doit être disponible dans le DNS public et l’adresse que vous choisissez doit correspondre au nom de sujet du certificat IP-HTTPS que vous déployez pour la connexion IP-HTTPS.   
  
2.  En outre, l’accès à distance applique le routage symétrique ; Par conséquent, seuls les clients Teredo et IP-HTTPS peuvent se connecter quand un déploiement multisite est activé. Pour autoriser les clients IPv6 natifs pour se connecter, l’adresse ConnectTo (l’URL IP-HTTPS) doit être résolue en les deux les externe (accessible sur Internet) adresses IPv6 et IPv4 sur le serveur d’accès à distance.  
  
3.  L'accès à distance crée une sonde web par défaut utilisée par les ordinateurs clients DirectAccess pour vérifier la connectivité au réseau interne. Lors de la configuration du serveur les noms suivants doivent avoir été enregistrés dans DNS :  
  
    1.  DirectAccess-WebProbeHost doit résoudre à l’adresse IPv4 interne du serveur d’accès à distance, ou à l’adresse IPv6 dans un environnement IPv6 uniquement.  
  
    2.  DirectAccess-CorpConnectivityHost doit résoudre à une adresse localhost (bouclage) (soit :: 1 ou 127.0.0.1, selon que IPv6 est déployé dans le réseau d’entreprise).  
  
    Dans un déploiement multisite, une entrée DNS supplémentaire pour directaccess-WebProbeHost doit être créée pour chaque point d’entrée. Lorsque vous ajoutez un point d’entrée, l’accès à distance tente de créer automatiquement cette entrée directaccess-WebProbeHost supplémentaires. Toutefois, en cas d’échec un avertissement s’affichera et vous devez créer manuellement l’entrée.  
  
    > [!NOTE]  
    > Lorsque le nettoyage DNS est activé dans votre infrastructure DNS, il est recommandé de désactiver le nettoyage sur les entrées DNS créées automatiquement par l’accès à distance.  
  

  



