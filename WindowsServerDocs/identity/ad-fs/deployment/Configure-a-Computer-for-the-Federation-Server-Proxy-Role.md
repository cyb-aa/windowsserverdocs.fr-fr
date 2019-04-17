---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: "Configurer un ordinateur pour le rôle de Proxy de serveur de fédération"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 2a89bab2fd1af1a1d7234da29f2025b4b12d6774
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurer un ordinateur pour le rôle de Proxy de serveur de fédération

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Après avoir configuré un ordinateur avec les certificats requis et que vous avez installé le service de rôle Proxy du Service de fédération, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur proxy de fédération. Vous pouvez utiliser la procédure suivante pour que l’ordinateur tienne le rôle de proxy de serveur de fédération.  
  
> [!IMPORTANT]  
> Avant d’utiliser cette procédure pour configurer l’ordinateur de proxy de serveur de fédération, assurez-vous que vous avez suivi toutes les étapes de [liste de vérification: configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md) dans l’ordre d’apparition. Assurez-vous qu’au moins un serveur de fédération est déployé et que tous les informations d’identification pour autoriser une configuration serveur proxy de fédération sont implémentées. Vous devez également configurer les liaisons \(SSL\) (Secure Sockets Layer) sur le site Web par défaut, ou si cet Assistant ne démarrera pas. Toutes ces tâches doivent être effectuées avant que ce serveur proxy de fédération puisse fonctionner.  
  
Après avoir terminé la configuration de l’ordinateur, vérifiez que le serveur proxy de fédération fonctionne comme prévu. Pour plus d’informations, voir [Vérifiez qu’un Federation Server Proxy est opérationnel](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
L’appartenance au groupe **administrateurs**, ou équivalente, sur l’ordinateur local est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Pour configurer un ordinateur pour le rôle de proxy de serveur de fédération  
  
1.  Il existe deux façons de démarrer l’Assistant Configuration du serveur de fédération ADFS. Pour démarrer l’Assistant, effectuez l’une des opérations suivantes:  
  
    -   Sur le **Démarrer**, tapez**Assistant Configuration de Proxy du serveur de fédération ADFS**, puis appuyez sur ENTRÉE.  
  
    -   À tout moment une fois l’Assistant Installation terminé, ouvrez l’Explorateur Windows, accédez à la **C:\\Windows\\ADFS** dossier, puis double-cliquant sur **FspConfigWizard.exe**.  
  
2.  À l’aide de l’autre méthode, démarrez l’Assistant, puis, dans le **Bienvenue**, cliquez sur **suivant**.  
  
3.  Sur le **spécifier un nom de Service de fédération** sous **nom du Service de fédération**, tapez le nom qui représente le Service de fédération pour lequel cet ordinateur tiendra le rôle de proxy.  
  
4.  Selon vos besoins en matière de réseau spécifique, déterminez si vous devez utiliser un serveur proxy HTTP pour transférer les demandes au Service de fédération. Si tel est le cas, sélectionnez le **utiliser un serveur proxy HTTP lors de l’envoi des demandes à ce Service de fédération** case à cocher sous **adresse du serveur proxy HTTP** tapez l’adresse du serveur proxy, cliquez sur **tester la connexion** pour vérifier la connectivité, puis cliquez sur **suivant**.  
  
5.  Lorsque vous y êtes invité, spécifiez les informations d’identification qui sont nécessaires pour établir une relation d’approbation entre ce serveur proxy de fédération et le Service de fédération.  
  
    Par défaut, seul le compte de service utilisé par le Service de fédération ou un membre du groupe local BUILTIN\\Administrateurs peut autoriser un serveur proxy de fédération.  
  
6.  Sur le **prêt à appliquer les paramètres** page, passez en revue les détails. Si les paramètres sont corrects, cliquez sur **suivant** pour commencer à configurer cet ordinateur avec ces paramètres de proxy.  
  
7.  Sur le **résultats de la Configuration** page, passez en revue les résultats. Lorsque toutes les étapes de configuration sont terminées, cliquez sur **fermer** pour quitter l’Assistant.  
  
    Il n’existe aucune \(MMC\) snap\ MicrosoftManagement Console-dans à utiliser pour administrer les proxys de serveur de fédération. Pour configurer les paramètres pour chacun des proxys de serveur de fédération dans votre organisation, utilisez les applets de commande Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>La configuration d’un autre Port TCP\/IP pour les opérations de Proxy  
Par défaut, le service de proxy de serveur de fédération est configuré pour utiliser le port TCP 443 pour le trafic HTTPS et le port 80 pour le trafic HTTP pour la communication avec le serveur de fédération. Pour configurer des ports différents, tels que le port TCP 444 pour le protocole HTTPS et le port 81 pour le protocole HTTP, les tâches suivantes doivent être effectuées.  
  
> [!NOTE]  
> Si vous envisagez de déployer initialement ADFS pour fonctionner sous d’autres ports TCP\/IP, vous devez tout d’abord modifier les ports dans vos liaisons de protocole IIS pour HTTP et HTTPS sur le serveur de fédération et les ordinateurs de proxy de serveur de fédération. Cela doit se produire avant d’exécuter les Assistants de configuration ADFS pour la configuration initiale. Si vous configurez d’abord \(IIS\) Internet Information Services, vos paramètres de port alternatifs TCP\/IP sont découverts lors de la configuration basée sur les assistant\ se produit dans ADFS, et la procédure suivante n’est pas nécessaire. Si vous souhaitez modifier les paramètres de port ultérieurement, mettre à jour les liaisons de protocole IIS tout d’abord, puis utilisez la procédure suivante pour mettre à jour les paramètres de port en conséquence. Pour plus d’informations sur la modification des liaisons IIS, consultez [article 149605](https://go.microsoft.com/fwlink/?LinkId=190275) dans la Base de connaissances Microsoft.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Pour configurer d’autres ports TCP\/IP pour le serveur proxy de fédération à utiliser  
  
1.  Configurer le serveur de fédération pour utiliser les ports par défaut.  
  
    Pour ce faire, spécifiez le numéro de port par défaut en l’ajoutant avec la *HttpsPort* et *HttpPort* options dans le cadre de la **Set-ADFSProperties** applet de commande. Par exemple, pour configurer ces ports, utilisez les commandes suivantes dans la session Windows PowerShell sur l’ordinateur de serveur de fédération:  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configurer le serveur proxy de fédération pour utiliser le port par défaut.  
  
    Pour ce faire, spécifiez le numéro de port par défaut en l’ajoutant avec la *HttpsPort* et *HttpPort* options dans le cadre de la **Set-ADFSProxyProperties** applet de commande. Par exemple, pour configurer ces ports, utilisez les commandes suivantes dans la session Windows PowerShell sur l’ordinateur de serveur de fédération:  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > URL de point de terminaison ne sont pas activées par défaut pour le service de proxy de serveur de fédération. Si vous configurez une nouvelle installation de serveur de fédération, vous devez activer les points de terminaison service federation server proxy tout d’abord. Par exemple, il est supposé que pour tous les points de terminaison qui désigne l’exemple dans cette procédure vous avez activé pour le proxy en les sélectionnant dans la gestion ADFS enfichable puis en sélectionnant **activer sur le proxy**.  
  
3.  Mettre à jour l’installation d’IIS sur le serveur proxy de fédération afin que Security Assertion Markup Language \(SAML\) et les points de terminaison WS-Trust sont configurés pour prendre en compte le numéro de port mis à jour. Pour ce faire, vous pouvez utiliser le bloc-notes pour modifier le code suivant dans le fichier Web.config, qui se trouve dans systemdrive%\\inetpub\\adfs\\ls\\ sur l’ordinateur de proxy de serveur de fédération. Par exemple, en supposant que vous disposez d’un serveur de fédération nommé sts1.contoso.com et le nouveau numéro de port 444, recherchez et ouvrez le fichier Web.config dans le bloc-notes sur l’ordinateur de proxy de serveur de fédération, recherchez la section suivante, modifiez le numéro de port comme indiqué ci-dessous, puis enregistrez et fermez le bloc-notes.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Ajouter le compte d’utilisateur de service de proxy de fédération serveur pour le contrôle d’accès de liste \(ACL\) pour les URL de point de terminaison associées. Par exemple, si le numéro de port est 1234 et que le compte d’utilisateur qui est utilisé pour exécuter le service du serveur proxy AD FSfederation sous est le compte de Service réseau intégrée, tapez la commande suivante à une invite de commandes:  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    Les commandes précédentes doivent être exécutées sur le serveur de fédération et les serveurs proxy de fédération.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification: Configuration d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

