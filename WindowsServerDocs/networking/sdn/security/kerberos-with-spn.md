---
title: Kerberos avec un nom principal de service (SPN)
description: Contrôleur de réseau prend en charge plusieurs méthodes d’authentification pour la communication avec les clients de gestion. Vous pouvez utiliser l’authentification Kerberos en fonction, X509 authentification par certificat. Vous avez également la possibilité de n’utiliser aucune authentification pour les déploiements de test.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 2459cfa8dfec3de4aa23da7aba192d6eeed7f8ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59828970"
---
# <a name="kerberos-with-service-principal-name-spn"></a>Kerberos avec un nom principal de service (SPN)

>S’applique à : Windows Server 2019

Contrôleur de réseau prend en charge plusieurs méthodes d’authentification pour la communication avec les clients de gestion. Vous pouvez utiliser l’authentification Kerberos en fonction, X509 authentification par certificat. Vous avez également la possibilité de n’utiliser aucune authentification pour les déploiements de test.

System Center Virtual Machine Manager utilise l’authentification Kerberos. Si vous utilisez l’authentification Kerberos, vous devez configurer un nom de Principal du Service (SPN) pour le contrôleur de réseau dans Active Directory. Le SPN est un identificateur unique pour l’instance de service de contrôleur de réseau, qui est utilisé par l’authentification Kerberos pour associer une instance de service à un compte de connexion de service. Pour plus d’informations, consultez [noms principaux de Service](https://docs.microsoft.com/windows/desktop/ad/service-principal-names).

## <a name="configure-service-principal-names-spn"></a>Configurer des noms de principal du Service (SPN)

Le contrôleur de réseau configure automatiquement le SPN. Il vous suffit de faire consiste à fournir des autorisations pour les machines de contrôleur de réseau pour vous inscrire et de modifier le SPN.

1.  Sur l’ordinateur de contrôleur de domaine, démarrez **Active Directory Users and Computers**.

2.  Sélectionnez **vue \> Advanced**.

3.  Sous **ordinateurs**, recherchez un des comptes d’ordinateur de contrôleur de réseau, puis avec le bouton droit et sélectionnez **propriétés**.

4.  Sélectionnez le **sécurité** onglet et cliquez sur **avancé**.

5.  Dans la liste, si tous les comptes d’ordinateur de contrôleur de réseau ou d’une sécurité de groupe ayant tous les comptes d’ordinateur de contrôleur de réseau n’est pas répertorié, cliquez sur **ajouter** pour l’ajouter.

6.  Pour chaque compte d’ordinateur de contrôleur de réseau ou un seul groupe de sécurité contenant les comptes d’ordinateur de contrôleur de réseau :

    a.  Sélectionnez le compte ou le groupe et cliquez sur **modifier**.

    b.  Sous, sélectionnez autorisations **servicePrincipalName valider écrire**.

    d.  Faites défiler et sous **propriétés** sélectionnez :

       -  **Lire servicePrincipalName**

       -  **Écrire servicePrincipalName**

    e.  Cliquez deux fois sur **OK**.

7.  Répétez l’étape 3 à 6 pour les ordinateurs de chaque contrôleur de réseau.

8.  Fermez **Utilisateurs et ordinateurs Active Directory**.

## <a name="failure-to-provide-permissions-for-spn-registrationmodification"></a>Incapacité à fournir des autorisations de modification / d’inscription SPN

Sur un **NEW** échoue de déploiement de Windows Server 2019, si vous avez choisi de Kerberos pour l’authentification du client REST et n’accordez pas d’autorisation pour les nœuds de contrôleur de réseau inscrire ou de modifier le nom principal de service, les opérations REST sur le contrôleur de réseau en vous empêchant de gérer le SDN.

Pour une mise à niveau à partir de Windows Server 2016 pour Windows Server 2019 et que vous avez choisi Kerberos pour l’authentification du client REST, les opérations REST ne seront pas bloquées, assurer la transparence pour les déploiements de production existant. 

Si le SPN n’est pas inscrit, l’authentification du client REST utilise NTLM, qui est moins sécurisé. Vous obtenez également un événement critique dans le canal d’administration de **NetworkController-Framework** canal d’événement vous demandant de fournir des autorisations sur les nœuds de contrôleur de réseau pour inscrire le SPN. Une fois que vous fournissez d’autorisation, contrôleur de réseau s’inscrit automatiquement le SPN, et toutes les opérations de client utilisent Kerberos.


>[!TIP]
>En règle générale, vous pouvez configurer de contrôleur de réseau pour utiliser une adresse IP ou le nom DNS pour les opérations basées sur REST. Toutefois, lorsque vous configurez Kerberos, vous ne pouvez pas utiliser une adresse IP pour les requêtes REST au contrôleur de réseau. Par exemple, vous pouvez utiliser \< https://networkcontroller.consotso.com\>, mais vous ne pouvez pas utiliser \< https://192.34.21.3\>. Noms principaux de service ne peut pas fonctionner si les adresses IP sont utilisées.
>
>Si vous utilisiez une adresse IP pour les opérations REST, ainsi que l’authentification Kerberos dans Windows Server 2016, la communication réelle aurait été via l’authentification NTLM. Dans un tel déploiement, une fois que vous mettez à niveau vers Windows Server 2019, vous continuez à utiliser l’authentification NTLM. Pour déplacer vers l’authentification Kerberos, vous devez utiliser nom DNS de contrôleur de réseau pour les opérations REST et fournir une autorisation pour les nœuds de contrôleur de réseau inscrire le SPN.

---