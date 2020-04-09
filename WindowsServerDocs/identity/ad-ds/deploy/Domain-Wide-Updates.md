---
ms.assetid: 2a5d5271-6ac6-4c1b-b4ef-9b568932a55a
title: Active Directory des mises à jour de schéma à l’ensemble du domaine
description: Mises à jour de schéma à l’ensemble du domaine effectuées par adprep/domainprep lors de la promotion d’un contrôleur de domaine
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 10/29/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3b82958471e5292f202aa338aee7f4f5863459af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80825212"
---
# <a name="domain-wide-schema-updates"></a>Mises à jour de schéma à l’ensemble du domaine

>S’applique à : Windows Server

Vous pouvez consulter l’ensemble de modifications suivant pour vous aider à comprendre et à préparer les mises à jour de schéma effectuées par adprep/domainprep dans Windows Server.

À compter de Windows Server 2012, les commandes adprep s’exécutent automatiquement en fonction des besoins pendant AD DS installation. Ils peuvent également être exécutés séparément à l’avance de AD DS installation. Pour plus d’informations, voir [Exécution d’Adprep.exe](https://technet.microsoft.com/library/dd464018(v=ws.10).aspx).

Pour plus d’informations sur la façon d’interpréter les chaînes d’entrée de contrôle d’accès (ACE), consultez [chaînes ACE](https://msdn.microsoft.com/library/aa374928(VS.85).aspx). Pour plus d’informations sur la façon d’interpréter les chaînes d’ID de sécurité (SID), consultez [sid Strings](https://msdn.microsoft.com/library/aa379602(VS.85).aspx).

## <a name="windows-server-semi-annual-channel-domain-wide-updates"></a>Windows Server (canal semi-annuel) : mises à jour à l’ensemble du domaine

Une fois les opérations effectuées par **domainprep** dans Windows Server 2016 (Operation 89), l’attribut **Revision** pour l’objet CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = DomaineRacineForêt a la valeur **16**.

|Nombre d’opérations et GUID|Description|Autorisations|
|------------------------------|---------------|--------------|---------------|
|**Opération 89**: {A0C238BA-9E30-4EE6-80A6-43F731E9A5CD}|Supprimez l’ACE accordant le contrôle total aux administrateurs de clés d’entreprise et ajoutez un ACE accordant à l’administrateur de clé d’entreprise le contrôle total sur l’attribut msdsKeyCredentialLink uniquement.|Supprimer (A ; CI RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administrateurs de clé d’entreprise) <br /> <br />Add (OA ; CI RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Administrateurs de clé d’entreprise)|

## <a name="windows-server-2016-domain-wide-updates"></a>Windows Server 2016 : mises à jour à l’ensemble du domaine

Une fois les opérations effectuées par **domainprep** dans Windows Server 2016 (operations 82-88), l’attribut **Revision** pour l’objet CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = DomaineRacineForêt a la valeur **15**.

|Nombre d’opérations et GUID|Description|Attributs|Autorisations|
|------------------------------|---------------|--------------|---------------|
|**Opération 82**: {83C53DA7-427e-47A4-A07A-A324598B88F7}|Créer un conteneur CN = Keys à la racine du domaine|-objectClass : conteneur<br />-Description : conteneur par défaut pour les objets d’informations d’identification de clé<br />-ShowInAdvancedViewOnly : TRUE|Un CI RPWPCRLCLOCCDCRCWDWOSDDTSW;;; EA<br />Un CI RPWPCRLCLOCCDCRCWDWOSDDTSW ;;;D Un<br />Un CI RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Sy<br />Un CI RPWPCRLCLOCCDCRCWDWOSDDTSW ;;;D E<br />Un CI RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Ed|
|**Opération 83**: {C81FC9CC-0130-4fd1-B272-634D74818133}|Ajoutez contrôle total autoriser les ACE au conteneur CN = clés pour « domain\Key admins » et « rootdomain\Enterprise Key admins ».|N/A|Un CI RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administrateurs de clés)<br />Un CI RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administrateurs de clé d’entreprise)|
|**Opération 84**: {E5F9E791-D96D-4FC9-93C9-D53E1DC439BA}|Modifiez l’attribut otherWellKnownObjects pour qu’il pointe vers le conteneur CN = Keys.|-otherWellKnownObjects : B :32:683A24E2E8164BD3AF86AC3C2CF3F981 : CN = Keys,% WS|N/A|
|**Opération 85**: {e6d5fd00-385d-4e65-b02d-9da3493ed850}|Modifiez le contexte de nommage de domaine pour autoriser « domain\Key admins » et « rootdomain\Enterprise Key admins » à modifier l’attribut msDS-KeyCredentialLink. |N/A|FONCTIONNEMENT CI RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Administrateurs de clés)<br />FONCTIONNEMENT CI RPWP;5b47d60f-6090-40b2-9f37-2a4de88f3063;; Les administrateurs de clé d’entreprise dans le domaine racine, mais dans les domaines non racine, ont provoqué une fausse entrée de l’ACE relative au domaine avec un SID non résoluble-527.|
|**Opération 86**: {3a6b3fbf-3168-4312-a10d-dd5b3393952d}|Accordez à l’ordinateur de test DS-valid-Write-Computer ERE le propriétaire du créateur et|N/A|FONCTIONNEMENT CIIO ; SW ; 9b026da6-0d3c-465C-8bee-5199d7165cba ; bf967a86-0de6-11D0-A285-00aa003049e2 ; PS)<br />FONCTIONNEMENT CIIO ; SW ; 9b026da6-0d3c-465C-8bee-5199d7165cba ; bf967a86-0de6-11D0-A285-00aa003049e2 ; CO)|
|**Opération 87**: {7F950403-0AB3-47F9-9730-5D7B0269F9BD}|Supprimez l’entrée du contrôle d’accès accordant le contrôle total au groupe administrateurs de clés d’entreprise du domaine incorrect, puis ajoutez un ACE accordant le contrôle total au groupe administrateurs de clé d’entreprise. |N/A|Supprimer (A ; CI RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administrateurs de clé d’entreprise)<br /> <br />Ajouter (A ; CI RPWPCRLCLOCCDCRCWDWOSDDTSW;;; Administrateurs de clé d’entreprise)|
|**Opération 88**: {434bb40d-DBC9-4fe7-81d4-d57229f7b080}|Ajoutez « msDS-ExpirePasswordsOnSmartCardOnlyAccounts » sur l’objet de contexte de nommage de domaine et définissez la valeur par défaut sur FALSe.|N/A|N/A|

Les groupes Administrateurs de clé d’entreprise et administrateurs de clés sont créés uniquement après la promotion d’un contrôleur de domaine Windows Server 2016 et le rôle FSMO d’émulateur de contrôleur de domaine principal.

## <a name="windows-server-2012-r2-domain-wide-updates"></a>Windows Server 2012 R2 : mises à jour à l’ensemble du domaine

Bien qu’aucune opération ne soit effectuée par **domainprep** dans Windows Server 2012 R2, une fois la commande terminée, l’attribut **Revision** pour l’objet CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = DomaineRacineForêt a la valeur **10**.

## <a name="windows-server-2012-domain-wide-updates"></a>Windows Server 2012 : mises à jour à l’ensemble du domaine

Une fois les opérations effectuées par **domainprep** dans Windows Server 2012 (operations 78, 79, 80 et 81), l’attribut **Revision** de l’objet CN = ActiveDirectoryUpdate, CN = DomainUpdates, CN = System, DC = DomaineRacineForêt est défini sur **9**.

|Nombre d’opérations et GUID|Description|Attributs|Autorisations|
|------------------------------|---------------|--------------|---------------|
|**Opération 78**: {c3c927a6-cc1d-47C0-966B-be8f9b63d991}|Créez un objet CN = TPM Devices dans la partition de domaine.|Classe d’objet : msTPM-InformationObjectsContainer|N/A|
|**Opération 79**: {54afcfb9-637a-4251-9f47-4d50e7021211}|Création d’une entrée de contrôle d’accès pour le service TPM.|N/A|FONCTIONNEMENT CIIO; WP ; ea1b7b93-5e48-46D5-bc6c-4df4fda78a35 ; bf967a86-0de6-11D0-A285-00aa003049e2 ; PS)|
|**Opération 80**: {f4728883-84dd-483c-9897-274f2ebcf11e}|Accorder le droit étendu « clone DC » au groupe **contrôleurs de domaine clonables**|N/A|(OA ;; CR ; 3e0f7e18-2C7A-4c10-BA82-4d926db99a3e ;; *SID de domaine*-522)|
|**Opération 81**: {ff4f9d27-7157-4cb0-80a9-5d6f2b14c8ff}|Accordez ms-DS-allowed-to-Act-on-action-of-other-Identity à principal Self sur tous les objets.|N/A|FONCTIONNEMENT CIOI; RPWP;3f78c3e5-f79a-46bd-a0b8-9d18116ddc79;; ALIMENTATION|
