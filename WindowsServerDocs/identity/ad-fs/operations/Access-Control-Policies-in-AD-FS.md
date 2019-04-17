---
ms.assetid: 102eeeb1-6c55-42a2-b321-71a7dab46146
title: "Stratégies de contrôle d’accès dans ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 101cab68d7c79bb107f1d6ef73900d9a4475b6ea
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="access-control-policies-in-windows-server-2016-ad-fs"></a>Stratégies de contrôle d’accès dans Windows Server2016 ADFS

>S’applique à: Windows Server2016

  
## <a name="access-control-policy-templates-in-ad-fs"></a>Modèles de stratégie de contrôle de l’accès dans ADFS  
ActiveDirectory Federation Services prend désormais en charge l’utilisation de modèles de stratégie de contrôle de l’accès.  À l’aide de modèles de stratégie de contrôle de l’accès, un administrateur peut appliquer des paramètres de stratégie en attribuant le modèle de stratégie à un groupe des parties de confiance (RPs). Administrateur peut également mettre à jour le modèle de stratégie et les modifications seront appliquées pour les parties de confiance automatiquement s’il n’existe aucune interaction utilisateur nécessitée.  
  
## <a name="what-are-access-control-policy-templates"></a>Quelles sont les modèles de stratégie de contrôle d’accès?  
Pipeline de traitement de la stratégie core ADFS comprend trois phases: émission d’authentification, d’autorisation et de revendication. Actuellement, les administrateurs ADFS doivent configurer une stratégie pour chacune de ces phases séparément.  Cela implique également comprendre les implications de ces stratégies et si ces stratégies ont une dépendance entre. En outre, les administrateurs doivent comprendre les règle langue et auteur personnalisés règles de revendication pour activer certaines stratégie commun simple (par ex. Bloquer l’accès externe).  
  
Quelle stratégie de contrôle d’accès modèles faire est de remplacer cet ancien modèle où les administrateurs doivent configurer des règles d’autorisation d’émission des revendications langue.  Les applets de commande PowerShell anciens de règles d’autorisation d’émission s’appliquent toujours, mais elle s’excluent mutuellement le nouveau modèle. Les administrateurs peuvent choisir soit d’utiliser le nouveau modèle ou l’ancien modèle.  Le nouveau modèle permet aux administrateurs de contrôler quand accorder l’accès, notamment pour faire appliquer l’authentification multifacteur.  
  
Modèles de stratégie de contrôle de l’accès utilisent un modèle d’autorisation.  Cela signifie que par défaut, a accès et que l’accès doit être explicitement accordé.  Toutefois, cela n’est pas simplement un tout ou rien autoriser.  Les administrateurs peuvent ajouter des exceptions à la règle d’autorisation.  Par exemple, un administrateur peut vouloir accorder l’accès basé sur un réseau spécifique en sélectionnant cette option et en spécifiant la plage d’adresses IP.  Toutefois, l’administrateur peut ajouter et exception, par exemple, l’administrateur peut ajouter une exception à partir d’un réseau spécifique et spécifiez la plage d’adresses IP.  
  
![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP2.PNG)  
  
## <a name="built-in-access-control-policy-templates-vs-custom-access-control-policy-templates"></a>Modèles de stratégie contrôle modèles Visual Studio accès personnalisé accès intégré contrôle stratégie  
ADFS comprend plusieurs modèles de stratégie de contrôle des accès intégré.  Ces cibles quelques scénarios courants qui ont le même ensemble d’exigences de stratégie, par exemple les stratégie d’accès client pour Office 365.  Ces modèles ne peuvent pas être modifiées.  
  
![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP3.PNG)  
  
Pour fournir une plus grande souplesse pour répondre à vos besoins, les administrateurs peuvent créer leurs propres accès aux modèles de stratégie.  Elles peuvent être modifiées après la création et modifications apportées au modèle de stratégie personnalisée seront applique à tous les RPs qui sont contrôlés par les modèles de stratégie.  Pour ajouter un modèle de stratégie personnalisée Cliquez simplement sur Ajouter une stratégie contrôle l’accès à partir de gestion ADFS.  
  
Pour créer un modèle de stratégie, un administrateur doit d’abord spécifier une demande sera autorisée sur les conditions d’émission de jeton et/ou de délégation. Options de condition et une action s’affichent dans le tableau ci-dessous.   Conditions en gras peuvent être plus configurées par l’administrateur avec des valeurs différentes ou nouveaux. Administrateur peut également spécifier des exceptions s’il existe. Lorsqu’une condition est remplie, une action d’autorisation ne sera pas déclenchée s’il existe une exception spécifiée et la demande entrante correspond à la condition spécifiée dans l’exception.  
  
|Autoriser les utilisateurs|À l’exception| 
| --- | --- | 
 |À partir de **spécifique** réseau|À partir de **spécifique** réseau<br /><br />À partir de **spécifique** groupes<br /><br />À partir d’appareils avec **spécifique** les niveaux de confiance<br /><br />Avec **spécifique** revendications dans la demande|  
|À partir de **spécifique** groupes|À partir de **spécifique** réseau<br /><br />À partir de **spécifique** groupes<br /><br />À partir d’appareils avec **spécifique** les niveaux de confiance<br /><br />Avec **spécifique** revendications dans la demande|  
|À partir d’appareils avec **spécifique** les niveaux de confiance|À partir de **spécifique** réseau<br /><br />À partir de **spécifique** groupes<br /><br />À partir d’appareils avec **spécifique** les niveaux de confiance<br /><br />Avec **spécifique** revendications dans la demande|  
|Avec **spécifique** revendications dans la demande|À partir de **spécifique** réseau<br /><br />À partir de **spécifique** groupes<br /><br />À partir d’appareils avec **spécifique** les niveaux de confiance<br /><br />Avec **spécifique** revendications dans la demande|  
|Et exiger une authentification multifacteur|À partir de **spécifique** réseau<br /><br />À partir de **spécifique** groupes<br /><br />À partir d’appareils avec **spécifique** les niveaux de confiance<br /><br />Avec **spécifique** revendications dans la demande|  
  
Si un administrateur sélectionne plusieurs conditions, ils sont de **et** relation. Actions sont excluent mutuellement, et pour une règle, vous pouvez choisir uniquement une seule action. Si l’administrateur sélectionne plusieurs exceptions, ils sont d’une **ou** relation. Voici quelques exemples de règle de stratégie:  
  
|**Stratégie**|**Règles de stratégie**|
| --- | --- |  
|L’accès extranet exige l’authentification Multifacteur<br /><br />Tous les utilisateurs sont autorisés.|**Règle #1**<br /><br />à partir de **extranet**<br /><br />Et avec l’authentification Multifacteur<br /><br />Autoriser<br /><br />**Règle n ° 2**<br /><br />à partir de **intranet**<br /><br />Autoriser|  
|Un accès externe ne sont pas autorisés à l’exception non-des employés<br /><br />Accès intranet pour des employés sur un appareil joint à un espace de travail sont autorisés.|**Règle #1**<br /><br />À partir de **extranet**<br /><br />Et à partir de **non FTE** groupe<br /><br />Autoriser<br /><br />**Règle #2**<br /><br />à partir de **intranet**<br /><br />Et à partir de **joint à un espace de travail** périphérique<br /><br />Et à partir de **des employés** groupe<br /><br />Autoriser|  
|L’accès extranet exige l’authentification Multifacteur à l’exception de «service admin»<br /><br />Tous les utilisateurs sont autorisés à accéder|**Règle #1**<br /><br />à partir de **extranet**<br /><br />Et avec l’authentification Multifacteur<br /><br />Autoriser<br /><br />À l’exception **groupe des administrateurs de service**<br /><br />**Règle #2**<br /><br />toujours<br /><br />Autoriser|  
|APPAREIL joint à un lieu de travail non - accès à partir d’extranet exige l’authentification Multifacteur<br /><br />Autoriser la structure ActiveDirectory pour l’intranet et l’accès extranet|**Règle #1**<br /><br />à partir de **intranet**<br /><br />Et à partir de **AD Fabric** groupe<br /><br />Autoriser<br /><br />**Règle #2**<br /><br />à partir de **extranet**<br /><br />Et à partir de **non-espace de travail joint** périphérique<br /><br />Et à partir de **AD Fabric** groupe<br /><br />Et avec l’authentification Multifacteur<br /><br />Autoriser<br /><br />**Règle #3**<br /><br />à partir de **extranet**<br /><br />Et à partir de **joint à un espace de travail** périphérique<br /><br />Et à partir de **AD Fabric** groupe<br /><br />Autoriser|  
  
## <a name="parameterized-policy-template-vs-non-parameterized-policy-template"></a>Modèle de stratégie non paramétrée de Visual Studio de modèle Stratégie paramétrées  
Stratégies de contrôle d’accès peuvent être  
  
Un modèle de stratégie paramétrée est un modèle de stratégie qui a des paramètres. Un administrateur doit entrer la valeur de ces paramètres lors de l’attribution de ce modèle à RPs.An administrateur ne peut pas apporter des modifications au modèle de stratégie paramétrée après que qu’il a été créé.  Un exemple d’une stratégie paramétrée est la stratégie intégrée, d’un groupe spécifique d’autorisation.  Chaque fois que cette stratégie est appliquée à une identité, ce paramètre doit être spécifié.  
  
![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP5.PNG)  
  
Un modèle de stratégie non paramétrée est un modèle de stratégie qui n’a pas de paramètres. Un administrateur peut affecter ce modèle à RPs sans aucune entrée nécessitée et permettre apporter des modifications à un modèle de stratégie non paramétrée après que qu’il a été créé.  Ceci est la stratégie intégrée, autoriser tout le monde et exiger l’authentification Multifacteur.  
  
![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP4.PNG)  
  
## <a name="how-to-create-a-non-parameterized-access-control-policy"></a>Comment créer une stratégie de contrôle d’accès non paramétrées  
Procédez comme suit pour créer un accès non paramétrée stratégie de contrôle  
  
#### <a name="to-create-a-non-parameterized-access-control-policy"></a>Pour créer une stratégie de contrôle d’accès non paramétrées  
  
1.  À partir de gestion ADFS sur la gauche Sélectionnez des stratégies de contrôle d’accès et sur la droite, cliquez sur Ajouter une stratégie de contrôle d’accès.  
  
2.  Entrez un nom et une description.  Par exemple: permettre aux utilisateurs avec des périphériques authentifiés.  
  
3.  Sous **autoriser l’accès si une des règles suivantes sont remplie**, cliquez sur **ajouter**.  
  
4.  Sous Autoriser, placez une vérification dans la zone regard **à partir d’appareils avec un niveau de confiance spécifique**  
  
5.  En bas, sélectionnez le texte souligné **spécifique**  
  
6.  Dans la fenêtre qui pop-up, sélectionnez **authentifié** dans la liste déroulante.  Cliquez sur **Ok**.  
  
    ![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP6.PNG)  
  
7.  Cliquez sur **Ok**. Cliquez sur **Ok**.  
  
    ![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP7.PNG)  
  
## <a name="how-to-create-a-parameterized-access-control-policy"></a>Comment créer une stratégie de contrôle d’accès paramétrées  
Pour créer un contrôle d’accès paramétrée stratégie utiliser la procédure suivante  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Pour créer une stratégie de contrôle d’accès paramétrées  
  
1.  À partir de gestion ADFS sur la gauche Sélectionnez des stratégies de contrôle d’accès et sur la droite, cliquez sur Ajouter une stratégie de contrôle d’accès.  
  
2.  Entrez un nom et une description.  Par exemple: permettre aux utilisateurs avec une revendication spécifique.  
  
3.  Sous **autoriser l’accès si une des règles suivantes sont remplie**, cliquez sur **ajouter**.  
  
4.  Sous Autoriser, placez une vérification dans la zone regard **avec les revendications spécifiques dans la demande**  
  
5.  En bas, sélectionnez le texte souligné **spécifique**  
  
6.  Dans la fenêtre qui pop-up, sélectionnez **paramètre spécifié lorsque la stratégie de contrôle d’accès est attribuée**.  Cliquez sur **Ok**.  
  
    ![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP8.PNG)  
  
7.  Cliquez sur **Ok**. Cliquez sur **Ok**.  
  
    ![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP9.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-an-exception"></a>Comment créer une stratégie de contrôle d’accès personnalisés avec une exception  
Pour créer un contrôle d’accès stratégie avec une exception utilisez la procédure suivante.  
  
#### <a name="to-create-a-custom-access-control-policy-with-an-exception"></a>Pour créer une stratégie de contrôle d’accès personnalisés avec une exception  
  
1.  À partir de gestion ADFS sur la gauche Sélectionnez des stratégies de contrôle d’accès et sur la droite, cliquez sur Ajouter une stratégie de contrôle d’accès.  
  
2.  Entrez un nom et une description.  Par exemple: permis aux utilisateurs authentifiés périphériques mais pas géré.  
  
3.  Sous **autoriser l’accès si une des règles suivantes sont remplie**, cliquez sur **ajouter**.  
  
4.  Sous Autoriser, placez une vérification dans la zone regard **à partir d’appareils avec un niveau de confiance spécifique**  
  
5.  En bas, sélectionnez le texte souligné **spécifique**  
  
6.  Dans la fenêtre qui pop-up, sélectionnez **authentifié** dans la liste déroulante.  Cliquez sur **Ok**.  
  
7.  Sous à l’exception, placez une vérification dans la zone regard **à partir d’appareils avec un niveau de confiance spécifique**  
  
8.  En bas, sauf sous, sélectionnez le texte souligné **spécifique**  
  
9. Dans la fenêtre qui pop-up, sélectionnez **gérés** dans la liste déroulante.  Cliquez sur **Ok**.  
  
10. Cliquez sur **Ok**. Cliquez sur **Ok**.  
  
    ![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP10.PNG)  
  
## <a name="how-to-create-a-custom-access-control-policy-with-multiple-permit-conditions"></a>Comment créer une stratégie de contrôle d’accès personnalisés avec plusieurs conditions de l’autorisation  
Pour créer une stratégie de contrôle d’accès avec autoriser plusieurs conditions d’utilisent la procédure suivante  
  
#### <a name="to-create-a-parameterized-access-control-policy"></a>Pour créer une stratégie de contrôle d’accès paramétrées  
  
1.  À partir de gestion ADFS sur la gauche Sélectionnez des stratégies de contrôle d’accès et sur la droite, cliquez sur Ajouter une stratégie de contrôle d’accès.  
  
2.  Entrez un nom et une description.  Par exemple: permettre aux utilisateurs avec une revendication spécifique et de groupe spécifique.  
  
3.  Sous **autoriser l’accès si une des règles suivantes sont remplie**, cliquez sur **ajouter**.  
  
4.  Sous Autoriser, placez une vérification dans la zone regard **à partir d’un groupe spécifique** et **avec les revendications spécifiques dans la demande**  
  
5.  En bas, sélectionnez le texte souligné **spécifique** pour la première condition, en regard de groupes  
  
6.  Dans la fenêtre qui pop-up, sélectionnez **paramètre spécifié lorsque la stratégie est attribuée**.  Cliquez sur **Ok**.  
  
7.  En bas, sélectionnez le texte souligné **spécifique** pour la deuxième condition, en regard de revendications  
  
8.  Dans la fenêtre qui pop-up, sélectionnez **paramètre spécifié lorsque la stratégie de contrôle d’accès est attribuée**.  Cliquez sur **Ok**.  
  
9. Cliquez sur **Ok**. Cliquez sur **Ok**.  
  
![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP12.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-a-new-application"></a>Comment attribuer une stratégie de contrôle d’accès à une nouvelle application  
Affectation d’une stratégie de contrôle d’accès à une nouvelle application est assez simple et a maintenant été intégré dans l’Assistant pour l’ajout d’une identité.  Dans l’Assistant d’approbation de partie de confiance, vous pouvez sélectionner la stratégie de contrôle d’accès que vous souhaitez attribuer.  Cela est obligatoire lors de la création d’une nouvelle approbation de partie de confiance.  
  
![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP13.PNG)  
  
## <a name="how-to-assign-an-access-control-policy-to-an-existing-application"></a>Comment attribuer une stratégie de contrôle d’accès à une application existante  
Attribution d’une stratégie de contrôle d’accès à une application existante Sélectionnez simplement l’application d’approbations de partie de confiance et sur le bouton droit sur **modifier une stratégie de contrôle d’accès**.  
  
![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP14.PNG)  
  
À partir de là, vous pouvez sélectionner la stratégie de contrôle d’accès et s’appliquent à l’application.  
  
![Stratégies de contrôle d’accès](media/Access-Control-Policies-in-AD-FS/ADFSACP15.PNG)  
  
## <a name="see-also"></a>Voir aussi  
[Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md) 

