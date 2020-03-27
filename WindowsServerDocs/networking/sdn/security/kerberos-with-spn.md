---
title: Kerberos avec un nom principal de service (SPN)
description: Le contrôleur de réseau prend en charge plusieurs méthodes d’authentification pour la communication avec les clients de gestion. Vous pouvez utiliser l’authentification basée sur Kerberos, l’authentification basée sur un certificat x509. Vous avez également la possibilité d’utiliser aucune authentification pour les déploiements de test.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: lizross
author: eross-msft
ms.date: 08/23/2018
ms.openlocfilehash: adf282222674130dcb16b0c7bfe0cf3ff05ed720
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317393"
---
# <a name="kerberos-with-service-principal-name-spn"></a>Kerberos avec un nom principal de service (SPN)

>S’applique à : Windows Server 2019

Le contrôleur de réseau prend en charge plusieurs méthodes d’authentification pour la communication avec les clients de gestion. Vous pouvez utiliser l’authentification basée sur Kerberos, l’authentification basée sur un certificat x509. Vous avez également la possibilité d’utiliser aucune authentification pour les déploiements de test.

System Center Virtual Machine Manager utilise l’authentification basée sur Kerberos. Si vous utilisez l’authentification Kerberos, vous devez configurer un nom de principal du service (SPN) pour le contrôleur de réseau dans Active Directory. Le SPN est un identificateur unique pour l’instance de service de contrôleur de réseau, qui est utilisé par l’authentification Kerberos pour associer une instance de service à un compte de connexion de service. Pour plus d’informations, consultez [noms de principal du service](https://docs.microsoft.com/windows/desktop/ad/service-principal-names).

## <a name="configure-service-principal-names-spn"></a>Configurer les noms de principal du service (SPN)

Le contrôleur de réseau configure automatiquement le SPN. Il vous suffit de fournir des autorisations pour que les ordinateurs du contrôleur de réseau inscrivent et modifient le SPN.

1.  Sur l’ordinateur contrôleur de domaine, démarrez **Active Directory utilisateurs et ordinateurs**.

2.  Sélectionnez **afficher \> avancé**.

3.  Sous **ordinateurs**, localisez l’un des comptes d’ordinateur du contrôleur de réseau, puis cliquez avec le bouton droit et sélectionnez **Propriétés**.

4.  Sélectionnez l’onglet **sécurité** , puis cliquez sur **avancé**.

5.  Dans la liste, si tous les comptes d’ordinateur du contrôleur de réseau ou un groupe de sécurité disposant de tous les comptes d’ordinateur du contrôleur de réseau ne sont pas répertoriés, cliquez sur **Ajouter** pour l’ajouter.

6.  Pour chaque compte d’ordinateur du contrôleur de réseau ou un groupe de sécurité unique contenant les comptes d’ordinateur du contrôleur de réseau :

    a.  Sélectionnez le compte ou le groupe, puis cliquez sur **modifier**.

    b.  Sous autorisations, sélectionnez **valider l’écriture servicePrincipalName**.

    d.  Faites défiler la liste et sous **Propriétés** , sélectionnez :

       -  **Read servicePrincipalName**

       -  **ServicePrincipalName d’écriture**

    e.  Cliquez deux fois sur **OK**.

7.  Répétez l’étape 3-6 pour chaque ordinateur du contrôleur de réseau.

8.  Fermez **Utilisateurs et ordinateurs Active Directory**.

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>Impossible de fournir des autorisations pour l’inscription/la modification du SPN

Dans un **nouveau** déploiement de Windows Server 2019, si vous avez choisi Kerberos pour l’authentification du client REST et n’accordez pas d’autorisation pour que les nœuds du contrôleur de réseau inscrivent ou modifient le SPN, les opérations Rest sur le contrôleur de réseau échouent et vous empêche de gérer le Sdn.

Pour une mise à niveau de Windows Server 2016 vers Windows Server 2019, et que vous avez choisi Kerberos pour l’authentification du client REST, les opérations REST ne sont pas bloquées, garantissant ainsi la transparence pour les déploiements de production existants. 

Si le SPN n’est pas inscrit, l’authentification du client REST utilise NTLM, ce qui est moins sécurisé. Vous recevez également un événement critique dans le canal d’administration de **NetworkController-Framework** Event Channel, qui vous demande de fournir des autorisations aux nœuds du contrôleur de réseau pour inscrire le SPN. Une fois que vous avez autorisé, le contrôleur de réseau inscrit le SPN automatiquement, et toutes les opérations du client utilisent Kerberos.


>[!TIP]
>En règle générale, vous pouvez configurer le contrôleur de réseau pour utiliser une adresse IP ou un nom DNS pour les opérations REST. Toutefois, lorsque vous configurez Kerberos, vous ne pouvez pas utiliser une adresse IP pour les requêtes REST sur le contrôleur de réseau. Par exemple, vous pouvez utiliser \<https://networkcontroller.consotso.com\>, mais vous ne pouvez pas utiliser \<https://192.34.21.3\>. Les noms de principal du service ne peuvent pas fonctionner si des adresses IP sont utilisées.
>
>Si vous utilisiez l’adresse IP pour les opérations REST avec l’authentification Kerberos dans Windows Server 2016, la communication réelle aurait été dépassée par l’authentification NTLM. Dans ce type de déploiement, une fois que vous avez mis à niveau vers Windows Server 2019, vous continuez à utiliser l’authentification NTLM. Pour passer à l’authentification Kerberos, vous devez utiliser le nom DNS du contrôleur de réseau pour les opérations REST et autoriser les nœuds du contrôleur de réseau à inscrire le SPN.

---