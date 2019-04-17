---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: "Stratégies de contrôle d’accès dans ADFS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0b4549192bc4dab9edf3b2a10421325144ed0cd2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Stratégies de contrôle d’accès dans Windows Server2012R2 et WindowsServerFS2012 AD

>S’applique à: Windows Server2012R2 et Windows Server2012 

Vérifiez les stratégies décrites dans cet article utilisent deux types de revendications  
  
1.  Revendications QU'ADFS crée en fonction des informations du proxy ADFS et l’Application Web peut examiner et vérifier, telles que l’adresse IP du client se connectant directement à ADFS ou le protocole WAP.  
  
2.  Revendications ADFS crée la fonction des informations communiquées à ADFS par le client en tant que les en-têtes HTTP  
  
>**Important**: les stratégies comme expliqué ci-dessous bloquera jonction de domaine Windows10 et les scénarios qui nécessitent un accès aux points de terminaison supplémentaires suivantes pour l’ouverture de session

Requis pour la jonction de domaine Windows10 et de connexion sur les points de terminaison ADFS
- [nom du service de fédération] / adfs/services/approbation/2005/windowstransport
- [nom du service de fédération] / adfs/services/approbation/13/windowstransport
- [nom du service de fédération] / adfs/services/approbation/2005/usernamemixed
- [nom du service de fédération] / adfs/services/approbation/13/usernamemixed 
- [nom du service de fédération] / adfs/services/approbation/2005/certificatemixed
- [nom du service de fédération] / adfs/services/approbation/13/certificatemixed

Pour résoudre, mettre à jour des stratégies qui refusent basées sur la point de terminaison revendication pour autoriser l’exception pour les points de terminaison ci-dessus.

Par exemple, la règle ci-dessous:

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

Serait mis à jour pour:

`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Revendications de cette catégorie doivent uniquement être utilisées pour implémenter des stratégies d’entreprise et non sous forme de stratégies de sécurité pour protéger l’accès à votre réseau.  Il est possible pour les clients non autorisés à envoyer des en-têtes de fausses informations comme un moyen d’accéder.  
  
Les stratégies décrites dans cet article doivent toujours être utilisés avec une autre méthode d’authentification, telles que l’authentification facteur nom d’utilisateur et mot de passe ou multiples.  
  
## <a name="client-access-policies-scenarios"></a>Scénarios de stratégies d’accès client  
  
|**Scénario**|**Description**| 
| --- | --- | 
|Scénario 1: Bloquer tout accès externe à Office 365|Office 365 est accessible à partir de tous les clients sur le réseau d’entreprise interne, mais les demandes des clients externes sont refusées basé sur l’adresse IP du client externe.|  
|Scénario 2: Bloquer tout accès externe à Office 365 à l’exception d’Exchange ActiveSync|Office 365 est accessible à partir de tous les clients sur le réseau d’entreprise interne, ainsi que de tous les périphériques clients externes, tels que des Smartphones, qui utilisent Exchange ActiveSync. Tous les autres clients externes, tels que ceux à l’aide d’Outlook, sont bloqués.|  
|Scénario 3: Bloquer tout accès externe à Office 365 à l’exception des applications basées sur le navigateur|Bloque l’accès externe à Office 365, à l’exception des applications passives (basée sur le navigateur) telles que Outlook Web Access ou SharePoint Online.|  
|Scénario 4: Bloquer tout accès externe à Office 365 à l’exception des groupes ActiveDirectory désignés|Ce scénario est utilisé pour tester et valider le déploiement de stratégies d’accès client. Il bloque un accès externe à Office 365 uniquement pour les membres d’un ou plusieurs groupe ActiveDirectory. Il peut également être utilisé pour fournir un accès externe uniquement aux membres d’un groupe.|  
  
## <a name="enabling-client-access-policy"></a>L’activation de stratégie d’accès Client  
 Pour activer la stratégie d’accès client dans ADFS dans Windows Server2012R2, vous devez mettre à jour de la plate-forme d’identité de MicrosoftOffice 365 de confiance. Choisissez parmi les exemples de scénarios ci-dessous pour configurer les règles de revendication sur le **plate-forme d’identité MicrosoftOffice 365** de confiance que répond le mieux aux besoins de votre organisation.  
  
###  <a name="scenario1"></a>Scénario 1: Bloquer tout accès externe à Office 365  
 Ce scénario de stratégie d’accès client permet d’accéder à partir de tous les clients internes et les blocs de tous les clients externes basés sur l’adresse IP du client externe. Vous pouvez utiliser les procédures suivantes pour ajouter les règles d’autorisation d’émission appropriées à Office 365 de confiance pour votre scénario choisi.  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Pour créer des règles pour bloquer tout accès externe à Office 365  
  
1.  À partir de **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **AD Fs\relations**, cliquez sur **approbations de partie de confiance**, avec le bouton droit le **plate-forme d’identité MicrosoftOffice 365** approuver, puis cliquez sur **modifier les règles de revendication**.  
  
3.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez le **les règles d’autorisation d’émission** onglet, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication.  
  
4.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  
  
5.  Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle, par exemple «s’il existe des réclamations IP en dehors de la plage souhaitée, interdire». Sous **règle personnalisée**, tapez ou collez la syntaxe du langage règle revendication suivante (remplacez la valeur ci-dessus pour «x-ms-transmis-client-ip» avec une expression IP valide):  
`c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste des règles d’autorisation d’émission avant la valeur par défaut **autoriser l’accès à tous les utilisateurs** règle (la règle de refus est prioritaire même si elle apparaît précédemment dans la liste).  Si vous n’avez pas la valeur par défaut autorise la règle d’accès, vous pouvez ajouter un à la fin de votre liste à l’aide du langage de règle de revendication comme suit:  </br>
    
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Pour enregistrer les nouvelles règles, dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK**. La liste résultante doit se présenter comme suit.  
  
     ![Règles d’autorisation d’émission](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  
  
###  <a name="scenario2"></a>Scénario 2: Bloquer tout accès externe à Office 365 à l’exception d’Exchange ActiveSync  
 L’exemple suivant permet d’accéder à toutes les applications Office 365, notamment Exchange Online à partir des clients internes, y compris Outlook. Il bloque l’accès à partir de clients qui se trouvent en dehors du réseau d’entreprise, tel qu’indiqué par l’adresse IP du client, à l’exception des clients d’Exchange ActiveSync tels que des Smartphones.  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Pour créer des règles pour bloquer tout accès externe à Office 365 à l’exception d’Exchange ActiveSync  
  
1.  À partir de **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **AD Fs\relations**, cliquez sur **approbations de partie de confiance**, avec le bouton droit le **plate-forme d’identité MicrosoftOffice 365** approuver, puis cliquez sur **modifier les règles de revendication**.  
  
3.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez le **les règles d’autorisation d’émission** onglet, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication.  
  
4.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  
  
5.  Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle, pour exemple» s’il existe des réclamations IP en dehors de la plage souhaitée, émettre des revendications ipoutsiderange». Sous **règle personnalisée**, tapez ou collez la syntaxe du langage règle revendication suivante (remplacez la valeur ci-dessus pour «x-ms-transmis-client-ip» avec une expression IP valide):  
    
    `c1:[Type == " https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  
  
7.  Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission**, cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  
  
8.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  
  
9. Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle, par exemple «s’il existe une adresse IP en dehors de la plage souhaitée et il existe une revendication d’application x-ms-client non EAS, interdire». Sous **règle personnalisée**, tapez ou collez la syntaxe du langage de règle revendication suivantes:  
  

    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
  
10. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  
  
11. Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission**, cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  
  
12. Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication,** sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  
  
13. Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle, par exemple «vérifier si la revendication d’application existe». Sous **règle personnalisée**, tapez ou collez la syntaxe du langage de règle revendication suivantes:  
  
    ```  
    NOT EXISTS([Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
    ```  
  
14. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  
  
15. Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission**, cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  
  
16. Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication,** sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  
  
17. Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle, par exemple «interdire aux utilisateurs la valeur true ipoutsiderange et application d’échouer». Sous **règle personnalisée**, tapez ou collez la syntaxe du langage de règle revendication suivantes:  
  
`c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/xmsapplication", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle s’affiche immédiatement sous la règle précédente et avant la valeur par défaut autoriser l’accès à tous les utilisateurs de règles dans la liste des règles d’autorisation d’émission (la règle de refus est prioritaire même si elle apparaît précédemment dans la liste).  </br>Si vous n’avez pas la valeur par défaut autorise la règle d’accès, vous pouvez ajouter un à la fin de votre liste à l’aide du langage de règle de revendication comme suit:</br></br>      `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Pour enregistrer les nouvelles règles, dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur OK. La liste résultante doit se présenter comme suit.  
  
     ![Règles d’autorisation d’émission](media/Access-Control-Policies-W2K12/clientaccess2.png )  
  
###  <a name="scenario3"></a>Scénario 3: Bloquer tout accès externe à Office 365 à l’exception des applications basées sur le navigateur  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Pour créer des règles pour bloquer tout accès externe à Office 365 à l’exception des applications basées sur le navigateur  
  
1.  À partir de **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **AD Fs\relations**, cliquez sur **approbations de partie de confiance**, avec le bouton droit le **plate-forme d’identité MicrosoftOffice 365** approuver, puis cliquez sur **modifier les règles de revendication**.  
  
3.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez le **les règles d’autorisation d’émission** onglet, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication.  
  
4.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  
  
5.  Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle, pour exemple» s’il existe des réclamations IP en dehors de la plage souhaitée, émettre des revendications ipoutsiderange». Sous **règle personnalisée**, tapez ou collez la syntaxe du langage règle revendication suivante (remplacez la valeur ci-dessus pour «x-ms-transmis-client-ip» avec une expression IP valide):  </br>
`c1:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  
  
7.  Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission**, cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  
  
8.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication,** sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  
  
9. Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle, par exemple «s’il existe une adresse IP en dehors de la plage souhaitée et le point de terminaison n’est pas/adfs/ls, interdire». Sous **règle personnalisée**, tapez ou collez la syntaxe du langage de règle revendication suivantes:  
  
 
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
  
10. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste des règles d’autorisation d’émission avant la valeur par défaut **autoriser l’accès à tous les utilisateurs** règle (la règle de refus est prioritaire même si elle apparaît précédemment dans la liste).  </br></br> Si vous n’avez pas la valeur par défaut autorise la règle d’accès, vous pouvez ajouter un à la fin de votre liste à l’aide du langage de règle de revendication comme suit:  
  
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`
  
11. Pour enregistrer les nouvelles règles, dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK**. La liste résultante doit se présenter comme suit.  
  
     ![Émission](media/Access-Control-Policies-W2K12/clientaccess3.png)  
  
###  <a name="scenario4"></a>Scénario 4: Bloquer tout accès externe à Office 365 à l’exception des groupes ActiveDirectory désignés  
 L’exemple suivant permet d’accéder à partir de clients internes basé sur l’adresse IP. Il bloque l’accès à partir de clients résidant en dehors du réseau d’entreprise qui ont une adresse IP de clients externes, à l’exception des personnes dans une ActiveDirectory spécifié Group.Use la procédure suivante pour ajouter les règles d’autorisation d’émission corrects pour le **plate-forme d’identité MicrosoftOffice 365** de confiance à l’aide de l’Assistant règle de revendication:  
  
##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Pour créer des règles pour bloquer tout accès externe à Office 365, à l’exception de désigné groupes ActiveDirectory  
  
1.  À partir de **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **gestion ADFS**.  
  
2.  Dans l’arborescence de la console, sous **AD Fs\relations**, cliquez sur **approbations de partie de confiance**, avec le bouton droit le **plate-forme d’identité MicrosoftOffice 365** approuver, puis cliquez sur **modifier les règles de revendication**.  
  
3.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez le **les règles d’autorisation d’émission** onglet, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication.  
  
4.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication**, sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  
  
5.  Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle, pour exemple» s’il existe des réclamations IP en dehors de la plage souhaitée, émettre des revendications ipoutsiderange.» Sous **règle personnalisée**, tapez ou collez la syntaxe du langage règle revendication suivante (remplacez la valeur ci-dessus pour «x-ms-transmis-client-ip» avec une expression IP valide):  
  
      
    `c1:[Type == "https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  
  
7.  Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission**, cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  
  
8.  Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication,** sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  
  
9. Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle, par exemple «vérifier les SID de groupe». Sous **règle personnalisée**, tapez ou collez la syntaxe du langage règle revendication suivante (remplacez «SIDgroupe» avec le SID du groupe ActiveDirectory que vous utilisez):  
   
    `NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  
  
11. Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission**, cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  
  
12. Sur le **sélectionner le modèle de règle** sous **modèle de règle de revendication,** sélectionnez **envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  
  
13. Sur le **configurer la règle** sous **nom de règle de revendication**, tapez le nom d’affichage pour cette règle, par exemple «interdire aux utilisateurs la valeur true ipoutsiderange et SIDgroupe échouent». Sous **règle personnalisée**, tapez ou collez la syntaxe du langage de règle revendication suivantes:  
   
    `c1:[Type == "https://custom/ipoutsiderange", Value == "true"] && c2:[Type == "https://custom/groupsid", Value == "fail"] => issue(Type = "https://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle s’affiche immédiatement sous la règle précédente et avant la valeur par défaut autoriser l’accès à tous les utilisateurs de règles dans la liste des règles d’autorisation d’émission (la règle de refus est prioritaire même si elle apparaît précédemment dans la liste).  </br></br>Si vous n’avez pas la valeur par défaut autorise la règle d’accès, vous pouvez ajouter un à la fin de votre liste à l’aide du langage de règle de revendication comme suit:  
   
    `c:[] => issue(Type = "https://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Pour enregistrer les nouvelles règles, dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur OK. La liste résultante doit se présenter comme suit.  
  
     ![Émission](media/Access-Control-Policies-W2K12/clientaccess4.png)  
  
##  <a name="buildingip"></a>Création de l’expression de plage d’adresses IP  
 La revendication x-ms-transmis-client-ip est complétée à partir d’un en-tête HTTP qui est actuellement défini par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour ADFS. La valeur de la revendication peut être une des opérations suivantes:  
  
> [!NOTE]
>  Exchange Online prend actuellement en charge uniquement les adresses IPV4 et IPV6 pas.  
  
-   Une seule adresse IP: adresse IP du client qui est connecté directement à Exchange Online  
  
> [!NOTE]
>  -   L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme adresse IP externe d’interface de passerelle ou de proxy sortantes de l’organisation.  
> -   Les clients qui sont connectés au réseau d’entreprise via un VPN ou par MicrosoftDirectAccess (DA) peuvent apparaître en tant que clients d’entreprise internes ou externes clients en fonction de la configuration du VPN ou DA.  
  
-   Une ou plusieurs adresses IP: lorsque Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur en fonction de la valeur de l’en-tête x transmis pour, un en-tête non standard qui peut être inclus dans les demandes HTTP et est pris en charge par de nombreux clients, les équilibreurs de charge et les proxys sur le marché.  
  
> [!NOTE]
>  1.  Plusieurs adresses IP, ce qui indique l’adresse IP du client et l’adresse de chaque serveur proxy qui a transmis la demande, seront séparées par une virgule.  
> 2.  Les adresses IP liés à l’infrastructure Exchange Online ne seront pas dans la liste.  
  
### <a name="regular-expressions"></a>Expressions régulières  
 Lorsque vous disposez correspondre à une plage d’adresses IP, il devient nécessaire construire une expression régulière pour effectuer la comparaison. Dans la prochaine série d’étapes, nous fournirons des exemples pour savoir comment construire une telle expression pour faire correspondre les plages d’adresses suivantes (Notez que vous devrez modifier ces exemples pour faire correspondre votre plage IP public):  
  
-   192.168.1.1 – 192.168.1.25  
  
-   10.0.0.1 – 10.0.0.14  
  
 Tout d’abord, le modèle de base correspondant à une seule adresse IP est le suivant: \b###\\.###\\.###\\.###\b  
  
 L’extension de cela, nous pouvons correspondent à deux adresses IP avec une expression ou comme suit: \b###\\.###\\.###\\.###\b et #124;\b###\\.###\\.###\\.###\b  
  
 Par conséquent, un exemple pour correspondre à seulement deux adresses (comme 192.168.1.1 ou 10.0.0.1) serait: \b192\\.168\\.1\\.1\b et #124;\b10\\.0\\.0\\.1\b  
  
 Cela vous donne la technique par laquelle vous pouvez entrer n’importe quel nombre d’adresses. Lorsqu’une plage d’adresses doivent être autorisée, par exemple 192.168.1.1 – 192.168.1.25, la mise en correspondance doit être réalisée caractère par caractère: \b192\\.168\\.1\\. ([1-9] et #124; 1 [0-9] et #124; 2 [0-5]) \b  
  
 Notez les points suivants:  
  
-   L’adresse IP est considérée comme chaîne et non un nombre.  
  
-   La règle est répartie comme suit: \b192\\.168\\.1\\.  
  
-   Cela correspond à n’importe quel 192.168.1 à compter de valeur.  
  
-   Les plages requis pour la partie de l’adresse après la virgule décimale finale correspond aux éléments suivants:  
  
    -   ([1-9] correspond à des adresses se terminant par 1-9  
  
    -   & #124; 1 [0-9] correspond à 10-19 se terminant par des adresses  
  
    -   & adresses de correspondances #124;2[0-5]) se terminant par 20-25  
  
-   Notez que les parenthèses doivent être positionnés correctement, afin que vous ne démarrez pas correspondance d’autres parties d’adresses IP.  
  
-   Avec le bloc de 192mis en correspondance, nous pouvons écrire une expression semblable pour le bloc de 10: \b10\\.0\\.0\\. ([1-9] et #124; 1 [0-4]) \b  
  
-   Et les assembler, l’expression suivante doit correspondre à toutes les adresses de «192.168.1.1~25» et «10.0.0.1~14»: \b192\\.168\\.1\\. ([1-9] et #124; 1 [0-9] et #124; 2 [0-5]) \b et #124;\b10\\.0\\.0\\. ([1-9] et #124; 1 [0-4]) \b  
  
### <a name="testing-the-expression"></a>L’Expression de test  
 Expressions Regex peuvent devenir très difficile, nous vous recommandons vivement d’à l’aide d’un outil de vérification de regex. Si vous effectuez une recherche sur internet pour «générateur d’expressions regex en ligne», vous trouverez plusieurs utilitaires en ligne bons qui vous permet de tester vos expressions contre les exemples de données.  
  
 Lorsque vous testez l’expression, il est important de comprendre à quoi s’attendre à correspondre à. Le système en ligne Exchange peut envoyer le nombre d’adresses IP, séparé par des virgules. Les expressions ci-dessus fonctionnera pour cela. Toutefois, il est important d’aborder ce sujet lors du test de vos expressions de regex. Par exemple, un peut utiliser l’exemple suivant d’entrée pour vérifier les exemples ci-dessus:  
  
 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  
  
 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  
  
## <a name="claim-types"></a>Types de revendication  
 ADFS dans Windows Server2012R2 fournit des informations de contexte de demande à l’aide de types de revendications suivants:  
  
### <a name="x-ms-forwarded-client-ip"></a>X-MS-transmis-Client-IP  
 Type de revendication: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  
  
 Cette revendication ADFS représente un «mieux pour tenter «permettant de vérifier l’adresse IP de l’utilisateur (par exemple, le client Outlook) effectue la demande. Cette revendication peut contenir plusieurs adresses IP, y compris l’adresse de chaque serveur proxy qui a transmis la demande.  Cette revendication est complétée à partir de HTTP. La valeur de la revendication peut être une des opérations suivantes:  
  
-   Une seule adresse IP: adresse IP du client qui est connecté directement à Exchange Online  
  
> [!NOTE]
>  L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme adresse IP externe d’interface de passerelle ou de proxy sortantes de l’organisation.  
  
-   Une ou plusieurs adresses IP  
  
    -   Si Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur en fonction de la valeur de l’en-tête x transmis pour un en-tête non standard qui peut être inclus dans le protocole HTTP sur demande et est pris en charge par de nombreux clients, les équilibreurs de charge et les proxys sur le marché.  
  
    -   Plusieurs adresses IP qui indique l’adresse IP du client et l’adresse de chaque serveur proxy qui a transmis la demande seront séparées par une virgule.  
  
> [!NOTE]
>  Les adresses IP liés à l’infrastructure Exchange Online ne sera pas présents dans la liste.  
  
> [!WARNING]
>  Exchange Online prend actuellement en charge uniquement les adresses IPV4. Il ne prend pas en charge les adresses IPV6.  
  
### <a name="x-ms-client-application"></a>Application X-MS-Client  
 Type de revendication: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  
  
 Cette revendication ADFS représente le protocole utilisé par le client, qui correspond librement à l’application en cours d’utilisation.  Cette revendication est remplie à partir d’un en-tête HTTP qui n’est actuellement défini uniquement à Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour ADFS. En fonction de l’application, la valeur de cette revendication sera une des opérations suivantes:  
  
-   Dans le cas des périphériques qui utilisent Exchange Active Sync, la valeur est Microsoft.Exchange.ActiveSync.  
  
-   Utilisation du client MicrosoftOutlook peut provoquer une des valeurs suivantes:  
  
    -   Microsoft.Exchange.Autodiscover  
  
    -   Microsoft.Exchange.OfflineAddressBook  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  
  
-   Les autres valeurs possibles pour cet en-tête sont les suivantes:  
  
    -   Microsoft.Exchange.Powershell  
  
    -   Microsoft.Exchange.SMTP  
  
    -   Microsoft.Exchange.Pop  
  
    -   Microsoft.Exchange.Imap  
  
### <a name="x-ms-client-user-agent"></a>X-MS-Client-Agent utilisateur  
 Type de revendication: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  
  
 Cette revendication ADFS fournit une chaîne représentant le type de périphérique que le client utilise pour accéder au service. Cela peut servir lorsque les clients souhaitez empêcher l’accès pour certains appareils (tels que des Smartphones particuliers).  Exemples de valeurs pour cette revendication inclure (mais ne sont pas limités à) les valeurs ci-dessous.  
  
 Le vous trouverez ci-dessous des exemples de la valeur x-ms-user-agent qui peut contenir pour un client dont x-ms-application cliente est «Microsoft.Exchange.ActiveSync»  
  
-   Tourbillon/1.0  
  
-   Apple-iPad1C1/812.1  
  
-   Apple-iPhone3C1/811.2  
  
-   Apple-iPhone/704.11  
  
-   Moto-DROID2/4.5.1  
  
-   SAMSUNGSPHD700/100.202  
  
-   Android/0,3  
  
 Il est également possible que cette valeur est vide.  
  
### <a name="x-ms-proxy"></a>X-MS-Proxy  
 Type de revendication: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  
  
 Cette revendication ADFS indique que la demande a passé par le biais du proxy d’Application Web.  Cette revendication est remplie par le proxy d’Application Web, qui remplit l’en-tête lors du passage de la demande d’authentification pour le Service de fédération principal. ADFS puis le convertit en une revendication.  
  
 La valeur de la revendication est le nom DNS du proxy d’Application Web qui a transmis la demande.  
  
### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Type de revendication: `https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  
  
 Type de revendication similaire pour le proxy x-ms ci-dessus, ce type de revendication indique si la demande a passé par le biais du proxy d’application web. Contrairement à x-ms-proxy insidecorporatenetwork est une valeur booléenne True indiquant une demande directement vers le service de fédération à l’intérieur du réseau d’entreprise à partir de.  
  
### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-point de terminaison-absolue-chemin d’accès (actif et passif)  
 Type de revendication: `https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  
  
 Ce type de revendication peut être utilisé pour déterminer les demandes provenant de clients (riches) «actives» et «passifs» (web-clients sur navigateur). Ainsi, les requêtes externes à partir des applications de navigateur comme Outlook Web Access, SharePoint Online ou le portail Office 365 à autoriser tandis que les demandes provenant de clients complexes telles que MicrosoftOutlook sont bloqués.  
  
 La valeur de la revendication est le nom du service ADFS qui a reçu la demande.  
  
## <a name="see-also"></a>Voir aussi  
 [Opérations ADFS](../../ad-fs/AD-FS-2016-Operations.md)
