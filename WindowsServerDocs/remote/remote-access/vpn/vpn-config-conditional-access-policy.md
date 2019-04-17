---
title: Configurer la stratégie d'accès conditionnel
description: Après avoir créé un certificat racine, la 'connectivité VPN' déclenche la création de l’application de cloud «Serveur VPN» dans le locataire du client.
services: active-directory
ms.prod: windows-server-threshold
ms.technology: networking-ras
documentationcenter: ''
ms.assetid: ''
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.reviewer: deverette
ms.openlocfilehash: 8c00855c50de79efa1b48c7b8762e1b679db4a87
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067116"
---
# Étape7.3. Configurer la stratégie d’accès conditionnel

>S’applique à: Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows 10

& #171;  [ **Précédente:** étape 7.2. Créer des certificats racine pour l’authentification VPN avec Azure AD](vpn-create-root-cert-for-vpn-auth-azure-ad.md)<br>
& #187; [ **Suivant:** étape 7.4. Déployer des certificats racine de l’accès conditionnel sur site AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md)

Dans cette étape, vous configurez la stratégie d’accès conditionnel pour une connexion VPN. Lorsque le premier certificat racine est créé dans le panneau de «Connectivité VPN», il crée automatiquement une application en nuage «Serveur VPN» dans le client. 

Créer une stratégie d’accès conditionnel qui est attribuée à un groupe d’utilisateurs VPN et l’étendue de l' **application de Cloud** pour le **Serveur VPN**: 

- **Les utilisateurs**: les utilisateurs VPN
- **Application cloud**: serveur VPN
- **Grant (contrôle d’accès)**: «Nécessitent une authentification multifacteur». Autres contrôles peuvent être utilisés si vous le souhaitez.

**Procédure:** Cette étape couvre la création de la stratégie d’accès conditionnel plus simple.Si vous le souhaitez, des contrôles et des Conditions supplémentaires peuvent être utilisés.


1. Sur la page de **L’accès conditionnel** , dans la barre d’outils dans la partie supérieure, cliquez sur **Ajouter**.

    ![Sélectionnez Ajouter sur la page de l’accès conditionnel](../../media/Always-On-Vpn/07.png)

2. Sur la page de **Nouveau** , dans la zone **nom** , tapez un nom pour votre stratégie. Par exemple, tapez **stratégie VPN**.

    ![Ajoutez le nom de la stratégie sur la page de l’accès conditionnel](../../media/Always-On-Vpn/08.png)

3. Dans la section **attribution** , cliquez sur **les utilisateurs et groupes**.

    ![Sélectionnez Utilisateurs et groupes](../../media/Always-On-Vpn/09.png)

4. Sur la page **utilisateurs et groupes** , procédez comme suit:

    ![Sélectionnez tester un utilisateur](../../media/Always-On-Vpn/10.png)

    a. Cliquez sur **Sélectionner des utilisateurs et groupes**.

    b. Cliquez sur **Sélectionner**.

    c. Sur la page **Sélectionner** , sélectionnez le groupe **d’utilisateurs VPN** , puis cliquez sur **Sélectionner**.

    d. Sur la page **utilisateurs et groupes** , cliquez sur **terminé**.

5. Sur la page de **Nouveau** , procédez comme suit:

    ![Sélectionnez les applications dans le cloud](../../media/Always-On-Vpn/11.png)

    a. Dans la section **affectations** , cliquez sur **applications dans le Cloud**.

    b. Sur la page **applications dans le Cloud** , cliquez sur **Sélectionner des applications**.

    d. Sélectionnez le **serveur VPN**.

13. Sur la page de **Nouveau** , pour ouvrir la page **Grant** , dans la section **contrôles** , cliquez sur **octroyer**.

    ![Sélectionnez la licence](../../media/Always-On-Vpn/13.png)

14. Sur la page **Grant** , procédez comme suit:

    ![Sélectionnez exiger une authentification multifacteur](../../media/Always-On-Vpn/14.png)

    a. Sélectionnez **Exiger une authentification multifacteur**.

    b. Cliquez sur **Sélectionner**.

15. Sur la page de **Nouveau** , sous la **stratégie est activée**, cliquez **sur**.

    ![Activer la stratégie](../../media/Always-On-Vpn/15.png)

16. Sur la page de **Nouveau** , cliquez sur **créer**.


## Étape suivante
Étape [7.4. Déployer des certificats racine de l’accès conditionnel sur site AD](vpn-deploy-cond-access-root-cert-to-on-premise-ad.md): dans cette étape, vous déployez le certificat racine de l’accès conditionnel comme certificat racine de confiance pour l’authentification VPN vers votre site sur AD.

---