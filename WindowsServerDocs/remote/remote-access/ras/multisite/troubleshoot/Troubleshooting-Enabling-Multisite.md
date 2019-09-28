---
title: Résolution des problèmes d’activation du déploiement multisite
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 570c81d6-c4f4-464c-bee9-0acbd4993584
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fc42040d68b8a22dcfc46aa30db3a2a3c3bc060a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367062"
---
# <a name="troubleshooting-enabling-multisite"></a>Résolution des problèmes d’activation du déploiement multisite

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique contient des informations de résolution des problèmes liés à la commande `Enable-DAMultisite`. Pour confirmer que l’erreur que vous avez reçue est liée à l’activation du déploiement multisite, recherchez l’ID d’événement 10051 dans le journal des événements Windows.  
  
## <a name="user-connectivity-issues"></a>Problèmes de connectivité des utilisateurs  
Les utilisateurs peuvent rencontrer des problèmes de connectivité lorsque vous activez le déploiement multisite si la configuration n’est pas correcte.  
  
**Cause**  
  
Dans un déploiement multisite, les ordinateurs clients Windows 10 et Windows 8 sont en mesure de se déplacer entre différents points d’entrée.  Les ordinateurs clients Windows 7 doivent être associés à un point d’entrée spécifique dans le déploiement multisite. Si les ordinateurs clients ne sont pas dans le groupe de sécurité approprié, ils peuvent recevoir les mauvais paramètres de stratégie de groupe.  
  
**Solution**  
  
DirectAccess requiert au moins un groupe de sécurité pour tous les ordinateurs clients Windows 10 et Windows 8 ; Nous vous recommandons d’utiliser un groupe de sécurité pour tous les ordinateurs Windows 10 et Windows 8 par domaine. DirectAccess requiert également un groupe de sécurité pour les ordinateurs clients Windows 7 pour chaque point d’entrée. Chaque ordinateur client doit être inclus dans un seul groupe de sécurité. Par conséquent, vous devez vous assurer que les groupes de sécurité pour les clients Windows 10 et Windows 8 contiennent uniquement des ordinateurs exécutant Windows 10 ou Windows 8, et que chaque ordinateur client Windows 7 appartient à un seul groupe de sécurité dédié pour le point d’entrée concerné et qu’aucun client Windows 10 ou Windows 8 n’appartient aux groupes de sécurité Windows 7.  
  
Configurez les groupes de sécurité Windows 8 dans la page **Sélectionner des groupes** de l’Assistant **installation du client DirectAccess** . Configurez les groupes de sécurité Windows 7 sur la page **prise en charge des clients** de l’Assistant **activer le déploiement multisite** ou sur la page **prise en charge des clients** de l’Assistant **Ajouter un point d’entrée** .  
  
## <a name="kerberos-proxy-authentication"></a>Authentification du proxy Kerberos  
**Erreur reçue**. L’authentification du proxy Kerberos n’est pas prise en charge dans un déploiement multisite. Vous devez activer l’utilisation de certificats d’ordinateur pour l’authentification d’utilisateurs IPsec.  
  
**Cause**  
  
L’authentification du certificat d’ordinateur doit être activée avant d’activer le déploiement multisite.  
  
**Solution**  
  
Pour activer l’authentification du certificat d’ordinateur :  
  
1.  Dans la console Gestion de l’accès à distance, dans le volet d’informations, sous **Étape 2 : Serveur d’accès à distance**, cliquez sur **Modifier**.  
  
2.  Dans l’Assistant **Installation du serveur d’accès à distance**, depuis la page **Authentification**, activez la case à cocher **Utiliser les certificats d’ordinateur**, puis sélectionnez l’autorité de certification racine ou intermédiaire qui émet les certificats dans votre déploiement.  
  
Pour activer l’authentification par certificat d’ordinateur à l’aide de Windows PowerShell, utilisez l’applet de commande `Set-DAServer` et spécifiez le paramètre *IPsecRootCertificate* .  
  
## <a name="ip-https-certificates"></a>Certificats IP-HTTPS  
**Erreur reçue**. Le serveur DirectAccess utilise un certificat IP-HTTPs auto-signé. Configurez IP-HTTPS de manière à utiliser un certificat signé d’une autorité de certification connue.  
  
**Cause**  
  
Le certificat IP-HTTPS est auto-signé. Vous ne pouvez pas utiliser des certificats auto-signés dans un déploiement multisite.  
  
**Solution**  
  
Pour sélectionner un certificat IP-HTTPS :  
  
1.  Dans la console Gestion de l’accès à distance, dans le volet d’informations, sous **Étape 2 : Serveur d’accès à distance**, cliquez sur **Modifier**.  
  
2.  Dans l’Assistant **Installation du serveur d’accès à distance**, depuis la page **Cartes réseau**, sous **Sélectionnez le certificat utilisé pour authentifier les connexions IP-HTTPS**, vérifiez que la case à cocher **Utiliser un certificat auto-signé créé automatiquement par DirectAccess** est désactivée, puis cliquez sur **Parcourir** pour sélectionner un certificat émis par une autorité de certification approuvée.  
  
## <a name="network-location-server"></a>Serveur Emplacement réseau  
  
-   **Problème 1**  
  
    **Erreur reçue**. DirectAccess est configuré pour utiliser un certificat auto-signé pour le serveur emplacement réseau. Configurez le serveur Emplacement réseau pour qu’il utilise un certificat signé d’une autorité de certification.  
  
    **Cause**  
  
    Le serveur Emplacement réseau est déployé sur le serveur d’accès à distance et il utilise un certificat auto-signé. Vous ne pouvez pas utiliser des certificats auto-signés dans un déploiement multisite.  
  
    **Solution**  
  
    Pour sélectionner un certificat de serveur Emplacement réseau :  
  
    1.  Dans la console Gestion de l’accès à distance, dans le volet d’informations, sous **Étape 3 : Serveurs d’infrastructure**, cliquez sur **Modifier**.  
  
    2.  Dans l’Assistant **Installation du serveur d’infrastructure**, depuis la page **Serveur Emplacement réseau**, sous **Le serveur Emplacement réseau est déployé sur le serveur d’accès à distance**, vérifiez que la case à cocher **Utiliser un certificat auto-signé** est désactivée, puis cliquez sur **Parcourir** pour sélectionner un certificat émis par une autorité de certification d’entreprise.  
  
-   **Problème 2**  
  
    **Erreur reçue**. Pour déployer un cluster à charge réseau équilibrée ou un déploiement multisite, obtenez un certificat pour le serveur emplacement réseau avec un nom de sujet différent du nom interne du serveur d’accès à distance.  
  
    **Cause**  
  
    Le nom du sujet du certificat utilisé pour le site Web du serveur Emplacement réseau est identique au nom interne du serveur d’accès à distance. Cela entraîne des problèmes de résolution de noms.  
  
    **Solution**  
  
    Obtenez un certificat dont le nom du sujet n’est pas le même que le nom interne du serveur d’accès à distance.  
  
    Pour configurer le serveur Emplacement réseau :  
  
    1.  Dans la console Gestion de l’accès à distance, dans le volet d’informations, sous **Étape 3 : Serveurs d’infrastructure**, cliquez sur **Modifier**.  
  
    2.  Dans l’Assistant **Installation du serveur d’infrastructure**, depuis la page **Serveur Emplacement réseau**, sous **Le serveur Emplacement réseau est déployé sur le serveur d’accès à distance**, cliquez sur **Parcourir** pour sélectionner le certificat que vous avez préalablement obtenu. Le certificat doit porter un nom du sujet différent du nom interne du serveur d’accès à distance.  
  
## <a name="windows-7-client-computers"></a>Ordinateurs clients Windows 7  
**Avertissement reçu**. Lorsque vous activez multisite, les groupes de sécurité configurés pour les clients DirectAccess ne doivent pas contenir d’ordinateurs Windows 7. Pour prendre en charge les ordinateurs clients Windows 7 dans un déploiement multisite, sélectionnez un groupe de sécurité contenant les clients pour chaque point d’entrée.  
  
**Cause**  
  
Dans le déploiement DirectAccess existant, la prise en charge du client Windows 7 a été activée.  
  
**Solution**  
  
DirectAccess requiert au moins un groupe de sécurité pour tous les ordinateurs clients Windows 8 et un groupe de sécurité pour les ordinateurs clients Windows 7 pour chaque point d’entrée. Chaque ordinateur client doit être inclus dans un seul groupe de sécurité. Par conséquent, vous devez vous assurer que le groupe de sécurité pour les clients Windows 8 contient uniquement des ordinateurs exécutant Windows 8 et que chaque ordinateur client Windows 7 appartient à un seul groupe de sécurité dédié pour le point d’entrée concerné et qu’aucun client Windows 8 n’est présent. appartiennent aux groupes de sécurité Windows 7.  
  
## <a name="active-directory-site"></a>Site Active Directory  
**Erreur reçue**. Le serveur < nom_serveur > n’est pas associé à un site Active Directory.  
  
**Cause**  
  
DirectAccess n’a pas pu déterminer le site Active Directory. Dans la console Sites et services Active Directory, vous pouvez configurer les différents sous-réseaux de votre réseau et associer chacun d’eux au site Active Directory approprié. Cette erreur peut se produire si l’adresse IP du serveur d’accès à distance n’appartient pas à l’un des sous-réseaux ou si le sous-réseau auquel elle appartient n’est pas défini avec un site Active Directory.  
  
**Solution**  
  
Confirmez que ce site est la source du problème en exécutant la commande `nltest /dsgetsite` sur votre serveur d’accès à distance. Si tel est le cas, la commande renvoie ERROR_NO_SITENAME. Pour remédier à ce problème, sur votre contrôleur de domaine, vérifiez qu’il existe un sous-réseau contenant l’adresse IP du serveur interne et qu’il est défini avec un site Active Directory.  
  
## <a name="SaveGPOSettings"></a>Enregistrement des paramètres d’objet de stratégie de groupe de serveur  
**Erreur reçue**. Une erreur s’est produite lors de l’enregistrement des paramètres d’accès à distance dans l’objet de stratégie de groupe < GPO_name >.  
  
**Cause**  
  
Les modifications apportées à l’objet de stratégie de groupe du serveur n’ont pas pu être enregistrées en raison de problèmes de connectivité ou en cas de violation de partage sur le fichier Registry. pol, par exemple, un autre utilisateur a verrouillé le fichier.  
  
**Solution**  
  
Assurez-vous que la connectivité est établie entre le serveur d’accès à distance et le contrôleur de domaine. En cas de connectivité, vérifiez sur le contrôleur de domaine si le fichier registry.pol est verrouillé par un autre utilisateur et si besoin, mettez fin à la session de cet utilisateur pour déverrouiller le fichier.  
  
## <a name="InternalServerError"></a>Une erreur interne s’est produite  
**Erreur reçue**. Une erreur interne s'est produite.  
  
**Cause**  
  
Cela peut être provoqué par une configuration inattendue de la table de points d’entrée dans l’objet de stratégie de groupe client. Cela peut se produire si l’administrateur utilise des applets de commande de clients DirectAccess pour modifier la table de points d’entrée dans l’objet de stratégie de groupe client.  
  
**Solution**  
  
Examinez la configuration de la table de points d’entrée dans tous les objets de stratégie de groupe clients et corrigez toutes les incohérences dans la configuration multisite entre les différentes instances des objets de stratégie de groupe clients et la configuration DirectAccess. Utilisez l’applet de commande `Get-DaEntryPointTableItem` avec le nom de l’objet de stratégie de groupe client pour obtenir la table de points d’entrée sur le client. Utilisez l’applet de commande `Get-NetIPHttpsConfiguration` pour obtenir tous les profils IP-HTTPS de tous les points d’entrée.  
  
Pour plus d’informations, voir [Applets de commandes des clients DirectAccess dans Windows PowerShell](https://technet.microsoft.com/library/hh848426).  
  


