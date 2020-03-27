---
title: Installer l’autorité de certification
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: 4acdc3ad-078e-45cc-b54c-e9456e0c90f5
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fffb03a707c48c8dd485c68b5c6bf10783b0e6cf
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318364"
---
# <a name="install-the-certification-authority"></a>Installer l’autorité de certification

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour installer les services de certificats Active Directory (AD CS) afin de pouvoir inscrire un certificat de serveur sur les serveurs qui exécutent le serveur NPS (Network Policy Server), le service de routage et d’accès à distance (RRAS), ou les deux.  
  
> [!IMPORTANT]  
> -   Avant d’installer les services de certificats Active Directory, vous devez nommer l’ordinateur, configurer l’ordinateur avec une adresse IP statique et joindre l’ordinateur au domaine. Pour plus d’informations sur la façon d’accomplir ces tâches, consultez le [Guide du réseau de base](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide)de Windows Server 2016.  
> -   Pour effectuer cette procédure, l’ordinateur sur lequel vous installez les services AD CS doit être joint à un domaine dans lequel Active Directory Domain Services (AD DS) est installé.  
  
Pour être autorisé à effectuer cette procédure, vous devez faire partie du groupe **Administrateurs de l'entreprise** et du groupe **Admins du domaine** du domaine racine.  
  
> [!NOTE]  
> Pour effectuer cette procédure à l’aide de Windows PowerShell, ouvrez Windows PowerShell et tapez la commande suivante, puis appuyez sur entrée.   
>   
> `Add-WindowsFeature Adcs-Cert-Authority -IncludeManagementTools`  
>   
> Une fois les services AD CS installés, tapez la commande suivante et appuyez sur entrée.  
>   
> `Install-AdcsCertificationAuthority -CAType EnterpriseRootCA`  
  
### <a name="to-install-active-directory-certificate-services"></a>Pour installer les services de certificats Active Directory  

> [!TIP]
> Si vous souhaitez utiliser Windows PowerShell pour installer des services de certificats Active Directory, consultez [install-AdcsCertificationAuthority](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcscertificationauthority?view=win10-ps) pour les applets de commande et les paramètres facultatifs.
  
1.  Ouvrez une session en tant que membre du groupe Administrateurs de l'entreprise et du groupe Admins du domaine du domaine racine.  
  
2.  Dans le Gestionnaire de serveur, cliquez sur **Gérer**, puis sur **Ajouter des rôles et des fonctionnalités**. L’Assistant Ajout de rôles et de fonctionnalités s’ouvre.  
  
3.  Dans **Avant de commencer**, cliquez sur **Suivant**.  
  
    > [!NOTE]  
    > La page **Avant de commencer** de l’Assistant Ajout de rôles et de fonctionnalités ne s’affiche pas si vous avez préalablement sélectionné **Ignorer cette page par défaut** dans l’Assistant.  
  
4.  Dans **Sélectionner le type d’installation**, vérifiez que **Installation basée sur un rôle ou une fonctionnalité** est sélectionné, puis cliquez sur **Suivant**.  
  
5.  Dans **Sélectionner le serveur de destination**, vérifiez que **Sélectionner un serveur du pool de serveurs** est sélectionné. Dans **Pool de serveurs**, vérifiez que l’ordinateur local est sélectionné. Cliquez sur **Suivant**.  
  
6.  Dans **Sélectionner des rôles de serveurs**, dans **rôles**, sélectionnez Active Directory les **services de certificats**. Lorsque vous êtes invité à ajouter les fonctionnalités requises, cliquez sur **Ajouter des fonctionnalités**, puis sur **suivant**.  
  
7.  Dans **Sélectionner des fonctionnalités**, cliquez sur **suivant**.  
  
8.  Dans **Active Directory les services de certificats**, lisez les informations fournies, puis cliquez sur **suivant**.  
  
9. Dans **Confirmer les sélections pour l’installation**, cliquez sur **Installer**. Ne fermez pas l’Assistant pendant le processus d’installation. Une fois l’installation terminée, cliquez sur **configurer Active Directory les services de certificats sur le serveur de destination**. L’Assistant Configuration des services AD CS s’ouvre. Lisez les informations d’identification et, si nécessaire, fournissez les informations d’identification d’un compte membre du groupe administrateurs de l’entreprise. Cliquez sur **Suivant**.  
  
10. Dans **services de rôle**, cliquez sur **autorité de certification**, puis sur **suivant**.  
  
11. Sur la page **type d’installation** , vérifiez que l’option **autorité de certification d’entreprise** est sélectionnée, puis cliquez sur **suivant**.  
  
12. Dans la page **spécifier le type de l’autorité de certification** , vérifiez que **autorité de certification racine** est sélectionné, puis cliquez sur **suivant**.  
  
13. Dans la page **spécifier le type de la clé privée** , vérifiez que l’option **créer une nouvelle clé privée** est sélectionnée, puis cliquez sur **suivant**.  
  
14. Dans la page **chiffrement pour l’autorité de certification** , conservez les paramètres par défaut pour CSP (**RSA # Microsoft Software Key Storage Provider**) et algorithme de hachage (**SHA2**), et déterminez la meilleure longueur de clé pour votre déploiement. Les longueurs de caractères clés offrent une sécurité optimale ; Toutefois, ils peuvent avoir un impact sur les performances du serveur et peuvent ne pas être compatibles avec les applications héritées. Il est recommandé de conserver le paramètre par défaut 2048. Cliquez sur **Suivant**.  
  
15. Dans la page nom de l' **autorité de certification** , conservez le nom commun suggéré pour l’autorité de certification ou modifiez le nom en fonction de vos besoins. Assurez-vous que le nom de l’autorité de certification est compatible avec vos conventions d’affectation de noms et à vos objectifs, car vous ne pouvez pas modifier le nom de l’autorité de certification après avoir installé les services AD CS. Cliquez sur **Suivant**.  
  
16. Sur la page **période de validité** , dans **spécifier la période de validité**, tapez le nombre et sélectionnez une valeur de temps (années, mois, semaines ou jours). Le paramètre par défaut de cinq ans est recommandé. Cliquez sur **Suivant**.  
  
17. Dans la page **base de données de l’autorité de certification** , dans **Spécifiez les emplacements des bases**de données, spécifiez l’emplacement du dossier pour la base de données de certificats et le journal de la base de données Si vous spécifiez des emplacements autres que les emplacements par défaut, assurez-vous que les dossiers sont sécurisés à l’aide de listes de contrôle d’accès (ACL) qui empêchent les utilisateurs ou les ordinateurs non autorisés d’accéder à la base de données et aux fichiers journaux de l’autorité de certification. Cliquez sur **Suivant**.  
  
18. Dans **confirmation**, cliquez sur **configurer** pour appliquer vos sélections, puis cliquez sur **Fermer**.  
  


