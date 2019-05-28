---
ms.assetid: a2f23877-30a7-439f-817d-387da9e00e86
title: Configurer un ordinateur pour le rôle de serveur proxy de fédération
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b01e2ae567155cd3d53d6d7972bfd0b9ec0cf51b
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192281"
---
# <a name="configure-a-computer-for-the-federation-server-proxy-role"></a>Configurer un ordinateur pour le rôle de serveur proxy de fédération

Après avoir configuré un ordinateur avec les certificats requis et que vous avez installé le service de rôle Proxy du Service de fédération, vous êtes prêt à configurer l’ordinateur pour qu’il devienne un serveur proxy de fédération. Vous pouvez utiliser la procédure suivante pour que l’ordinateur tienne le rôle de serveur proxy de fédération.  
  
> [!IMPORTANT]  
> Avant d’utiliser cette procédure pour configurer le serveur proxy de fédération, assurez-vous que vous avez suivi les étapes décrites dans [liste de vérification : Paramètre d’un serveur Proxy de fédération](Checklist--Setting-Up-a-Federation-Server-Proxy.md) dans l’ordre d’apparition. Veillez à ce qu’au moins un serveur de fédération soit déployé et que toutes les informations d’identification nécessaires pour autoriser la configuration d’un serveur proxy de fédération sont implémentées. Vous devez également configurer le protocole SSL (Secure Sockets Layer) \(SSL\) liaisons sur le Site Web par défaut, ou que cet Assistant ne démarrera pas. Toutes ces tâches doivent être exécutées avant que le serveur proxy de fédération ne puisse fonctionner.  
  
Quand vous avez fini de configurer l’ordinateur, vérifiez que le serveur proxy de fédération fonctionne comme prévu. Pour plus d'informations, voir [Vérifier qu'un serveur proxy de fédération est opérationnel](Verify-That-a-Federation-Server-Proxy-Is-Operational.md).  
  
Pour effectuer cette procédure, vous devez au minimum être membre du groupe **Administrateurs**ou d'un groupe équivalent sur l'ordinateur local.  Examinez les informations relatives à l’utilisation des comptes et des appartenances au groupe appropriés dans la rubrique [Groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477).   
  
### <a name="to-configure-a-computer-for-the-federation-server-proxy-role"></a>Pour configurer un ordinateur pour le rôle de serveur proxy de fédération  
  
1.  Il existe deux façons de démarrer l’Assistant Configuration du serveur de fédération AD FS. Pour démarrer l'Assistant, effectuez l'une des opérations suivantes :  
  
    -   Sur le **Démarrer** , tapez**Assistant Configuration Proxy du serveur de fédération AD FS**, puis appuyez sur ENTRÉE.  
  
    -   Une fois l’Assistant installation est terminée, ouvrez l’Explorateur Windows, accédez à la **C:\\Windows\\ADFS** dossier et puis double\-cliquez sur **FspConfigWizard.exe**.  
  
2.  Quelle que soit la méthode choisie, démarrez ensuite l'Assistant et, dans la page **Bienvenue**, cliquez sur **Suivant**.  
  
3.  Dans la page **Spécifier le nom du service FS**, sous **Nom du service de fédération**, tapez le nom désignant le service de fédération pour lequel cet ordinateur aura un rôle de proxy.  
  
4.  En fonction de la configuration requise pour votre réseau, déterminez si vous avez besoin d'un serveur proxy HTTP pour transférer les requêtes au service de fédération. Si c'est le cas, cochez la case **Utiliser un serveur proxy HTTP pour l'envoi de requêtes à ce service de fédération**, sous **Adresse du serveur proxy HTTP**, tapez l'adresse du serveur proxy, cliquez sur **Tester la connexion** pour vérifier la connectivité, puis cliquez sur **Suivant**.  
  
5.  Lorsque vous y êtes invité, entrez les informations d'identification nécessaires pour établir une relation d'approbation entre ce serveur proxy de fédération et le service de fédération.  
  
    Par défaut, uniquement le compte de service utilisé par le Service de fédération ou un membre de la local BUILTIN\\groupe Administrateurs peut autoriser un serveur proxy de fédération.  
  
6.  Dans la page **Prêt à appliquer les paramètres**, vérifiez les paramètres définis. Si les paramètres sont corrects, cliquez sur **Suivant** pour commencer à configurer l'ordinateur avec ces paramètres de proxy.  
  
7.  Dans la page **Résultats de la configuration**, examinez les résultats. Lorsque toutes les étapes de configuration sont terminées, cliquez sur **fermer** pour quitter l’Assistant.  
  
    Il n’existe aucune Console de gestion Microsoft \(MMC\) aligner\-in à utiliser pour administrer les proxys serveur de fédération. Pour configurer les paramètres pour chacun des proxys serveur de fédération dans votre organisation, utilisez les applets de commande Windows PowerShell.  
  
## <a name="configuring-an-alternate-tcpip-port-for-proxy-operations"></a>Configuration d’un autre TCP\/Port IP pour les opérations de Proxy  
Par défaut, le service de proxy de serveur de fédération est configuré pour utiliser le port TCP 443 pour le trafic HTTPS et le port 80 pour le trafic HTTP pour la communication avec le serveur de fédération. Pour configurer des ports différents, par exemple le port TCP 444 pour le trafic HTTPS et le port 81 pour le trafic HTTP, effectuez les tâches décrites ci-dessous.  
  
> [!NOTE]  
> Si vous projetez de déployer initialement AD FS pour fonctionner sous un autre TCP\/ports IP, vous devez tout d’abord modifier les ports dans vos liaisons de protocole IIS pour HTTP et HTTPS sur le serveur de fédération et les ordinateurs de proxy de serveur de fédération. Cela doit se produire avant d’exécuter les Assistants de configuration AD FS pour la configuration initiale. Si vous configurez Internet Information Services \(IIS\) tout d’abord, votre autre TCP\/les paramètres de port IP sont découverts lorsque l’Assistant\-en fonction de configuration se produit au sein d’AD FS, et la procédure suivante n’est pas nécessaire. Si vous voulez modifier les paramètres de port définis initialement, mettez d'abord à jour les liaisons de protocole IIS, puis suivez la procédure ci-dessous pour mettre à jour les paramètres de port de façon appropriée. Pour plus d’informations sur la modification des liaisons IIS, consultez [article 149605](https://go.microsoft.com/fwlink/?LinkId=190275) dans la Base de connaissances Microsoft.  
  
#### <a name="to-configure-alternate-tcpip-ports-for-the-federation-server-proxy-to-use"></a>Pour configurer TCP autre\/ports IP pour le serveur proxy de fédération à utiliser  
  
1.  Configurez le serveur de fédération pour qu'il utilise d'autres ports que ceux prévus par défaut.  
  
    Pour ce faire, spécifiez le numéro de port non défini par défaut en l’incluant avec le *HttpsPort* et *HttpPort* options dans le cadre de la **définir\-ADFSProperties** applet de commande. Par exemple, pour configurer ces ports, utilisez les commandes suivantes dans la session Windows PowerShell sur l’ordinateur de serveur de fédération :  
  
    ```  
    Set-ADFSProperties -HttpsPort 444  
    Set-ADFSProperties -HttpPort 81  
    ```  
  
2.  Configurer le serveur proxy de fédération pour utiliser le port non défini par défaut.  
  
    Pour ce faire, spécifiez le numéro de port non défini par défaut en l’incluant avec le *HttpsPort* et *HttpPort* options dans le cadre de la **définir\-ADFSProxyProperties** l’applet de commande. Par exemple, pour configurer ces ports, utilisez les commandes suivantes dans la session Windows PowerShell sur l’ordinateur de serveur de fédération :  
  
    ```  
    Set-ADFSProxyProperties -HttpsPort 444  
    Set-ADFSProxyProperties -HttpPort 81  
    ```  
  
    > [!NOTE]  
    > URL de point de terminaison ne sont pas activés par défaut pour le service de proxy de serveur de fédération. Si vous configurez une nouvelle installation de serveur de fédération, vous devez activer tout d’abord les points de terminaison service federation server proxy. Par exemple, il est supposé que pour tous les points de terminaison que l’exemple de cette procédure fait référence à vous avez activé pour le proxy en les sélectionnant dans le composant logiciel enfichable Gestion AD FS\-dans, puis en sélectionnant **activer sur le proxy**.  
  
3.  Mettre à jour de l’installation d’IIS sur le serveur proxy de fédération donc que Security Assertion Markup Language \(SAML\) et WS\-approuver les points de terminaison sont configurés pour refléter le numéro de port mis à jour. Pour ce faire, vous pouvez utiliser le bloc-notes pour modifier les éléments suivants dans le fichier Web.config, qui se trouve sous systemdrive %\\inetpub\\adfs\\ls\\ sur le serveur proxy de fédération. Par exemple, en supposant que vous avez un serveur de fédération nommé sts1.contoso.com et le nouveau numéro de port est 444, accédez à et ouvrez le fichier Web.config dans le bloc-notes sur le serveur proxy de fédération, recherchez la section suivante, modifiez le numéro de port en tant que mis en surbrillance ci-dessous, puis enregistrez et quittez le bloc-notes.  
  
    ```  
    <securityTokenService samlProtocolEndpoint="https://sts1.contoso.com:444/adfs/services/trust/samlprotocol/proxycertificatetransport"  
          wsTrustEndpoint="https://sts1.contoso.com:444/adfs/services/trust/proxycertificatetransport" />  
    ```  
  
4.  Ajouter le compte d’utilisateur de service de proxy de fédération serveur à la liste de contrôle d’accès \(ACL\) pour les URL de point de terminaison associées. Par exemple, si le numéro de port est 1234 et le compte d’utilisateur qui est utilisé pour exécuter le AD FSfederation serveur proxy de service sous est intégrée\-dans le compte de Service réseau, tapez la commande suivante à une invite de commandes :  
  
    ```  
    netsh http add urlacl https://+:444/adfs/fs/federationserverservice.asmx/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/FederationMetadata/2007-06/ user="NT Authority\Network Service"  
    netsh http add urlacl https://+:444/adfs/services/ user="NT Authority\Network Service"  
  
    netsh http add urlacl http://+:81/adfs/services/ user="NT Authority\Network Service"  
    ```  
  
    Les commandes précédentes doivent être exécutées sur le serveur de fédération et les ordinateurs de proxy de serveur de fédération.  
  
## <a name="additional-references"></a>Références supplémentaires  
[Liste de vérification : configuration d’un serveur de fédération proxy](Checklist--Setting-Up-a-Federation-Server-Proxy.md)  
  

