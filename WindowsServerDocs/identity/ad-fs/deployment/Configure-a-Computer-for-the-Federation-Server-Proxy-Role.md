---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: Configurer un ordinateur pour le rôle de serveur proxy de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: d47f7d3985aa779276f0712347eb9030857cefdb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359791"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurer un ordinateur pour le rôle de serveur proxy de fédération

Une fois que vous avez configuré un ordinateur avec les certificats requis et que vous avez installé le service de rôle proxy FSP (Federation Service Proxy), vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur proxy de Fédération. Vous pouvez utiliser la procédure suivante pour que l’ordinateur tienne le rôle de serveur proxy de fédération.  
  
> [!IMPORTANT]  
> Avant d’utiliser cette procédure pour configurer l’ordinateur proxy du serveur de Fédération, vérifiez que vous avez suivi toutes les étapes de la [liste de vérification : configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md) dans l’ordre dans lequel ils sont répertoriés. Veillez à ce qu’au moins un serveur de fédération soit déployé et que toutes les informations d’identification nécessaires pour autoriser la configuration d’un serveur proxy de fédération sont implémentées. Vous devez également configurer protocole SSL \(les liaisons de\) SSL sur le site Web par défaut, sinon l’Assistant ne démarrera pas. Toutes ces tâches doivent être exécutées avant que le serveur proxy de fédération ne puisse fonctionner.  
  
Quand vous avez fini de configurer l’ordinateur, vérifiez que le serveur proxy de fédération fonctionne comme prévu. Pour plus d'informations, voir [Vérifier qu'un serveur proxy de fédération est opérationnel](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Pour configurer un ordinateur pour le rôle de serveur proxy de fédération  
  
1.  Il existe deux façons de démarrer l’Assistant Configuration du serveur de fédération AD FS. Pour démarrer l'Assistant, effectuez l'une des opérations suivantes :  
  
    -   Dans l’écran d' **Accueil** , tapez**AD FS Assistant Configuration du serveur proxy de Fédération**, puis appuyez sur entrée.  
  
    -   À chaque fois que l’Assistant installation est terminé, ouvrez l’Explorateur Windows, accédez au dossier **C :\\windows\\ADFS** , puis double\-cliquez sur **FspConfigWizard. exe**.  
  
2.  Quelle que soit la méthode choisie, démarrez ensuite l'Assistant et, dans la page **Bienvenue**, cliquez sur **Suivant**.  
  
3.  Dans la page **Spécifier le nom du service FS**, sous **Nom du service de fédération**, tapez le nom désignant le service de fédération pour lequel cet ordinateur aura un rôle de proxy.  
  
4.  En fonction de la configuration requise pour votre réseau, déterminez si vous avez besoin d'un serveur proxy HTTP pour transférer les requêtes au service de fédération. Si c'est le cas, cochez la case **Utiliser un serveur proxy HTTP pour l'envoi de requêtes à ce service de fédération**, sous **Adresse du serveur proxy HTTP**, tapez l'adresse du serveur proxy, cliquez sur **Tester la connexion** pour vérifier la connectivité, puis cliquez sur **Suivant**.  
  
5.  Lorsque vous y êtes invité, entrez les informations d'identification nécessaires pour établir une relation d'approbation entre ce serveur proxy de fédération et le service de fédération.  
  
    Par défaut, seul le compte de service utilisé par l’service FS (Federation Service) ou un membre du groupe local Administrateurs de\\BUILTIN peut autoriser un serveur proxy de Fédération.  
  
6.  Dans la page **Prêt à appliquer les paramètres**, vérifiez les paramètres définis. Si les paramètres sont corrects, cliquez sur **Suivant** pour commencer à configurer l'ordinateur avec ces paramètres de proxy.  
  
7.  Dans la page **Résultats de la configuration**, examinez les résultats. Une fois toutes les étapes de configuration terminées, cliquez sur **Fermer** pour quitter l’Assistant.  
  
    Il n’existe aucune console MMC (Microsoft Management Console) \(MMC\)\-Snap dans à utiliser pour l’administration des serveurs proxys de Fédération. Pour configurer les paramètres de chacun des serveurs proxys de Fédération de votre organisation, utilisez les applets de commande Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configuration d’un autre port TCP\/TCP pour les opérations de proxy  
Par défaut, le service de serveur proxy de Fédération est configuré pour utiliser le port TCP 443 pour le trafic HTTPs et le port 80 pour le trafic HTTP pour la communication avec le serveur de Fédération. Pour configurer des ports différents, par exemple le port TCP 444 pour le trafic HTTPS et le port 81 pour le trafic HTTP, effectuez les tâches décrites ci-dessous.  
  
> [!NOTE]  
> Si vous envisagez de déployer initialement AD FS pour qu’il fonctionne sous d’autres ports TCP\/IP, vous devez d’abord modifier les ports dans vos liaisons de protocole IIS pour HTTP et HTTPs sur le serveur de Fédération et les serveurs proxy de Fédération. Cela doit se produire avant d’exécuter les assistants de configuration AD FS pour la configuration initiale. Si vous configurez Internet Information Services \(IIS\) tout d’abord, vos paramètres de port IP TCP\/alternatifs sont découverts lorsque la configuration basée sur l’Assistant\-se produit dans AD FS, et la procédure suivante n’est pas nécessaire. Si vous voulez modifier les paramètres de port définis initialement, mettez d'abord à jour les liaisons de protocole IIS, puis suivez la procédure ci-dessous pour mettre à jour les paramètres de port de façon appropriée. Pour plus d’informations sur la modification des liaisons IIS, voir l' [article 149605](https://go.microsoft.com/fwlink/?LinkId=190275) de la base de connaissances Microsoft.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Pour configurer d’autres ports TCP\/IP pour le serveur proxy de Fédération à utiliser  
  
1.  Configurez le serveur de fédération pour qu'il utilise d'autres ports que ceux prévus par défaut.  
  
    Pour ce faire, spécifiez le numéro de port par défaut en l’incluant avec les options *HttpsPort* et *HttpPort* dans le cadre de l’applet de commande **Set\-ADFSProperties** . Par exemple, pour configurer ces ports, utilisez les commandes suivantes dans la session Windows PowerShell sur l’ordinateur serveur de Fédération :  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configurez le serveur proxy de Fédération pour qu’il utilise le port non défini par défaut.  
  
    Pour ce faire, spécifiez le numéro de port par défaut en l’incluant avec les options *HttpsPort* et *HttpPort* dans le cadre de l’applet de commande **Set\-ADFSProxyProperties** . Par exemple, pour configurer ces ports, utilisez les commandes suivantes dans la session Windows PowerShell sur l’ordinateur serveur de Fédération :  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > Les URL de point de terminaison ne sont pas activées par défaut pour le service de serveur proxy de Fédération. Si vous configurez une nouvelle installation de serveur de Fédération, vous devez d’abord activer les points de terminaison de service de serveur proxy de Fédération. Par exemple, il est supposé que pour tous les points de terminaison que l’exemple de cette procédure fait référence, vous les avez activés pour le proxy en les sélectionnant dans le\-du composant logiciel enfichable Gestion de AD FS, puis en sélectionnant **activer sur le proxy**.  
  
3.  Mettez à jour l’installation d’IIS sur le serveur proxy de Fédération afin que Security Assertion Markup Language \(\) SAML et les points de terminaison d’approbation WS\-soient configurés pour refléter le numéro de port mis à jour. Pour ce faire, vous pouvez utiliser le bloc-notes pour modifier les éléments suivants dans le fichier Web. config, qui se trouve à l’emplacement lecteur_système%\\Inetpub\\ADFS\\LS\\ sur l’ordinateur du serveur proxy de Fédération. Par exemple, en supposant que vous disposez d’un serveur de Fédération nommé sts1.contoso.com et que le nouveau numéro de port est 444, recherchez et ouvrez le fichier Web. config dans le bloc-notes sur l’ordinateur proxy de serveur de Fédération, recherchez la section suivante, modifiez le numéro de port comme suit : en surbrillance ci-dessous, puis enregistrez et quittez le bloc-notes.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Ajoutez le compte d’utilisateur du service de serveur proxy de Fédération à la liste de contrôle d’accès \(\) ACL pour les URL de point de terminaison associées. Par exemple, si le numéro de port est 1234 et que le compte d’utilisateur utilisé pour exécuter le service proxy du serveur AD FSfederation sous est le\-créé dans le compte de service réseau, tapez la commande suivante à l’invite de commandes :  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    Les commandes précédentes doivent être exécutées sur le serveur de Fédération et sur les ordinateurs proxy du serveur de Fédération.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur proxy de Fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

