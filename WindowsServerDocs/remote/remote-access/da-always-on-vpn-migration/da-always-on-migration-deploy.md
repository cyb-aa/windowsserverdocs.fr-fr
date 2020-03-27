---
title: Migration de DirectAccess vers Always On VPN
description: La migration de DirectAccess vers Always On VPN nécessite un processus spécifique pour migrer les clients, ce qui permet de réduire les conditions de concurrence qui résultent de l’exécution des étapes de migration dans le désordre.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: lizross
author: eross-msft
ms.date: 06/07/2018
ms.openlocfilehash: 9f78edf0e48dc914b09a5e6f2d054e0fafba62e3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80309304"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>Migrer vers VPN Toujours actif (AlwaysOn) et retirer DirectAccess

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

&#171;[ **Précédent :** planifier DirectAccess pour Always on la migration VPN](da-always-on-migration-planning.md)<br>

La migration de DirectAccess vers Always On VPN nécessite un processus spécifique pour migrer les clients, ce qui permet de réduire les conditions de concurrence qui résultent de l’exécution des étapes de migration dans le désordre. À un niveau élevé, le processus de migration comprend les quatre principales étapes suivantes :

1.  **Déployez une infrastructure VPN côte à côte.** Une fois que vous avez déterminé vos phases de migration et les fonctionnalités que vous souhaitez inclure dans votre déploiement, vous allez déployer l’infrastructure VPN côte à côte avec l’infrastructure DirectAccess existante.  

2.  **Déployez les certificats et la configuration sur les clients.**  Une fois l’infrastructure VPN prête, vous créez et publiez les certificats requis sur le client. Lorsque les clients ont reçu les certificats, vous déployez le script de configuration VPN_Profile. ps1. Vous pouvez également utiliser Intune pour configurer le client VPN. Utilisez le Configuration Manager de point de terminaison Microsoft ou Microsoft Intune pour surveiller les déploiements de configuration VPN réussis.

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

Avant de commencer le processus de migration de DirectAccess vers Always On VPN, assurez-vous que vous avez planifié la migration.  Si vous n’avez pas planifié votre migration, consultez [planifier la migration de DirectAccess vers Always on VPN](da-always-on-migration-planning.md).

>[!TIP] 
>Cette section n’est pas un guide de déploiement pas à pas pour Always On VPN mais est plutôt destiné à compléter [Always on déploiement VPN pour Windows Server et Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) et à fournir des conseils de déploiement spécifiques à la migration.

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>Déployer une infrastructure VPN côte à côte

Vous déployez l’infrastructure VPN côte à côte avec l’infrastructure DirectAccess existante.  Pour obtenir des détails pas à pas, consultez [Always on le déploiement VPN pour Windows Server et Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) pour installer et configurer l’infrastructure VPN Always on. 

Le déploiement côte à côte se compose des tâches de haut niveau suivantes :

1.  Créez les groupes utilisateurs VPN, serveurs VPN et serveurs NPS.

2.  Créez et publiez les modèles de certificats nécessaires.

3.  Inscrivez les certificats de serveur.

4.  Installez et configurez le service d’accès à distance pour Always On VPN.

5.  Installez et configurez NPS.

6.  Configurez les règles de pare-feu et DNS pour Always On VPN.

L’image suivante fournit une référence visuelle pour les modifications d’infrastructure tout au long de la migration de DirectAccess vers le Always On VPN.

![](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>Déployer des certificats et un script de configuration VPN sur les clients

Bien que l’essentiel de la configuration du client VPN dans la section [déployer Always on VPN](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md) , deux étapes supplémentaires sont nécessaires pour terminer la migration de DirectAccess vers Always on VPN. 

Vous devez vous assurer que le **VPN_Profile. ps1** vient _après_ l’émission du certificat afin que le client VPN ne tente pas de se connecter sans lui. Pour ce faire, vous exécutez un script qui ajoute uniquement les utilisateurs qui ont inscrit le certificat à votre groupe prêt pour le déploiement VPN, que vous utilisez pour déployer la configuration VPN Always On.

>[!NOTE] 
>Microsoft vous recommande de tester ce processus avant de l’exécuter sur l’un des anneaux de migration de vos utilisateurs.

1.  **Créez et publiez le certificat VPN, puis activez l’inscription automatique stratégie de groupe l’objet (GPO).** Pour les déploiements VPN Windows 10 basés sur des certificats traditionnels, un certificat est émis à l’appareil ou à l’utilisateur afin qu’il puisse authentifier la connexion. Lorsque le nouveau certificat d’authentification est créé et publié pour l’inscription automatique, vous devez créer et déployer un objet de stratégie de groupe avec le paramètre d’inscription automatique configuré sur le groupe d’utilisateurs VPN. Pour connaître les étapes de configuration des certificats et de l’inscription automatique, consultez [configurer l’infrastructure de serveur](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md).

2.  **Ajoutez des utilisateurs au groupe d’utilisateurs VPN.** Ajoutez les utilisateurs que vous migrez vers le groupe d’utilisateurs VPN. Ces utilisateurs restent dans ce groupe de sécurité une fois que vous les avez migrés pour qu’ils puissent recevoir des mises à jour de certificats à l’avenir. Continuez à ajouter des utilisateurs à ce groupe tant que vous n’avez pas déplacé chaque utilisateur de DirectAccess vers Always On VPN. 

3.  **Identifiez les utilisateurs qui ont reçu un certificat d’authentification VPN.** Vous effectuez une migration à partir de DirectAccess. vous devez donc ajouter une méthode pour identifier le moment où un client a reçu le certificat requis et est prêt à recevoir les informations de configuration du VPN. Exécutez le script **GetUsersWithCert. ps1** pour ajouter à un groupe de sécurité AD DS spécifié des utilisateurs qui reçoivent actuellement des certificats non révoqués provenant du nom de modèle spécifié. Par exemple, après l’exécution du script **GetUsersWithCert. ps1** , tout utilisateur ayant émis un certificat valide à partir du modèle de certificat d’authentification VPN est ajouté au groupe de déploiement prêt VPN.

    >[!NOTE] 
    >Si vous ne disposez pas d’une méthode permettant d’identifier le moment où un client a reçu le certificat requis, vous pouvez déployer la configuration VPN avant que le certificat n’ait été émis pour l’utilisateur, provoquant ainsi l’échec de la connexion VPN. Pour éviter cette situation, exécutez le script **GetUsersWithCert. ps1** sur l’autorité de certification ou selon un calendrier pour synchroniser les utilisateurs qui ont reçu le certificat avec le groupe de déploiement prêt pour le déploiement VPN. Vous allez ensuite utiliser ce groupe de sécurité pour cibler le déploiement de votre configuration VPN dans le point de terminaison Microsoft Configuration Manager ou Intune, ce qui garantit que le client géré ne reçoit pas la configuration VPN avant de recevoir le certificat.
    
    ### <a name="getuserswithcertps1"></a>GetUsersWithCert. ps1
    
    ```powershell
    Import-module ActiveDirectory
    Import-Module AdcsAdministration
    
    $TemplateName = 'VPNUserAuthentication'##Certificate Template Name (not the friendly name)
    $GroupName = 'VPN Deployment Ready' ##Group you will add the users to
    $CSServerName = 'localhost\corp-dc-ca' ##CA Server Information
    
    $users= @()
    $TemplateID = (get-CATemplate | Where-Object {$_.Name -like $TemplateName} | Select-Object oid).oid
    $View = New-Object -ComObject CertificateAuthority.View
    $NULL = $View.OpenConnection($CSServerName)
    $View.SetResultColumnCount(3)
    $i1 = $View.GetColumnIndex($false, "User Principal Name")
    $i2 = $View.GetColumnIndex($false, "Certificate Template")
    $i3 = $View.GetColumnIndex($false, "Revocation Date")
    $i1, $i2, $i3 | %{$View.SetResultColumn($_) }
    $Row= $View.OpenView()
    
    while ($Row.Next() -ne -1){
    $Cert = New-Object PsObject
    $Col = $Row.EnumCertViewColumn()
    [void]$Col.Next()
    do {
    $Cert | Add-Member -MemberType NoteProperty $($Col.GetDisplayName()) -Value $($Col.GetValue(1)) -Force
          }
    until ($Col.Next() -eq -1)
    $col = ''
    
    if($cert."Certificate Template" -eq $TemplateID -and $cert."Revocation Date" -eq $NULL){
       $users= $users+= $cert."User Principal Name"
    $temp = $cert."User Principal Name"
    $user = get-aduser -Filter {UserPrincipalName -eq $temp} –Property UserPrincipalName
    Add-ADGroupMember $GroupName $user
       }
      }
    ```

4. Déployez la configuration VPN Always On. À mesure que les certificats d’authentification VPN sont émis et que vous exécutez le script **GetUsersWithCert. ps1** , les utilisateurs sont ajoutés au groupe de sécurité prêt pour le déploiement VPN.


| Si vous utilisez...  | Conséquence |
| ---- | ---- |
| Gestionnaire de configurations | Créez un regroupement d’utilisateurs en fonction de l’appartenance de ce groupe de sécurité.<br><br>![](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png)!|
| Intune | Il vous suffit de cibler le groupe de sécurité directement une fois qu’il est synchronisé. |
|
    
Chaque fois que vous exécutez le script de configuration **GetUsersWithCert. ps1** , vous devez également exécuter une règle de détection AD DS pour mettre à jour l’appartenance au groupe de sécurité dans Configuration Manager. En outre, assurez-vous que la mise à jour d’appartenance pour la collection de déploiement se produit fréquemment (alignée avec la règle de script et de détection).

Pour plus d’informations sur l’utilisation de Configuration Manager ou Intune pour déployer des Always On VPN sur des clients Windows, consultez [Always on le déploiement VPN pour Windows Server et Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md). Toutefois, veillez à intégrer ces tâches spécifiques à la migration.

>[!NOTE] 
>L’incorporation de ces tâches spécifiques à la migration est une différence essentielle entre un déploiement simple Always On VPN et une migration de DirectAccess vers Always On VPN. Veillez à définir correctement la collection pour cibler le groupe de sécurité au lieu d’utiliser la méthode dans le Guide de déploiement.


## <a name="remove-devices-from-the-directaccess-security-group"></a>Supprimer des appareils du groupe de sécurité DirectAccess

À mesure que les utilisateurs reçoivent le certificat d’authentification et le script de configuration **VPN_Profile. ps1** , vous voyez les déploiements de script de configuration VPN réussis correspondants dans Configuration Manager ou Intune. Après chaque déploiement, supprimez l’appareil de cet utilisateur du groupe de sécurité DirectAccess afin de pouvoir supprimer DirectAccess ultérieurement. Intune et Configuration Manager contiennent des informations sur l’affectation des appareils utilisateur pour vous aider à déterminer l’appareil de chaque utilisateur.

>[!NOTE] 
>Si vous appliquez des objets de stratégie de groupe DirectAccess via des unités d’organisation (UO) plutôt que des groupes d’ordinateurs, déplacez l’objet ordinateur de l’utilisateur hors de l’unité d’organisation.

## <a name="decommission-the-directaccess-infrastructure"></a>Désaffectation de l’infrastructure DirectAccess

Une fois que vous avez terminé de migrer tous vos clients DirectAccess vers Always On VPN, vous pouvez désactiver l’infrastructure DirectAccess et supprimer les paramètres DirectAccess de stratégie de groupe. Microsoft recommande d’effectuer les étapes suivantes pour supprimer DirectAccess de votre environnement de manière appropriée :

1.  [!INCLUDE [remove-config-settings-shortdesc-include](../includes/remove-config-settings-shortdesc-include.md)]

    ![](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

3.  **Nettoyez le DNS.** Veillez à supprimer tous les enregistrements de votre serveur DNS interne et du serveur DNS public relatif à DirectAccess, par exemple, DA.contoso.com, DAGateway.contoso.com.

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

5.  **Supprimez tous les certificats DirectAccess des services de certificats Active Directory.** Si vous avez utilisé des certificats d’ordinateur pour votre implémentation DirectAccess, supprimez les modèles publiés du dossier modèles de certificats de la console autorité de certification.

## <a name="next-step"></a>Étape suivante
Vous avez terminé la migration de DirectAccess vers Always On VPN. 

---