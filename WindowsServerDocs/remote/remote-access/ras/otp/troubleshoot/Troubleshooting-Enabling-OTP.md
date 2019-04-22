---
title: Résolution des problèmes d’activation du mot de passe à usage unique
description: Cette rubrique fait partie du guide de déploiement des accès à distance avec authentification OTP dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b58252ca-4c1d-4664-a3c4-7301e2121517
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d789671a0425974e560e5f4a43ebcba4c1741c76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819460"
---
# <a name="troubleshooting-enabling-otp"></a>Résolution des problèmes d’activation du mot de passe à usage unique

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique contient des informations de dépannage des problèmes liés à l’activation de l’authentification DirectAccess OTP à l’aide du **Enable-DAOtpAuthentication** applet de commande PowerShell ou la console de gestion de l’accès à distance.
  
## <a name="failed-to-enroll-the-otp-signing-certificate"></a>Impossible d’inscrire le certificat de signature OTP  
**Erreur reçue** (journal des événements serveur). Un certificat de signature du secret à usage unique ne peut pas être inscrite à l’aide du modèle de certificat < OTP_signing_template_name >  
  
**Cause**  
  
Il existe trois causes possibles de cette erreur :  
  
-   Le modèle n’existe pas.  
  
-   Les autorisations définies sur le modèle n’autorisent pas le serveur DirectAccess à inscrire.  
  
-   Il n’existe aucune connectivité réseau à l’autorité de certification (CA).  
  
**Solution**  
  
1.  Assurez-vous que la signature du secret à usage unique modèle de certificat avec le nom donné :  
  
    1.  Existe et dispose des autorisations appropriées.  
  
    2.  A la valeur doit être émis par au moins une autorité de certification qui émettre des certificats pour le serveur DirectAccess.  
  
2.  Si le modèle n’existe, créez-la comme décrit dans le Plan 3.3 le certificat d’autorité d’inscription ou si un autre modèle correspondant existe reconfigurer DirectAccess OTP avec le nouveau nom de modèle.  
  
## <a name="failed-to-enable-directaccess-otp-when-webdav-is-installed"></a>Échec d’activation de DirectAccess OTP lorsque WebDAV est installé.  
**Scénario**. Lorsque vous tentez d’appliquer la configuration DirectAccess OTP dans la console de gestion de l’accès à distance ou en utilisant le `Enable-DAOtpAuthentication` applet de commande PowerShell, l’opération échoue.  
  
**Erreur reçue** (journal des événements serveur). Impossible d’appliquer les paramètres DirectAccess OTP, car l’extension WebDAV pour IIS s’exécute sur le serveur. Supprimer WebDAV et appliquer les paramètres à nouveau.  
  
**Cause**  
  
Le service DirectAccess OTP est incompatible avec la fonctionnalité de publication WebDAV et ne peut pas être activé pendant l’installation de WebDAV.  
  
**Solution**  
  
Désinstaller le rôle de WebDAV :  
  
1.  Dans la console Gestionnaire de serveur, dans le volet gauche, cliquez sur **IIS**.  
  
2.  Dans le volet principal, accédez à **rôles et fonctionnalités**.  
  
3.  Avec le bouton droit **publication WebDAV**, puis cliquez sur **supprimer un rôle ou fonctionnalité**.  
  
4.  Terminez l’Assistant de fonctionnalités et de suppression de rôles.  
  
5.  Ré-appliquer la configuration DirectAccess OTP.  
  
## <a name="no-templates-available-in-the-remote-access-management-console"></a>Aucun modèle disponible dans la console de gestion de l’accès à distance  
**Scénario**. Bien que configuration OTP ou inscription modèles de certificats autorité à l’aide de la console de gestion de l’accès à distance, certains ou tous les modèles sont manquants dans les fenêtres de sélection.  
  
**Cause**  
  
Il existe deux causes possibles de cette erreur :  
  
-   Le modèle n’est pas configuré conformément aux exigences de DirectAccess OTP et par conséquent, il ne peut pas être sélectionné.  
  
-   Les autorités de certification sélectionnée sous **serveurs d’autorité de certification OTP** ne sont pas configurés pour émettre les modèles requis.  
  
**Solution**  
  
1.  Assurez-vous que le modèle d’ouverture de session OTP et le modèle de certificat de signature OTP sont configurés correctement, comme décrit dans le modèle de certificat OTP 3.3 Plan et le certificat d’autorité d’inscription de Plan 3,2.  
  
2.  Assurez-vous que les autorités de certification configurée dans le **serveurs d’autorité de certification OTP** liste sont configurés pour les problèmes, les modèles pertinents :  
  
    1.  Sur le serveur d’autorité de certification, ouvrez la console Autorité de Certification.  
  
    2.  Dans le volet gauche, développez le serveur d’autorité de certification choisi.  
  
    3.  Cliquez sur **modèles de certificats** et assurez-vous que les modèles requis sont activés. Avec le bouton droit dans le cas contraire, **modèles de certificats**, cliquez sur **New**, cliquez sur **modèle de certificat à délivrer**, puis sélectionnez les modèles que vous souhaitez activer.  
  
## <a name="cannot-set-renewal-period-of-otp-template-to-1-hour"></a>Impossible de définir période de renouvellement du modèle de secret à usage unique et 1 heure  
**Scénario**. Lorsque vous configurez le modèle de connexion DirectAccess OTP à l’aide d’autorité de certification Windows 2003, il n’est pas possible de définir la période de renouvellement du modèle à 1 heure.  
  
**Cause**  
  
Le composant logiciel enfichable MMC Modèles de certificats dans Windows Server 2003 ne vous permet de définir la période de renouvellement d’un modèle à 1 heure.  
  
**Solution**  
  
Installer le composant logiciel enfichable modèles de certificats sur un serveur de Server 2003 postérieur à Windows et l’utiliser pour configurer le modèle d’ouverture de session OTP, consultez [installer le composant logiciel enfichable modèles de certificat](https://technet.microsoft.com/library/cc732445.aspx).  
  


