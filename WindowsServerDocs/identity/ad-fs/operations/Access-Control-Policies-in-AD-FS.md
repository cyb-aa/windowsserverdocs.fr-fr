---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: Stratégies de contrôle d’accès dans ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 27eb5b4b52dd727afae5cffc60e7d9749dd5d59f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407760"
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Stratégies de contrôle d’accès dans AD FS Windows Server 2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>Access Control des modèles de stratégie dans AD FS  
Services ADFS prend désormais en charge l’utilisation de modèles de stratégie de contrôle d’accès.  Grâce aux modèles de stratégie de contrôle d’accès, un administrateur peut appliquer des paramètres de stratégie en affectant le modèle de stratégie à un groupe de parties de confiance (RPs). L’administrateur peut également effectuer des mises à jour du modèle de stratégie et les modifications sont appliquées automatiquement aux parties de confiance si aucune intervention de l’utilisateur n’est nécessaire.  
  
## <a name="what-are-access-control-policy-templates"></a>Que sont les modèles de stratégie Access Control ?  
Le pipeline de base AD FS pour le traitement de stratégie comporte trois phases : l’authentification, l’autorisation et l’émission de revendications. Actuellement, AD FS administrateurs doivent configurer une stratégie pour chacune de ces phases séparément.  Cela implique également de comprendre les implications de ces stratégies et si ces stratégies ont une interdépendance. En outre, les administrateurs doivent comprendre le langage de règle de revendication et créer des règles personnalisées pour activer une stratégie simple/courante (par exemple, bloquer l’accès externe).  
  
Les modèles de stratégie de contrôle d’accès remplacent cet ancien modèle dans lequel les administrateurs doivent configurer des règles d’autorisation d’émission à l’aide du langage de revendications.  Les anciennes applets de commande PowerShell des règles d’autorisation d’émission s’appliquent toujours, mais elles s’excluent mutuellement du nouveau modèle. Les administrateurs peuvent choisir d’utiliser le nouveau modèle ou l’ancien modèle.  Le nouveau modèle permet aux administrateurs de contrôler le moment auquel accorder l’accès, y compris l’application de l’authentification multifacteur.  
  
Les modèles de stratégie de contrôle d’accès utilisent un modèle d’autorisation.  Cela signifie par défaut que personne n’a accès et que l’accès doit être accordé explicitement.  Toutefois, il ne s’agit pas simplement d’un permis de tout ou rien.  Les administrateurs peuvent ajouter des exceptions à la règle d’autorisation.  Par exemple, un administrateur peut souhaiter accorder l’accès en fonction d’un réseau spécifique en sélectionnant cette option et en spécifiant la plage d’adresses IP.  Toutefois, l’administrateur peut ajouter une exception, par exemple, l’administrateur peut ajouter une exception à partir d’un réseau spécifique et spécifier cette plage d’adresses IP.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>Modèles de stratégie de contrôle d’accès intégrés et modèles de stratégie de contrôle d’accès personnalisés  
AD FS comprend plusieurs modèles de stratégie de contrôle d’accès intégrés.  Ils ciblent des scénarios courants qui présentent le même ensemble de spécifications de stratégie, par exemple la stratégie d’accès client pour Office 365.  Ces modèles ne peuvent pas être modifiés.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
Pour offrir une plus grande flexibilité pour répondre aux besoins de votre entreprise, les administrateurs peuvent créer leurs propres modèles de stratégie d’accès.  Celles-ci peuvent être modifiées après la création et les modifications apportées au modèle de stratégie personnalisé s’appliqueront à tous les RPs contrôlés par ces modèles de stratégie.  Pour ajouter un modèle de stratégie personnalisée, il vous suffit de cliquer sur Ajouter une stratégie de Access Control dans AD FS Management.  
  
Pour créer un modèle de stratégie, un administrateur doit d’abord spécifier dans quelles conditions une demande sera autorisée pour l’émission et/ou la délégation de jetons. Les options de condition et d’action sont indiquées dans le tableau ci-dessous.   Les conditions en gras peuvent être configurées par l’administrateur avec des valeurs différentes ou nouvelles. L’administrateur peut également spécifier des exceptions, le cas échéant. Lorsqu’une condition est remplie, une action d’autorisation n’est pas déclenchée si une exception est spécifiée et que la demande entrante correspond à la condition spécifiée dans l’exception.  
  
|Autoriser les utilisateurs|Mais| 
| --- | --- | 
 |À partir d’un réseau **spécifique**|À partir d’un réseau **spécifique**<br /><br />À partir de groupes **spécifiques**<br /><br />À partir d’appareils avec des niveaux de confiance **spécifiques**<br /><br />Avec des revendications **spécifiques** dans la demande|  
|À partir de groupes **spécifiques**|À partir d’un réseau **spécifique**<br /><br />À partir de groupes **spécifiques**<br /><br />À partir d’appareils avec des niveaux de confiance **spécifiques**<br /><br />Avec des revendications **spécifiques** dans la demande|  
|À partir d’appareils avec des niveaux de confiance **spécifiques**|À partir d’un réseau **spécifique**<br /><br />À partir de groupes **spécifiques**<br /><br />À partir d’appareils avec des niveaux de confiance **spécifiques**<br /><br />Avec des revendications **spécifiques** dans la demande|  
|Avec des revendications **spécifiques** dans la demande|À partir d’un réseau **spécifique**<br /><br />À partir de groupes **spécifiques**<br /><br />À partir d’appareils avec des niveaux de confiance **spécifiques**<br /><br />Avec des revendications **spécifiques** dans la demande|  
|Et requièrent l’authentification multifacteur|À partir d’un réseau **spécifique**<br /><br />À partir de groupes **spécifiques**<br /><br />À partir d’appareils avec des niveaux de confiance **spécifiques**<br /><br />Avec des revendications **spécifiques** dans la demande|  
  
Si un administrateur sélectionne plusieurs conditions, il s’agit d’une relation de **et** de. Les actions s’excluent mutuellement et, pour une règle de stratégie, vous ne pouvez choisir qu’une seule action. Si l’administrateur sélectionne plusieurs exceptions, il s’agit d’une relation **ou** . Quelques exemples de règles de stratégie sont affichés ci-dessous :  
  
|**Règlement**|**Règles de stratégie**|
| --- | --- |  
|L’accès extranet requiert MFA<br /><br />Tous les utilisateurs sont autorisés|**#1 de la règle**<br /><br />à partir de l' **extranet**<br /><br />et avec MFA<br /><br />Autoriser<br /><br />**Règle n ° 2**<br /><br />à partir de l' **Intranet**<br /><br />Autoriser|  
|L’accès externe n’est pas autorisé sauf le non-ETP<br /><br />L’accès intranet pour ETP sur un appareil joint à un espace de travail est autorisé|**#1 de la règle**<br /><br />À partir de l' **extranet**<br /><br />et à partir d' **un groupe non-ETP**<br /><br />Autoriser<br /><br />**#2 de la règle**<br /><br />à partir de l' **Intranet**<br /><br />et à partir d’un appareil joint à l' **espace de travail**<br /><br />et à partir du groupe **ETP**<br /><br />Autoriser|  
|L’accès extranet requiert MFA, à l’exception de « service admin »<br /><br />Tous les utilisateurs sont autorisés à accéder à|**#1 de la règle**<br /><br />à partir de l' **extranet**<br /><br />et avec MFA<br /><br />Autoriser<br /><br />Sauf le **groupe d’administration de service**<br /><br />**#2 de la règle**<br /><br />Toujours<br /><br />Autoriser|  
|l’accès d’un appareil non lié à un emplacement de travail à partir d’un extranet requiert l’authentification MFA<br /><br />Autoriser l’accès intranet et extranet pour l’infrastructure AD|**#1 de la règle**<br /><br />à partir de l' **Intranet**<br /><br />Et à partir d’un groupe **ad Fabric**<br /><br />Autoriser<br /><br />**#2 de la règle**<br /><br />à partir de l' **extranet**<br /><br />et à partir d' **un appareil non joint à un espace de travail**<br /><br />et à partir d’un groupe **ad Fabric**<br /><br />et avec MFA<br /><br />Autoriser<br /><br />**#3 de la règle**<br /><br />à partir de l' **extranet**<br /><br />et à partir d’un appareil joint à l' **espace de travail**<br /><br />et à partir d’un groupe **ad Fabric**<br /><br />Autoriser|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>Modèle de stratégie paramétré et modèle de stratégie non paramétrable  
Les stratégies de contrôle d’accès peuvent être  
  
Un modèle de stratégie paramétré est un modèle de stratégie qui possède des paramètres. Un administrateur doit entrer la valeur de ces paramètres lors de l’affectation de ce modèle à RPs.An l’administrateur ne peut pas apporter de modifications au modèle de stratégie paramétré une fois qu’il a été créé.  Un exemple de stratégie paramétrable est la stratégie intégrée, autoriser un groupe spécifique.  Chaque fois que cette stratégie est appliquée à un RP, ce paramètre doit être spécifié.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
Un modèle de stratégie non paramétrable est un modèle de stratégie qui n’a pas de paramètres. Un administrateur peut attribuer ce modèle à RPs sans aucune entrée nécessaire et peut apporter des modifications à un modèle de stratégie non paramétrable une fois qu’il a été créé.  Par exemple, la stratégie intégrée, autoriser tout le monde et demander l’authentification MFA.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>Comment créer une stratégie de contrôle d’accès non paramétrable  
Pour créer une stratégie de contrôle d’accès non paramétrable, utilisez la procédure suivante :  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>Pour créer une stratégie de contrôle d’accès non paramétrable  
  
1.  Dans AD FS gestion sur la gauche, sélectionnez Access Control stratégies, puis cliquez avec le bouton droit sur Ajouter une stratégie de Access Control.  
  
2.  Entrez un nom et une description.  Exemple :  Autorisez les utilisateurs avec des appareils authentifiés.  
  
3.  Sous **autoriser l’accès si l’une des règles suivantes est remplie**, cliquez sur **Ajouter**.  
  
4.  Sous autoriser, activez la case à cocher en regard **de appareils avec un niveau de confiance spécifique** .  
  
5.  En bas, sélectionnez la ligne **spécifique** soulignée.  
  
6.  Dans la fenêtre qui s’affiche, sélectionnez **authentifié** dans la liste déroulante.  Cliquez sur **OK**.  
  
    ![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  Cliquez sur **OK**. Cliquez sur **OK**.  
  
    ![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>Comment créer une stratégie de contrôle d’accès paramétrable  
Pour créer une stratégie de contrôle d’accès paramétrable, utilisez la procédure suivante :  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Pour créer une stratégie de contrôle d’accès paramétrable  
  
1.  Dans AD FS gestion sur la gauche, sélectionnez Access Control stratégies, puis cliquez avec le bouton droit sur Ajouter une stratégie de Access Control.  
  
2.  Entrez un nom et une description.  Exemple :  Autoriser les utilisateurs avec une revendication spécifique.  
  
3.  Sous **autoriser l’accès si l’une des règles suivantes est remplie**, cliquez sur **Ajouter**.  
  
4.  Sous autoriser, activez la case à cocher en regard de **avec des revendications spécifiques dans la demande** .  
  
5.  En bas, sélectionnez la ligne **spécifique** soulignée.  
  
6.  Dans la fenêtre qui s’affiche, sélectionnez le **paramètre spécifié lorsque la stratégie de contrôle d’accès est assignée**.  Cliquez sur **OK**.  
  
    ![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  Cliquez sur **OK**. Cliquez sur **OK**.  
  
    ![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>Comment créer une stratégie de contrôle d’accès personnalisée à l’aide d’une exception  
Pour créer une stratégie de contrôle d’accès avec une exception, utilisez la procédure suivante.  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>Pour créer une stratégie de contrôle d’accès personnalisée à l’aide d’une exception  
  
1.  Dans AD FS gestion sur la gauche, sélectionnez Access Control stratégies, puis cliquez avec le bouton droit sur Ajouter une stratégie de Access Control.  
  
2.  Entrez un nom et une description.  Exemple :  Autoriser les utilisateurs avec des appareils authentifiés mais non gérés.  
  
3.  Sous **autoriser l’accès si l’une des règles suivantes est remplie**, cliquez sur **Ajouter**.  
  
4.  Sous autoriser, activez la case à cocher en regard **de appareils avec un niveau de confiance spécifique** .  
  
5.  En bas, sélectionnez la ligne **spécifique** soulignée.  
  
6.  Dans la fenêtre qui s’affiche, sélectionnez **authentifié** dans la liste déroulante.  Cliquez sur **OK**.  
  
7.  Sous sauf, activez la case à cocher en regard **de appareils avec un niveau de confiance spécifique** .  
  
8.  En bas sous sauf, sélectionnez la ligne **spécifique** soulignée.  
  
9. Dans la fenêtre qui s’affiche, sélectionnez **géré** dans la liste déroulante.  Cliquez sur **OK**.  
  
10. Cliquez sur **OK**. Cliquez sur **OK**.  
  
    ![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>Comment créer une stratégie de contrôle d’accès personnalisée avec plusieurs conditions d’autorisation  
Pour créer une stratégie de contrôle d’accès avec plusieurs conditions d’autorisation, utilisez la procédure suivante :  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Pour créer une stratégie de contrôle d’accès paramétrable  
  
1.  Dans AD FS gestion sur la gauche, sélectionnez Access Control stratégies, puis cliquez avec le bouton droit sur Ajouter une stratégie de Access Control.  
  
2.  Entrez un nom et une description.  Exemple :  Autoriser les utilisateurs avec une revendication spécifique et à partir d’un groupe spécifique.  
  
3.  Sous **autoriser l’accès si l’une des règles suivantes est remplie**, cliquez sur **Ajouter**.  
  
4.  Sous autoriser, activez la case à cocher en regard **de à partir d’un groupe spécifique** et **avec des revendications spécifiques dans la demande** .  
  
5.  En bas, sélectionnez la ligne **spécifique** soulignée pour la première condition, en regard de groupes  
  
6.  Dans la fenêtre qui s’affiche, sélectionnez le **paramètre spécifié lorsque la stratégie est affectée**.  Cliquez sur **OK**.  
  
7.  En bas, sélectionnez la ligne **spécifique** soulignée pour la deuxième condition, en regard de revendications.  
  
8.  Dans la fenêtre qui s’affiche, sélectionnez le **paramètre spécifié lorsque la stratégie de contrôle d’accès est assignée**.  Cliquez sur **OK**.  
  
9. Cliquez sur **OK**. Cliquez sur **OK**.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>Comment attribuer une stratégie de contrôle d’accès à une nouvelle application  
L’attribution d’une stratégie de contrôle d’accès à une nouvelle application est relativement simple et est désormais intégrée à l’Assistant pour l’ajout d’un RP.  À partir de l’Assistant approbation de partie de confiance, vous pouvez sélectionner la stratégie de contrôle d’accès que vous souhaitez affecter.  Il s’agit d’une condition requise lors de la création d’une approbation de partie de confiance.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>Comment attribuer une stratégie de contrôle d’accès à une application existante  
L’attribution d’une stratégie de contrôle d’accès à une application existante consiste simplement à sélectionner l’application auprès des approbations de partie de confiance, puis à cliquer avec le bouton droit sur **modifier la stratégie de Access Control**.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
À partir de là, vous pouvez sélectionner la stratégie de contrôle d’accès et l’appliquer à l’application.  
  
![stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>Voir aussi  
[Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md) 

