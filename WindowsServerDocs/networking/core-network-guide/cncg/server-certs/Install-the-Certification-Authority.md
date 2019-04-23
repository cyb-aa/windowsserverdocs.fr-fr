---
title: Installer l’autorité de certification
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 84e4b2fe0b59820b9e51229335f3539bcbeeec90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860740"
---
# <a name="install-the-certification-authority"></a>Installer l’autorité de certification

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour installer les Services de certificats Active Directory (AD CS) afin que vous pouvez inscrire un certificat de serveur pour les serveurs qui exécutent le serveur NPS (Network Policy Server), routage et accès distant (RRAS) ou les deux.  
  
> [!IMPORTANT]  
> -   Avant d’installer des Services de certificats Active Directory, vous devez nommer l’ordinateur, configurez l’ordinateur avec une adresse IP statique et joindre l’ordinateur au domaine. Pour plus d’informations sur la façon d’accomplir ces tâches, consultez Windows Server 2016 [Guide du réseau](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).  
> -   Pour effectuer cette procédure, l’ordinateur sur lequel vous installez les services AD CS doit être joint à un domaine où les Services de domaine Active Directory (AD DS) est installé.  
  
Pour être autorisé à effectuer cette procédure, vous devez faire partie du groupe **Administrateurs de l'entreprise** et du groupe **Admins du domaine** du domaine racine.  
  
> [!NOTE]  
> Pour effectuer cette procédure à l’aide de Windows PowerShell, ouvrez Windows PowerShell et tapez la commande suivante, puis appuyez sur ENTRÉE.   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> Une fois les services AD CS est installé, tapez la commande suivante et appuyez sur ENTRÉE.  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>Pour installer les services de certificats Active Directory  

>[!TIP]
>Si vous souhaitez utiliser Windows PowerShell pour installer les Services de certificats Active Directory, consultez [Install-AdcsCertificationAuthority](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) pour les applets de commande et les paramètres facultatifs.
  
1.  Ouvrez une session en tant que membre du groupe Administrateurs de l'entreprise et du groupe Admins du domaine du domaine racine.  
  
2.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.  
  
3.  Dans **Avant de commencer**, cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > La page **Avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez préalablement sélectionné **Ignorer cette page par défaut** dans l’Assistant.  
  
4.  Dans **Sélectionner le type d’installation**, vérifiez que **Installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **Suivant**.  
  
5.  Dans **Sélectionner le serveur de destination**, vérifiez que **Sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **Suivant**.  
  
6.  Dans **sélectionner des rôles de serveur**, dans **rôles**, sélectionnez **Active Directory Certificate Services**. Lorsque vous êtes invité à ajouter les fonctionnalités requises, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
7.  Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**.  
  
8.  Dans **Active Directory Certificate Services**, lisez les informations fournies, puis cliquez sur **suivant**.  
  
9. Dans **Confirmer les sélections pour l’installation**, cliquez sur **Installer**. Ne fermez pas l’Assistant pendant le processus d’installation. Lors de l’installation est terminée, cliquez sur **configurer Active Directory Certificate Services sur le serveur de destination**. L’Assistant de Configuration AD CS s’ouvre. Lire les informations d’identification et, si nécessaire, fournissez les informations d’identification d’un compte qui est membre du groupe Administrateurs de l’entreprise. Cliquez sur **Suivant**.  
  
10. Dans **Services de rôle**, cliquez sur **autorité de Certification**, puis cliquez sur **suivant**.  
  
11. Sur le **le Type d’installation** page, vérifiez que **AC d’entreprise** est sélectionnée, puis cliquez sur **suivant**.  
  
12. Sur le **spécifier le type de l’autorité de certification** page, vérifiez que **autorité de certification racine** est sélectionnée, puis cliquez sur **suivant**.  
  
13. Sur le **spécifier le type de la clé privée** page, vérifiez que **créer une nouvelle clé privée** est sélectionnée, puis cliquez sur **suivant**.  
  
14. Sur le **chiffrement pour l’autorité de certification** page, conservez les paramètres par défaut pour CSP (**RSA #Microsoft Software Key Storage Provider**) et l’algorithme de hachage (**SHA2**) et déterminer la meilleure longueur de clé pour votre déploiement. Longueurs de clé en caractères volumineux fournissent une sécurité optimale ; Cependant, ils peuvent affecter les performances du serveur et ne peuvent pas être compatibles avec les applications héritées. Il est recommandé de conserver le paramètre par défaut de 2048. Cliquez sur **Suivant**.  
  
15. Sur le **nom de l’autorité de certification** page, conservez le nom commun suggéré pour l’autorité de certification ou modifiez-le en fonction de vos besoins. Assurez-vous que vous êtes certain que le nom de l’autorité de certification est compatible avec vos conventions d’affectation de noms et les besoins, car vous ne pouvez pas modifier le nom de l’autorité de certification une fois que vous avez installé les services AD CS. Cliquez sur **Suivant**.  
  
16. Sur le **période de validité** page **spécifier la période de validité**, tapez le nombre et sélectionnez une valeur de temps (années, mois, semaines ou jours). Le paramètre par défaut de cinq ans est recommandé. Cliquez sur **Suivant**.  
  
17. Sur le **base de données de l’autorité de certification** page **spécifier les emplacements de base de données**, spécifiez l’emplacement du dossier pour la base de données de certificat et le journal de base de données de certificat. Si vous spécifiez des emplacements autres que les emplacements par défaut, assurez-vous que les dossiers sont sécurisés avec des listes de contrôle d’accès (ACL) qui empêchent les utilisateurs non autorisés ou des ordinateurs d’accéder à l’autorité de certification de base de données et les fichiers journaux. Cliquez sur **Suivant**.  
  
18. Dans **Confirmation**, cliquez sur **configurer** à appliquer vos sélections, puis cliquez sur **fermer**.  
  


