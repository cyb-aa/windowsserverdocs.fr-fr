---
title: Étape 2 planifier l’infrastructure multisite
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64c10107-cb03-41f3-92c6-ac249966f574
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ff8a58aa679691132d074ef52b876cea05366ab5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367104"
---
# <a name="step-2-plan-the-multisite-infrastructure"></a>Étape 2 planifier l’infrastructure multisite

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

L’étape suivante du déploiement de l’accès à distance dans une topologie multisite consiste à effectuer la planification de l’infrastructure multisite. notamment, Active Directory, les groupes de sécurité et les objets stratégie de groupe.  
## <a name="bkmk_2_1_AD"></a>2,1 Active Directory de plan  
Un déploiement multisite d’accès à distance peut être configuré dans plusieurs topologies :  
  
-   **Site à Active Directory unique, plusieurs points d’entrée**: dans cette topologie, vous disposez d’un seul site Active Directory pour l’ensemble de votre organisation avec des liaisons intranet rapides sur le site, mais vous avez plusieurs serveurs d’accès à distance déployés tout au long de votre organisation, chacune faisant office de point d’entrée. Un exemple géographique de cette topologie consiste à avoir un seul site de Active Directory pour la États-Unis avec des points d’entrée sur la côte est et la côte ouest.  
  
    ![Infrastructure multisite](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo1.png)  
  
-   **Plusieurs sites de Active Directory, plusieurs points d’entrée**: dans cette topologie, vous avez au moins deux sites Active Directory avec un serveur d’accès à distance déployé en tant que point d’entrée pour chaque site. Chaque serveur d’accès à distance est associé au contrôleur de domaine Active Directory pour le site. Un exemple géographique de cette topologie consiste à avoir un site Active Directory pour les États-Unis et un pour l’Europe avec un seul point d’entrée pour chaque site. Notez que, si vous avez plusieurs sites Active Directory, vous n’avez pas besoin d’un point d’entrée associé à chaque site. En outre, certains sites Active Directory peuvent avoir plusieurs points d’entrée associés.  
  
    ![Topologie multisite](../../../../media/Step-2-Plan-the-Multisite-Infrastructure/RAMultisiteTopo2.png)  
  
Dans un point d’entrée multisite, vous pouvez configurer un serveur d’accès à distance unique, plusieurs serveurs d’accès à distance ou des clusters de serveurs d’accès à distance.   
  
### <a name="active-directory-best-practices-and-recommendations"></a>Meilleures pratiques et recommandations relatives à Active Directory  
Notez les recommandations et contraintes suivantes pour le déploiement de Active Directory dans un scénario multisite :  
  
1.  Chaque site Active Directory peut contenir un ou plusieurs serveurs d’accès à distance, ou un cluster de serveurs, fonctionnant comme des points d’entrée multisites pour les ordinateurs clients. Toutefois, un site Active Directory n’a pas besoin d’un point d’entrée.  
  
2.  Un point d’entrée multisite ne peut être associé qu’à un seul site Active Directory. Lorsque des ordinateurs clients exécutant Windows 8 se connectent à un point d’entrée spécifique, ils sont considérés comme appartenant au site Active Directory associé à ce point d’entrée.  
  
3.  Il est recommandé que chaque site Active Directory dispose d’un contrôleur de domaine. Le contrôleur de domaine peut être en lecture seule.  
  
4.  Si chaque site Active Directory contient un contrôleur de domaine, l’objet de stratégie de groupe d’un serveur dans le point d’entrée est géré par l’un des contrôleurs de domaine du site Active Directory associé au point de terminaison. S’il n’existe aucun contrôleur de domaine accessible en écriture dans ce site, l’objet de stratégie de groupe d’un serveur est géré sur un contrôleur de domaine accessible en écriture, le plus proche du premier serveur d’accès à distance configuré dans le point d’entrée. Le plus proche est déterminé par un calcul du coût des liens. Notez que dans ce scénario, après avoir apporté des modifications à la configuration, il peut y avoir un délai lors de la réplication entre le contrôleur de domaine qui gère l’objet de stratégie de groupe et le contrôleur de domaine en lecture seule dans le site de Active Directory du serveur.  
  
5.  Les objets de stratégie de groupe de client et les objets de stratégie de groupe de serveur d’application facultatifs sont gérés sur le contrôleur de domaine exécutant comme émulateur de contrôleur de domaine principal (PDC). Cela signifie que les objets de stratégie de groupe client ne sont pas nécessairement gérés dans le site Active Directory contenant le point d’entrée auquel les clients se connectent.  
  
6.  Si le contrôleur de domaine d’un site Active Directory est inaccessible, le serveur d’accès à distance se connectera à un autre contrôleur de domaine dans le site (s’il est disponible). Si ce n’est pas le cas, il se connecte au contrôleur de domaine d’un autre site pour récupérer un objet de stratégie de groupe mis à jour et authentifier les clients. Dans les deux cas, la console de gestion de l’accès à distance et les applets de commande PowerShell ne peuvent pas être utilisées pour récupérer ou modifier des paramètres de configuration tant que le contrôleur de domaine n’est pas disponible. Notez les points suivants :  
  
    1.  Si le serveur qui exécute en tant qu’émulateur de contrôleur de domaine principal n’est pas disponible, vous devez désigner un contrôleur de domaine disponible qui a mis à jour les objets de stratégie de groupe comme émulateur PDC.  
  
    2.  Si le contrôleur de domaine qui gère un objet de stratégie de groupe de serveur n’est pas disponible, utilisez l’applet de commande PowerShell Set-DAEntryPointDC pour associer un nouveau contrôleur de domaine au point d’entrée. Le nouveau contrôleur de domaine doit avoir des objets de stratégie de groupe à jour avant d’exécuter l’applet de commande.  
  
## <a name="bkmk_2_2_SG"></a>2,2 planifier des groupes de sécurité  
Lors du déploiement d’un serveur unique avec des paramètres avancés, tous les ordinateurs clients qui accèdent au réseau interne via DirectAccess ont été rassemblés dans un groupe de sécurité. Dans un déploiement multisite, ce groupe de sécurité est utilisé uniquement pour les ordinateurs clients Windows 8. Dans le cas d’un déploiement multisite, les ordinateurs clients Windows 7 seront rassemblés dans des groupes de sécurité distincts pour chaque point d’entrée dans le déploiement multisite. Par exemple, si vous avez regroupé tous les ordinateurs clients dans le groupe DA_Clients, vous devez maintenant supprimer tous les ordinateurs Windows 7 de ce groupe et les placer dans un autre groupe de sécurité. Par exemple, dans la topologie plusieurs points d’entrée Active Directory sites, vous créez un groupe de sécurité pour le point d’entrée États-Unis (DA_Clients_US) et un pour le point d’entrée Europe (DA_Clients_Europe). Placez tous les ordinateurs clients Windows 7 situés dans les adresses suivantes DA_Clients_US et n’importe quel Europe situé dans le groupe DA_Clients_Europe. Si vous n’avez pas d’ordinateurs clients Windows 7, vous n’avez pas besoin de planifier des groupes de sécurité pour les ordinateurs Windows 7.  
  
Les groupes de sécurité requis sont les suivants :  
  
-   Un groupe de sécurité pour tous les ordinateurs clients Windows 8. Il est recommandé de créer un groupe de sécurité unique pour ces clients pour chaque domaine.  
  
-   Groupe de sécurité unique contenant les ordinateurs clients Windows 7 pour chaque point d’entrée. Il est recommandé de créer un groupe unique pour chaque domaine. Exemple : Domain1\DA_Clients_Europe; Domain2\DA_Clients_Europe; Domain1\DA_Clients_US; Domain2\DA_Clients_US.  
  
## <a name="bkmk_2_3_GPO"></a>2,3 stratégie de groupe les objets de plan  
Les paramètres DirectAccess configurés au cours du déploiement de l’accès à distance sont collectés dans les GPO. Votre déploiement sur un seul serveur utilise déjà des objets de stratégie de groupe pour les clients DirectAccess, le serveur d’accès à distance et éventuellement pour les serveurs d’applications. Un déploiement multisite requiert les objets de stratégie de groupe suivants :  
  
-   Un objet de stratégie de groupe de serveur pour chaque point d’entrée.  
  
-   Un objet de stratégie de groupe pour chaque domaine contenant des ordinateurs clients exécutant Windows 8.  
  
-   Un objet de stratégie de groupe pour chaque point d’entrée et chaque domaine contenant des ordinateurs clients Windows 7.  
  
Les objets de stratégie de groupe peuvent être configurés comme suit :  
  
-   **Automatiquement**: vous pouvez spécifier que les objets de stratégie de groupe sont créés automatiquement par l’accès à distance. Un nom par défaut est spécifié pour chaque objet de stratégie de groupe et peut être modifié.  
  
-   **Manuellement**: vous pouvez utiliser les objets de stratégie de groupe créés manuellement par l’administrateur Active Directory.  
  
> [!NOTE]  
> Une fois que DirectAccess est configuré pour utiliser des objets de stratégie de groupe spécifiques, il ne peut pas être configuré pour utiliser différents objets de stratégie de groupe.  
  
### <a name="231-automatically-created-gpos"></a>2.3.1 objets de stratégie de groupe créés automatiquement  
Notez les points suivants quand vous utilisez des objets de stratégie de groupe créés automatiquement :  
  
-   Les objets de stratégie de groupe créés automatiquement sont appliqués en fonction du paramètre d'emplacement et du paramètre de cible du lien, de la façon suivante :  
  
    -   Pour l’objet de stratégie de groupe de serveur, les paramètres location et Link pointent vers le domaine qui contient le serveur d’accès à distance.  
  
    -   Pour les objets de stratégie de groupe client, la cible du lien est définie à la racine du domaine dans lequel l’objet de stratégie de groupe a été créé. Un objet de stratégie de groupe est créé pour chaque domaine contenant des ordinateurs clients, et l’objet de stratégie de groupe est lié à la racine de chaque domaine. .  
  
-   Pour les objets de stratégie de groupe créés automatiquement, pour appliquer les paramètres DirectAccess, l’administrateur du serveur d’accès à distance requiert les autorisations suivantes :  
  
    -   Autorisations de création d’objet de stratégie de groupe pour les domaines requis.  
  
    -   Autorisations de lien pour toutes les racines de domaine client sélectionnées.  
  
    -   L’autorisation de créer le filtre WMI pour les objets de stratégie de groupe est requise si DirectAccess a été configuré pour fonctionner sur des ordinateurs portables uniquement.  
  
    -   Autorisations de liaison pour les racines des domaines associés aux points d’entrée (les domaines de l’objet de stratégie de groupe du serveur)  
  
    -   Nous recommandons à l'administrateur de l'accès à distance de posséder des autorisations de lecture des objets de stratégie de groupe pour chaque domaine requis. Cela permet l’accès à distance pour vérifier que les objets de stratégie de groupe avec des noms dupliqués n’existent pas lors de la création de GPO pour le déploiement multisite.  
  
    -   Active Directory des autorisations de réplication sur des domaines associés à des points d’entrée. En effet, lors de l’ajout initial de points d’entrée, l’objet de stratégie de groupe du serveur pour le point d’entrée est créé sur le contrôleur de domaine le plus proche de ce point d’entrée. Toutefois, étant donné que la création de liens est prise en charge uniquement sur l’émulateur PDC, l’objet de stratégie de groupe doit être répliqué à partir du contrôleur de domaine sur lequel il a été créé vers le contrôleur de domaine qui s’exécute en tant qu’émulateur de contrôleur de domaine principal avant de créer le lien.  
  
Notez que si les autorisations appropriées pour la réplication et la liaison des objets de stratégie de groupe n’existent pas, un avertissement est émis. L’opération d’accès à distance se poursuit, mais la réplication et la liaison n’ont pas lieu. Si un avertissement de lien est émis, les liens ne sont pas créés automatiquement, même si les autorisations sont en place. L'administrateur doit alors créer les liens manuellement.  
  
### <a name="232-manually-created-gpos"></a>2.3.2 objets de stratégie de groupe créés manuellement  
Notez les points suivants quand vous utilisez des objets de stratégie de groupe créés manuellement :  
  
-   Les objets de stratégie de groupe suivants doivent être créés manuellement pour le déploiement multisite :  
  
    -   **Objet de stratégie de groupe**de serveur : objet de stratégie de groupe de serveur pour chaque point d’entrée (dans le domaine dans lequel se trouve le point d’entrée). Cet objet de stratégie de groupe sera appliqué à chaque serveur d’accès à distance dans le point d’entrée.  
  
    -   **Objet de stratégie de groupe client (Windows 7)** : un objet de stratégie de groupe pour chaque point d’entrée et chaque domaine contenant des ordinateurs clients Windows 7 qui se connecteront aux points d’entrée dans le déploiement multisite. Par exemple, Domain1\DA_W7_Clients_GPO_Europe ; Domain2\DA_W7_Clients_GPO_Europe; Domain1\DA_W7_Clients_GPO_US; Domain2\DA_W7_Clients_GPO_US. Si aucun ordinateur client Windows 7 ne se connecte aux points d’entrée, les objets de stratégie de groupe ne sont pas requis.  
  
-   Il n’est pas nécessaire de créer des objets de stratégie de groupe supplémentaires pour les ordinateurs clients Windows 8. Un objet de stratégie de groupe pour chaque domaine contenant des ordinateurs clients a déjà été créé lors du déploiement du serveur d’accès à distance unique. Dans un déploiement multisite, ces objets de stratégie de groupe de client fonctionneront comme les objets de stratégie de groupe pour les clients Windows 8.  
  
-   Les objets de stratégie de groupe doivent exister avant de cliquer sur le bouton valider dans les assistants de déploiement multisite.  
  
-   De plus, une recherche de lien vers l'objet de stratégie de groupe est effectuée dans tout le domaine. Si l'objet de stratégie de groupe n'est pas lié dans le domaine, alors un lien est automatiquement créé à la racine du domaine. Si les autorisations requises pour créer le lien ne sont pas disponibles, un avertissement est émis.  
  
-   Lorsque vous utilisez des objets de stratégie de groupe créés manuellement, pour appliquer des paramètres DirectAccess, l’administrateur du serveur d’accès à distance requiert des autorisations complètes de GPO (modification, suppression, modification de la sécurité) sur les objets de stratégie de groupe créés manuellement.  
  
Notez que si les autorisations appropriées pour la réplication et la liaison des objets de stratégie de groupe n’existent pas, un avertissement est émis. L’opération d’accès à distance se poursuit, mais la réplication et la liaison n’ont pas lieu. Si un avertissement de lien est émis, les liens ne sont pas créés automatiquement, même si les autorisations sont en place. L'administrateur doit alors créer les liens manuellement.  
  
### <a name="233-managing-gpos-in-a-multi-domain-controller-environment"></a>2.3.3 gestion des objets de stratégie de groupe dans un environnement à plusieurs domaines  
Chaque objet de stratégie de groupe sera géré par un contrôleur de domaine spécifique, comme suit :  
  
-   L’objet de stratégie de groupe de serveur est géré par l’un des contrôleurs de domaine dans le Active Directory site associé au serveur, ou si les contrôleurs de domaine de ce site sont en lecture seule, par un contrôleur de domaine accessible en écriture le plus proche du premier serveur dans le point d’entrée.  
  
-   Les objets de stratégie de groupe client sont gérés par le contrôleur de domaine qui s’exécute en tant qu’émulateur PDC.  
  
Si vous souhaitez modifier manuellement les paramètres de l’objet de stratégie de groupe, notez les points suivants :  
  
-   Pour les objets de stratégie de groupe de serveur, afin d’identifier le contrôleur de domaine associé à un point d’entrée spécifique, exécutez l’applet de commande PowerShell `Get-DAEntryPointDC -EntryPointName <name of entry point>`.  
  
-   Par défaut, lorsque vous apportez des modifications à l’aide des applets de commande PowerShell de mise en réseau ou de la console de gestion stratégie de groupe, le contrôleur de domaine jouant le rôle d’émulateur de contrôleur de domaine principal est utilisé.  
  
-   Si vous modifiez des paramètres sur un contrôleur de domaine qui n’est pas le contrôleur de domaine associé au point d’entrée (pour les objets de stratégie de groupe de serveur) ou l’émulateur de contrôleur de domaine principal (pour les objets de stratégie de groupe client), notez les points suivants :  
  
    1.  Avant de modifier les paramètres, assurez-vous que le contrôleur de domaine est répliqué avec un objet de stratégie de groupe à jour et [Sauvegardez les paramètres des objets de stratégie de groupe](https://go.microsoft.com/fwlink/?LinkID=257928), avant d’apporter des modifications. Si l’objet de stratégie de groupe n’est pas mis à jour, des conflits de fusion lors de la réplication peuvent se produire, provoquant une corruption de la configuration de l’accès  
  
    2.  Après avoir modifié les paramètres, vous devez attendre que les modifications soient répliquées sur le contrôleur de domaine associé aux objets de stratégie de groupe. N’apportez pas de modifications supplémentaires à l’aide de la console de gestion de l’accès à distance ou des applets de commande PowerShell d’accès à distance jusqu’à ce que la réplication Si un objet de stratégie de groupe est modifié sur deux contrôleurs de domaine différents avant la fin de la réplication, des conflits de fusion peuvent se produire, ce qui entraîne une configuration endommagée.  
  
-   Vous pouvez également modifier le paramètre par défaut à l’aide de la boîte de dialogue **modifier le contrôleur de domaine** dans la console de gestion stratégie de groupe, ou à l’aide de l’applet de commande PowerShell **Open-NetGPO** , afin que les modifications effectuées à l’aide de la console ou des applets de commande de mise en réseau Utilisez le contrôleur de domaine que vous spécifiez.  
  
    1.  Pour effectuer cette opération dans la console de gestion de stratégie de groupe, cliquez avec le bouton droit sur le conteneur domaine ou sites, puis cliquez sur **modifier le contrôleur de domaine**.  
  
    2.  Pour effectuer cette opération dans PowerShell, spécifiez le paramètre DomainController pour l’applet de commande Open-NetGPO. Par exemple, pour activer les profils privés et publics dans le pare-feu Windows sur un objet de stratégie de groupe nommé domain1\DA_Server_GPO _Europe à l’aide d’un contrôleur de domaine nommé europe-dc.corp.contoso.com, procédez comme suit :  
  
        ```  
        $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
        Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
        Save-NetGPO -GpoSession $gpoSession  
        ```  
  
#### <a name="modifying-domain-controller-association"></a>Modification de l’Association du contrôleur de domaine  
Pour assurer la cohérence de la configuration dans un déploiement multisite, il est important de veiller à ce que chaque objet de stratégie de groupe soit géré par un seul contrôleur de domaine. Dans certains scénarios, il peut être nécessaire d’assigner un autre contrôleur de domaine pour un objet de stratégie de groupe :  
  
-   **Maintenance et temps d’arrêt du contrôleur de domaine**: lorsque le contrôleur de domaine qui gère un objet de stratégie de groupe n’est pas disponible, il peut être nécessaire de gérer l’objet de stratégie de groupe sur un autre contrôleur de domaine.  
  
-   **Optimisation de la distribution de la configuration**: après modification de l’infrastructure réseau, il peut être nécessaire de gérer l’objet de stratégie de groupe du serveur d’un point d’entrée sur un contrôleur de domaine dans le même site de Active Directory que le point d’entrée.   
  
## <a name="bkmk_2_4_DNS"></a>2,4 planifier le DNS  
Notez les points suivants lors de la planification du DNS pour un déploiement multisite :  
  
1.  Les ordinateurs clients utilisent l’adresse ConnectTo pour se connecter au serveur d’accès à distance. Chaque point d’entrée de votre déploiement requiert une adresse ConnectTo différente. Chaque adresse ConnectTo de point d’entrée doit être disponible dans le DNS public et l’adresse que vous choisissez doit correspondre au nom d’objet du certificat IP-HTTPs que vous déployez pour la connexion IP-HTTPs.   
  
2.  En outre, l’accès à distance applique le routage symétrique. par conséquent, seuls les clients Teredo et IP-HTTPs peuvent se connecter lorsqu’un déploiement multisite est activé. Pour permettre aux clients IPv6 natifs de se connecter, l’adresse ConnectTo (URL IP-HTTPs) doit être convertie en adresses IPv6 et IPv4 externes (accessibles sur Internet) sur le serveur d’accès à distance.  
  
3.  L'accès à distance crée une sonde web par défaut utilisée par les ordinateurs clients DirectAccess pour vérifier la connectivité au réseau interne. Lors de la configuration du serveur unique, les noms suivants doivent avoir été enregistrés dans DNS :  
  
    1.  DirectAccess-WebProbeHost : doit correspondre à l’adresse IPv4 interne du serveur d’accès à distance, ou à l’adresse IPv6 dans un environnement IPv6 uniquement.  
  
    2.  DirectAccess-CorpConnectivityHost : doit correspondre à une adresse localhost (bouclage) ( :: 1 ou 127.0.0.1, selon que le protocole IPv6 est déployé ou non sur le réseau de l’entreprise).  
  
    Dans un déploiement multisite, une entrée DNS supplémentaire pour DirectAccess-WebProbeHost doit être créée pour chaque point d’entrée. Lors de l’ajout d’un point d’entrée, l’accès à distance tente de créer automatiquement cette entrée DirectAccess-WebProbeHost supplémentaire. Toutefois, en cas d’échec, un avertissement s’affiche et vous devez créer l’entrée manuellement.  
  
    > [!NOTE]  
    > Lorsque le nettoyage DNS est activé dans votre infrastructure DNS, il est recommandé de désactiver le nettoyage sur les entrées DNS créées automatiquement par l’accès à distance.  
  

  



