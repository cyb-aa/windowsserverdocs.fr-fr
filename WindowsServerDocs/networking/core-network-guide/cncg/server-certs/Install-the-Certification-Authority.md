---
title: Installer l’autorité de Certification
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17a887ab32570b739d5ca99611ee0d496a966d1b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-certification-authority"></a>Installer l’autorité de Certification

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour installer les Services de certificats ActiveDirectory (ADCS) afin que vous pouvez inscrire un certificat de serveur pour les serveurs qui exécutent le serveur NPS (Network Policy), routage et accès distant (RRAS) ou les deux.  
  
> [!IMPORTANT]  
> -   Avant d’installer les Services de certificats ActiveDirectory, vous devez nommer l’ordinateur, configurez l’ordinateur avec une adresse IP statique et joindre l’ordinateur au domaine. Pour plus d’informations sur comment effectuer ces tâches, consultez Windows Server2016 [Guide réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide).  
> -   Pour effectuer cette procédure, l’ordinateur sur lequel vous installez les services ADCS doit être joint à un domaine où les Services de domaine ActiveDirectory (ADDS) est installé.  
  
L’appartenance dans les deux **administrateurs de l’entreprise** et du domaine racine **Admins du domaine** groupe est la condition minimale requise pour effectuer cette procédure.  
  
> [!NOTE]  
> Pour effectuer cette procédure en utilisant Windows PowerShell, ouvrez Windows PowerShell et tapez la commande suivante et appuyez sur ENTRÉE.   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> Une fois les services ADCS est installé, tapez la commande suivante et appuyez sur ENTRÉE.  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>Pour installer les Services de certificats ActiveDirectory  
  
1.  Ouvrez une session en tant que membre du groupe Administrateurs de l’entreprise et Admins du domaine du domaine racine.  
  
2.  Dans le Gestionnaire de serveur, cliquez sur **gérer**, puis cliquez sur **Ajout de rôles et fonctionnalités**. L’Assistant Ajouter des rôles et fonctionnalités s’ouvre.  
  
3.  Dans **avant de commencer**, cliquez sur **suivant**.  
  
    > [!NOTE]  
    > Le **avant de commencer** page de l’ajout de rôles et fonctionnalités Assistant ne s’affiche pas si vous avez sélectionné précédemment **ignorer cette page par défaut** lorsque l’ajout de rôles et fonctionnalités Assistant a été exécuté.  
  
4.  Dans **sélectionner le Type d’Installation**, assurez-vous que **installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **suivant**.  
  
5.  Dans **serveur de destination sélectionnez**, assurez-vous que **sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **suivant**.  
  
6.  Dans **sélectionner des rôles de serveur**, dans **rôles**, sélectionnez **ActiveDirectory Certificate Services**. Lorsque vous êtes invité à ajouter les fonctionnalités requises, cliquez sur **ajouter des fonctionnalités**, puis cliquez sur **suivant**.  
  
7.  Dans **sélectionner des fonctionnalités**, cliquez sur **suivant**.  
  
8.  Dans **ActiveDirectory Certificate Services**, lisez les informations fournies, puis cliquez sur **suivant**.  
  
9. Dans **confirmer les sélections d’installation**, cliquez sur **installer**. Ne fermez pas l’Assistant pendant le processus d’installation. Lorsque l’installation est terminée, cliquez sur **configurer des Services de certificats ActiveDirectory sur le serveur de destination**. L’Assistant de Configuration ADCS s’ouvre. Lire les informations d’identification et, si nécessaire, indiquez les informations d’identification d’un compte qui est membre du groupe Administrateurs de l’entreprise. Cliquez sur **suivant**.  
  
10. Dans **les Services de rôle**, cliquez sur **autorité de Certification**, puis cliquez sur **suivant**.  
  
11. Sur le **le Type d’installation** page, vérifiez que **AC d’entreprise** est sélectionné, puis cliquez sur **suivant**.  
  
12. Sur le **spécifier le type de l’autorité de certification** page, vérifiez que **autorité de certification racine** est sélectionné, puis cliquez sur **suivant**.  
  
13. Sur le **spécifier le type de la clé privée** page, vérifiez que **créer une nouvelle clé privée** est sélectionné, puis cliquez sur **suivant**.  
  
14. Sur le **chiffrement pour l’autorité de certification** page, conservez les paramètres par défaut pour CSP (**RSA #MicrosoftSoftware Key Storage Provider**) et l’algorithme de hachage (**SHA1**) et déterminez la meilleure longueur de clé en caractères pour votre déploiement. Longueurs de clé en caractères fournissent une sécurité optimale; toutefois, ils peuvent affecter les performances du serveur et ne peuvent pas être compatibles avec les applications héritées. Il est recommandé de conserver le paramètre par défaut de 2048. Cliquez sur **suivant**.  
  
15. Sur le **nom de l’autorité de certification** page, conservez le nom commun suggéré pour l’autorité de certification ou modifiez-le en fonction de vos besoins. Vérifiez que vous êtes certain que le nom de l’autorité de certification est compatible avec votre conventions d’affectation de noms et les besoins, car vous ne pouvez pas modifier le nom de l’autorité de certification après avoir installé les services ADCS. Cliquez sur **suivant**.  
  
16. Sur le **période de validité** page dans **spécifier la période de validité**, tapez le nombre et sélectionnez une valeur de temps (années, mois, semaines ou jours). Le paramètre par défaut de cinq ans est recommandé. Cliquez sur **suivant**.  
  
17. Sur le **base de données de l’autorité de certification** page dans **spécifier des emplacements de la base de données**, spécifiez l’emplacement du dossier de la base de données de certificats et du journal de base de données de certificats. Si vous spécifiez des emplacements autres que les emplacements par défaut, assurez-vous que les dossiers sont sécurisées avec les listes de contrôle d’accès (ACL) qui empêchent les utilisateurs non autorisés ou les ordinateurs d’accéder à l’autorité de certification de base de données et les fichiers journaux. Cliquez sur **suivant**.  
  
18. Dans **Confirmation**, cliquez sur **configurer** pour appliquer vos sélections, puis cliquez sur **fermer**.  
  


