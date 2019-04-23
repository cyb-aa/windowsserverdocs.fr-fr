---
title: Migration de DirectAccess pour VPN Always On
description: Migration de DirectAccess vers VPN Always On nécessite un processus spécifique pour migrer des clients, qui permet de réduire les conditions de concurrence surviennent d’effectuer les étapes de migration en désordre.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: eeca4cf7-90f0-485d-843c-76c5885c54b0
ms.author: pashort
author: shortpatti
ms.date: 06/07/2018
ms.openlocfilehash: 94852b33936ece0b329614f428e845f2c7a2ac6a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854580"
---
# <a name="migrate-to-always-on-vpn-and-decommission-directaccess"></a>Migrer vers VPN Toujours actif (AlwaysOn) et retirer DirectAccess

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows 10

&#171;[ **Précédente :** Planifier la DirectAccess pour la migration VPN Always On](da-always-on-migration-planning.md)<br>

Migration de DirectAccess vers VPN Always On nécessite un processus spécifique pour migrer des clients, qui permet de réduire les conditions de concurrence surviennent d’effectuer les étapes de migration en désordre. À un niveau élevé, le processus de migration se compose de ces quatre étapes principales :

1.  **Déployer une infrastructure VPN côte à côte.** Une fois que vous avez déterminé vos phases de migration et les fonctionnalités que vous souhaitez inclure dans votre déploiement, vous allez déployer l’infrastructure VPN côte à côte avec l’infrastructure DirectAccess existant.  

2.  **Déployer des certificats et configuration aux clients.**  Une fois que l’infrastructure VPN est prêt, vous créez et publiez les certificats requis sur le client. Lorsque les clients ont reçu les certificats, vous déployez le script de configuration VPN_Profile.ps1. Vous pouvez également utiliser Intune pour configurer le client VPN. Utilisez Microsoft System Center Configuration Manager ou Microsoft Intune pour surveiller les déploiements de configuration VPN réussis.

3.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

Avant de commencer le processus de migration de DirectAccess pour VPN Always On, veillez à ce que vous avez planifié pour la migration.  Si vous n’avez pas planifié votre migration, consultez [planifier DirectAccess pour la migration VPN Always On](da-always-on-migration-planning.md).

>[!TIP] 
>Cette section n’est pas un guide de déploiement étape par étape pour VPN Always On, mais vise plutôt à compléter [toujours sur VPN déploiement de Windows Server et Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) et fournissent des conseils de déploiement spécifique de la migration.

## <a name="deploy-a-side-by-side-vpn-infrastructure"></a>Déployer une infrastructure VPN côte à côte

Vous déployez l’infrastructure VPN côte à côte avec l’infrastructure DirectAccess existant.  Pour plus d’informations détaillées, consultez [toujours sur VPN déploiement de Windows Server et Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md) pour installer et configurer l’infrastructure VPN Always On. 

Déploiement de côte à côte comprend les tâches principales suivantes :

1.  Créez les groupes d’utilisateurs VPN, les serveurs VPN et les serveurs NPS.

2.  Créer et publier les modèles de certificat nécessaires.

3.  Inscrire les certificats de serveur.

4.  Installez et configurez le Service d’accès distant pour VPN Always On.

5.  Installez et configurez le serveur NPS.

6.  Configurer des règles de pare-feu et de DNS pour VPN Always On.

L’illustration suivante donne une représentation visuelle pour que les modifications d’infrastructure dans l’ensemble de DirectAccess-à-toujours sur la migration de VPN.

![](../../media/DA-to-AlwaysOnVPN/6b64f322f945f837f22a32bf87a228f8.png)

## <a name="deploy-certificates-and-vpn-configuration-script-to-the-clients"></a>Déployer des certificats et le script de configuration VPN sur les clients

Bien que la majeure partie de la configuration du client VPN dans le [déployer VPN Always On](../vpn/always-on-vpn/deploy/always-on-vpn-deploy-deployment.md) section, deux autres étapes sont nécessaires pour effectuer la migration de DirectAccess vers VPN Always On avec succès. 

Vous devez vous assurer que le **VPN_Profile.ps1** vient _après_ le certificat a été émis afin que le client VPN ne tente pas de se connecter sans lui. Pour ce faire, vous exécutez un script qui ajoute uniquement les utilisateurs qui ont inscrit dans le certificat à votre groupe VPN déploiement prêt, ce qui vous permet de déployer la configuration de VPN Always On.

>[!NOTE] 
>Microsoft recommande de tester ce processus avant d’effectuer cela sur un de vos boucles de migration utilisateur.

1.  **Créer et publier le certificat VPN et l’objet de stratégie de groupe (GPO)-inscription automatique.** Pour les déploiements de VPN Windows 10 traditionnels, basée sur certificat, un certificat est émis pour l’appareil ou l’utilisateur afin qu’il peut s’authentifier la connexion. Lorsque le nouveau certificat d’authentification est créé et publié pour l’inscription automatique, vous devez créer et déployer un GPO avec le paramètre de l’inscription automatique configuré pour le groupe d’utilisateurs VPN. Pour découvrir comment configurer des certificats et l’inscription automatique, consultez [configurer l’infrastructure de serveur](../vpn/always-on-vpn/deploy/vpn-deploy-server-infrastructure.md).

2.  **Ajouter des utilisateurs au groupe utilisateurs de VPN.** Ajouter des utilisateurs quelle que soit la migration vers le groupe d’utilisateurs VPN. Ces utilisateurs restent dans ce groupe de sécurité une fois que vous avez migré les afin qu’ils peuvent recevoir des mises à jour du certificat à l’avenir. Continuez à ajouter des utilisateurs à ce groupe jusqu'à ce que vous avez déplacé tous les utilisateurs de DirectAccess pour VPN Always On. 

3.  **Identifiez les utilisateurs qui ont reçu un certificat d’authentification VPN.** Vous effectuez une migration de DirectAccess, donc vous devez ajouter une méthode permettant d’identifier quand un client a reçu le certificat requis et est prêt à recevoir les informations de configuration de VPN. Exécutez le **GetUsersWithCert.ps1** script pour ajouter des utilisateurs qui sont actuellement nonrevoked certificats émis d’origine à partir du nom de modèle spécifié à un groupe de sécurité AD DS spécifié. Par exemple, après l’exécution du **GetUsersWithCert.ps1** script, tout utilisateur un certificat valide émis le certificat d’authentification VPN modèle est ajouté au groupe de déploiement de VPN prêt.

    >[!NOTE] 
    >Si vous n’avez pas une méthode pour identifier quand un client a reçu le certificat requis, vous pouvez déployer la configuration de VPN avant que le certificat a été émis à l’utilisateur, à l’origine de l’échec de la connexion VPN. Pour éviter cette situation, exécutez le **GetUsersWithCert.ps1** script sur l’autorité de certification ou selon une planification pour synchroniser les utilisateurs qui ont reçu le certificat pour le groupe de déploiement de VPN prêt. Vous utiliserez ensuite ce groupe de sécurité pour cibler votre déploiement de configuration de VPN dans System Center Configuration Manager ou Intune, ce qui garantit que le client géré ne reçoit pas de la configuration VPN avant d’avoir reçu le certificat.
    
    ### <a name="getuserswithcertps1"></a>GetUsersWithCert.ps1
    
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

4. Déployer la configuration de VPN Always On. En tant que la connexion VPN sont émis les certificats d’authentification, et que vous exécutez le **GetUsersWithCert.ps1** script, les utilisateurs sont ajoutés au groupe de sécurité VPN déploiement prêt.


| Si vous utilisez...  | Alors… |
| ---- | ---- |
| System Center Configuration Manager | Créer un regroupement d’utilisateurs basé sur l’appartenance de ce groupe de sécurité.<br><br>![](../../media/DA-to-AlwaysOnVPN/b38723b3ffcfacd697b83dd41a177f66.png)\!|
| Intune | Simplement cibler directement le groupe de sécurité une fois qu’il soit synchronisé. |
|
    
Chaque fois que vous exécutez le **GetUsersWithCert.ps1** script de configuration, vous devez également exécuter une règle de détection des services AD DS pour mettre à jour l’appartenance au groupe de sécurité dans System Center Configuration Manager. En outre, assurez-vous que la mise à jour de l’appartenance pour le regroupement de déploiement se produit fréquemment suffisamment (aligné avec la règle de découverte et de script).

Pour plus d’informations sur l’utilisation de System Center Configuration Manager ou Intune pour déployer le VPN Always On pour les clients Windows, consultez [toujours sur VPN déploiement de Windows Server et Windows 10](../vpn/always-on-vpn/deploy/always-on-vpn-deploy.md). Veillez, toutefois, pour intégrer ces tâches de migration spécifique.

>[!NOTE] 
>L’incorporation de ces tâches de migration spécifique est une différence importante entre un simple déploiement de VPN Always On et la migration de DirectAccess pour VPN Always On. Veillez à définir correctement la collection pour cibler le groupe de sécurité plutôt que d’à l’aide de la méthode dans le guide de déploiement.


## <a name="remove-devices-from-the-directaccess-security-group"></a>Supprimer des appareils du groupe de sécurité de DirectAccess

Comme les utilisateurs reçoivent le certificat d’authentification et le **VPN_Profile.ps1** script de configuration, vous voyez correspondant réussites déploiements de script de configuration de VPN dans System Center Configuration Manager ou Intune. Après chaque déploiement, supprimer les périphériques de cet utilisateur à partir du groupe de sécurité de DirectAccess afin que vous pouvez supprimer ultérieurement de DirectAccess. Intune et System Center Configuration Manager contient des informations sur les affectations d’appareil utilisateur pour vous aider à déterminer les appareils de chaque utilisateur.

>[!NOTE] 
>Si vous appliquez des objets stratégie de groupe DirectAccess via des unités d’organisation (UO) au lieu de groupes d’ordinateurs, de déplacer l’objet ordinateur de l’utilisateur en dehors de l’unité d’organisation.

## <a name="decommission-the-directaccess-infrastructure"></a>Retirer l’infrastructure DirectAccess

Lorsque vous avez terminé la migration de tous vos clients DirectAccess pour VPN Always On, vous pouvez désaffecter l’infrastructure DirectAccess et supprimer les paramètres DirectAccess à partir de la stratégie de groupe. Microsoft vous recommande d’effectuer les étapes suivantes pour supprimer correctement les DirectAccess de votre environnement :

1.  [!INCLUDE [remove-config-settings-shortdesc-include](../includes/remove-config-settings-shortdesc-include.md)]

    ![](../../media/DA-to-AlwaysOnVPN/dbdc3d80e8dc1b8665f7b15d7d2ee1f6.png)

2.  [!INCLUDE [remove-da-security-group-shortdesc-include](../includes/remove-da-security-group-shortdesc-include.md)]

3.  **Nettoyer le DNS.** Veillez à supprimer tous les enregistrements à partir de votre serveur DNS interne et le serveur DNS public liés à DirectAccess, par exemple, DA.contoso.com, DAGateway.contoso.com.

4.  [!INCLUDE [decommission-da-shortdesc-include](../includes/decommission-da-shortdesc-include.md)]

5.  **Supprimez tous les certificats DirectAccess à partir des Services de certificats Active Directory.** Si vous avez utilisé des certificats d’ordinateur pour votre implémentation de DirectAccess, supprimez les modèles publiés à partir du dossier de modèles de certificats dans la console Autorité de Certification.

## <a name="next-step"></a>Étape suivante
Vous avez terminé la migration de DirectAccess pour VPN Always On. 

---