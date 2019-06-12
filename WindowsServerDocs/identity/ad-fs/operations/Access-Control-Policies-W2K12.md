---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: Stratégies de contrôle d’accès dans ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc3de72aa993546151b103d6eeacd160f81a4270
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444382"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Stratégies de contrôle d’accès dans Windows Server 2012 R2 et Windows Server 2012 AD FS


Vérifiez les stratégies décrites dans cet article utilisent deux types de revendications  

1.  Des revendications QU'AD FS crée avec les informations du proxy AD FS et l’Application Web peut inspecter et vérifier, telles que l’adresse IP du client se connectant directement à AD FS ou WAP.  

2.  Revendications AD FS crée en fonction des informations communiquées à AD FS par le client en tant qu’en-têtes HTTP  

>**Important** : Les stratégies, comme indiqué ci-dessous bloquera jonction de domaine Windows 10 et les scénarios qui requièrent l’accès aux points de terminaison supplémentaires suivantes pour l’ouverture de session

Points de terminaison AD FS requis pour la jonction de domaine Windows 10 et connectez-vous sur
- [nom du service de fédération] / adfs/services/trust/2005/windowstransport
- [nom du service de fédération] / adfs/services/trust/13/windowstransport
- [nom du service de fédération] / adfs/services/trust/2005/usernamemixed.
- [nom du service de fédération] / adfs/services/trust/13/usernamemixed. 
- [nom du service de fédération] / adfs/services/trust/2005/certificatemixed
- [nom du service de fédération] / adfs/services/trust/13/certificatemixed

Pour résoudre, mettez à jour des stratégies qui refusent basées sur la revendication de point de terminaison pour autoriser l’exception des points de terminaison ci-dessus.

Par exemple, la règle ci-dessous :

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

Voulez-vous mettre à jour :

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Revendications à partir de cette catégorie doivent uniquement être utilisées pour implémenter des stratégies d’entreprise et non comme des stratégies de sécurité pour protéger l’accès à votre réseau.  Il est possible pour les clients non autorisés à envoyer des en-têtes de fausses informations comme un moyen d’accéder.  

Les stratégies décrites dans cet article doivent toujours être utilisées avec une autre méthode d’authentification, comme nom d’utilisateur et mot de passe ou multi-factor authentication.  

## <a name="client-access-policies-scenarios"></a>Scénarios de stratégies d’accès client  

|**Scénario**|**Description**| 
| --- | --- | 
|Scénario 1 : Bloquer tous les accès externes à Office 365|Accès à Office 365 est autorisée à partir de tous les clients sur le réseau d’entreprise interne, mais les demandes des clients externes sont refusées en fonction de l’adresse IP du client externe.|  
|Scénario 2 : Bloquer tous les accès externes à Office 365 à l’exception d’Exchange ActiveSync|Accès à Office 365 est autorisée à partir de tous les clients sur le réseau d’entreprise interne, ainsi qu’à partir de tous les appareils clients externes, tels que des téléphones intelligents qui utilisent Exchange ActiveSync. Tous les autres clients externes, tels que ceux à l’aide d’Outlook, sont bloquées.|  
|Scénario 3 : Bloquer tous les accès externes à Office 365 à l’exception des applications basées sur le navigateur|Bloque l’accès externe à Office 365, à l’exception des applications passives (basée sur le navigateur) telles que Outlook Web Access ou SharePoint Online.|  
|Scénario 4 : Bloquer tous les accès externes à Office 365 à l’exception des groupes Active Directory désignés|Ce scénario est utilisé pour le test et validation du déploiement de stratégie d’accès client. Il bloque l’accès externe à Office 365 uniquement pour les membres d’un ou plusieurs groupes Active Directory. Il peut également servir à fournir un accès externe uniquement aux membres d’un groupe.|  

## <a name="enabling-client-access-policy"></a>Activation de la stratégie d’accès Client  
 Pour activer la stratégie d’accès client dans AD FS dans Windows Server 2012 R2, vous devez mettre à jour de la plateforme d’identité Microsoft Office 365 de confiance. Choisissez un des exemples de scénarios ci-dessous pour configurer les règles de revendication sur le **plateforme d’identité Microsoft Office 365** de confiance que les besoins de votre organisation.  

###  <a name="scenario1"></a> Scénario 1 : Bloquer tous les accès externes à Office 365  
 Ce scénario de stratégie d’accès client autorise l’accès à partir de tous les clients internes et les blocs de tous les clients externes, en fonction de l’adresse IP du client externe. Vous pouvez utiliser les procédures suivantes pour ajouter les règles d’autorisation d’émission appropriées à Office 365 de confiance pour votre scénario choisi.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Pour créer des règles pour bloquer tous les accès externes à Office 365  

1.  À partir de **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **gestion AD FS**.  

2.  Dans l’arborescence de la console, sous **AD Fs\relations**, cliquez sur **confiance**, avec le bouton droit le **plateforme d’identité Microsoft Office 365** confiance, puis cliquez sur **Modifier les règles de revendication**.  

3.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez le **les règles d’autorisation d’émission** onglet, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication.  

4.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

5.  Sur le **configurer la règle** page sous **nom de règle de revendication**, le nom complet de cette règle, par exemple « s’il existe toute revendication d’adresse IP en dehors de la plage souhaitée, interdire » de type. Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle (à remplacer la valeur ci-dessus pour « x-ms-transférés-client-ip » avec une expression d’adresse IP valide) revendication suivante :  
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste des règles d’autorisation d’émission avant la valeur par défaut **autoriser l’accès à tous les utilisateurs** règle (la règle de refus est prioritaire même s’il semble plus haut dans la liste).  Si vous n’avez pas la valeur par défaut autorisent la règle d’accès, vous pouvez ajouter une à la fin de votre liste à l’aide du langage de règle de revendication comme suit :  </br>

    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Pour enregistrer les nouvelles règles, dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK**. La liste résultante doit ressembler à ce qui suit.  

     ![Règles d’autorisation d’émission](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1")  

###  <a name="scenario2"></a> Scénario 2 : Bloquer tous les accès externes à Office 365 à l’exception d’Exchange ActiveSync  
 L’exemple suivant autorise l’accès à toutes les applications Office 365, notamment Exchange Online, à partir de clients internes, notamment Outlook. Il bloque l’accès à partir de clients résidant en dehors du réseau d’entreprise, comme indiqué par l’adresse IP de client, à l’exception des clients Exchange ActiveSync tels que des téléphones intelligents.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Pour créer des règles pour bloquer tous les accès externes à Office 365 à l’exception d’Exchange ActiveSync  

1.  À partir de **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **gestion AD FS**.  

2.  Dans l’arborescence de la console, sous **AD Fs\relations**, cliquez sur **confiance**, avec le bouton droit le **plateforme d’identité Microsoft Office 365** confiance, puis cliquez sur **Modifier les règles de revendication**.  

3.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez le **les règles d’autorisation d’émission** onglet, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication.  

4.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

5.  Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet de cette règle, pour l’exemple » s’il existe toute revendication d’adresse IP en dehors de la plage souhaitée, émettre ipoutsiderange revendication ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle (à remplacer la valeur ci-dessus pour « x-ms-transférés-client-ip » avec une expression d’adresse IP valide) revendication suivante :  

    `c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  

7.  Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission** , cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  

8.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

9. Sur le **configurer la règle** page sous **nom de règle de revendication**, le nom complet de cette règle, par exemple « s’il existe une adresse IP en dehors de la plage souhaitée et une revendication de x-ms-client-application non EAS, refuser de type «. Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivants :  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
~~~

10. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  

11. Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission** , cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  

12. Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication,** sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

13. Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet pour cette règle, par exemple « vérifier s’il existe des revendications d’application ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivants :  

   ```  
   NOT EXISTS([Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
   ```  

14. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  

15. Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission** , cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  

16. Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication,** sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

17. Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet pour cette règle, par exemple « interdire les utilisateurs avec la valeur true ipoutsiderange et application échouent ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivants :  

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît immédiatement sous la règle précédente et avant la valeur par défaut autoriser l’accès à tous les utilisateurs de règle dans la liste de règles d’autorisation d’émission (la règle de refus est prioritaire même s’il semble plus haut dans la liste).  </br>Si vous n’avez pas la valeur par défaut autorisent la règle d’accès, vous pouvez ajouter une à la fin de votre liste à l’aide du langage de règle de revendication comme suit :</br></br>      `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Pour enregistrer les nouvelles règles, dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur OK. La liste résultante doit ressembler à ce qui suit.  

    ![Règles d'autorisation d'émission](media/Access-Control-Policies-W2K12/clientaccess2.png )  

###  <a name="scenario3"></a> Scénario 3 : Bloquer tous les accès externes à Office 365 à l’exception des applications basées sur le navigateur  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Pour créer des règles pour bloquer tous les accès externes à Office 365 à l’exception des applications basées sur le navigateur  

1.  À partir de **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **gestion AD FS**.  

2.  Dans l’arborescence de la console, sous **AD Fs\relations**, cliquez sur **confiance**, avec le bouton droit le **plateforme d’identité Microsoft Office 365** confiance, puis cliquez sur **Modifier les règles de revendication**.  

3.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez le **les règles d’autorisation d’émission** onglet, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication.  

4.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

5.  Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet de cette règle, pour l’exemple » s’il existe toute revendication d’adresse IP en dehors de la plage souhaitée, émettre ipoutsiderange revendication ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle (à remplacer la valeur ci-dessus pour « x-ms-transférés-client-ip » avec une expression d’adresse IP valide) revendication suivante :  </br>
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  

7.  Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission** , cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  

8.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication,** sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

9. Sur le **configurer la règle** page sous **nom de règle de revendication**, le nom complet de cette règle, par exemple « s’il existe une adresse IP en dehors de la plage souhaitée et le point de terminaison n’est pas/adfs/ls, interdire » de type. Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivants :  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
~~~

10. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste des règles d’autorisation d’émission avant la valeur par défaut **autoriser l’accès à tous les utilisateurs** règle (la règle de refus est prioritaire même s’il semble plus haut dans la liste).  </br></br> Si vous n’avez pas la valeur par défaut autorisent la règle d’accès, vous pouvez ajouter une à la fin de votre liste à l’aide du langage de règle de revendication comme suit :  

   `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`

11. Pour enregistrer les nouvelles règles, dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur **OK**. La liste résultante doit ressembler à ce qui suit.  

    ![Émission](media/Access-Control-Policies-W2K12/clientaccess3.png)  

###  <a name="scenario4"></a> Scénario 4 : Bloquer tous les accès externes à Office 365 à l’exception des groupes Active Directory désignés  
 L’exemple suivant permet d’accéder à partir de clients internes en fonction de l’adresse IP. Il bloque l’accès à partir de clients résidant en dehors du réseau d’entreprise qui ont une adresse IP de client externe, sauf pour les personnes dans un Group.Use Directory Active spécifiée comme suit pour ajouter l’autorisation d’émission correcte de règles à la  **Plateforme d’identité Microsoft Office 365** de confiance à l’aide de l’Assistant règle de revendication :  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Pour créer des règles pour bloquer tous les accès externes à Office 365, à l’exception de désigné groupes Active Directory  

1.  À partir de **le Gestionnaire de serveur**, cliquez sur **outils**, puis cliquez sur **gestion AD FS**.  

2.  Dans l’arborescence de la console, sous **AD Fs\relations**, cliquez sur **confiance**, avec le bouton droit le **plateforme d’identité Microsoft Office 365** confiance, puis cliquez sur **Modifier les règles de revendication**.  

3.  Dans le **modifier les règles de revendication** boîte de dialogue, sélectionnez le **les règles d’autorisation d’émission** onglet, puis cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication.  

4.  Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication**, sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

5.  Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet de cette règle, pour l’exemple » s’il existe toute revendication d’adresse IP en dehors de la plage souhaitée, émettre ipoutsiderange revendication. » Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle (à remplacer la valeur ci-dessus pour « x-ms-transférés-client-ip » avec une expression d’adresse IP valide) revendication suivante :  


~~~
`c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  
~~~

6. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  

7. Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission** , cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  

8. Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication,** sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

9. Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet pour cette règle, par exemple « vérifier les SID de groupe ». Sous **règle personnalisée**, tapez ou collez la syntaxe du langage règle revendication suivante (remplacez « groupsid » par le SID du groupe AD que vous utilisez) :  

    `NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans le **les règles d’autorisation d’émission** liste.  

11. Ensuite, dans le **modifier les règles de revendication** boîte de dialogue le **les règles d’autorisation d’émission** , cliquez sur **ajouter une règle** pour démarrer l’Assistant règle de revendication à nouveau.  

12. Sur le **sélectionner le modèle de règle** page sous **modèle de règle de revendication,** sélectionnez **envoyer les revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

13. Sur le **configurer la règle** page sous **nom de règle de revendication**, tapez le nom complet pour cette règle, par exemple « interdire aux utilisateurs avec la valeur true ipoutsiderange et groupsid échouent ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivants :  

   `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît immédiatement sous la règle précédente et avant la valeur par défaut autoriser l’accès à tous les utilisateurs de règle dans la liste de règles d’autorisation d’émission (la règle de refus est prioritaire même s’il semble plus haut dans la liste).  </br></br>Si vous n’avez pas la valeur par défaut autorisent la règle d’accès, vous pouvez ajouter une à la fin de votre liste à l’aide du langage de règle de revendication comme suit :  

   `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Pour enregistrer les nouvelles règles, dans le **modifier les règles de revendication** boîte de dialogue, cliquez sur OK. La liste résultante doit ressembler à ce qui suit.  

     ![Émission](media/Access-Control-Policies-W2K12/clientaccess4.png)  

##  <a name="buildingip"></a> Création de l’expression de plage d’adresses IP  
 La revendication de x-ms-transférés-client-ip est remplie à partir d’un en-tête HTTP qui est actuellement configuré par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour AD FS. La valeur de la revendication peut être une des opérations suivantes :  

> [!NOTE]
>  Exchange Online prend en charge uniquement les adresses IPV4 et IPV6 pas.  

-   Une seule adresse IP : L’adresse IP du client qui est directement connecté à Exchange Online  

> [!NOTE]
> - L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme adresse IP externe d’interface de passerelle ou proxy sortant de l’organisation.  
>   -   Les clients qui sont connectés au réseau d’entreprise via un VPN ou par Microsoft DirectAccess (DA) peuvent apparaître en tant que clients d’entreprise internes ou en tant que clients externes, en fonction de la configuration de VPN ou DA.  

-   Une ou plusieurs adresses IP : Lorsque Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur basée sur la valeur de l’en-tête x-forwarded-for, un en-tête non standard qui peut être inclus dans les demandes HTTP et est pris en charge par le nombre de clients, les équilibreurs de charge et proxys sur le marché.  

> [!NOTE]
> 1. Plusieurs adresses IP, indiquant l’adresse IP du client et l’adresse de chaque proxy qui a transmis la demande, doivent être séparés par une virgule.  
>    2. Les adresses IP liées à l’infrastructure Exchange Online ne seront pas dans la liste.  

### <a name="regular-expressions"></a>Expressions régulières  
 Lorsque vous devez faire correspondre une plage d’adresses IP, il devient nécessaire construire une expression régulière pour effectuer la comparaison. Dans la prochaine série d’étapes, nous vous proposons des exemples pour savoir comment construire une telle expression pour faire correspondre les plages d’adresses suivante (Notez que vous devrez modifier ces exemples pour correspondre à votre plage IP publique) :  

- 192.168.1.1 – 192.168.1.25  

- 10.0.0.1 – 10.0.0.14  

  Tout d’abord, le modèle de base qui correspond à une seule adresse IP est la suivante : \b###\\. ###\\. ###\\. ### \b  

  Cette extension, nous pouvons correspond à deux adresses IP différentes avec une expression OR comme suit : \b###\\. ###\\. ###\\. ### \b&#124;\b###\\. ###\\. ###\\. ### \b  

  Par conséquent, un exemple pour faire correspondre deux adresses (par exemple, 192.168.1.1 ou 10.0.0.1) serait : \b192\\.168\\.1\\. 1\b&#124;\b10\\.0\\.0\\. 1\b  

  Cela vous donne la technique par laquelle vous pouvez entrer n’importe quel nombre d’adresses. Où une plage d’adresses doivent être autorisées, par exemple 192.168.1.1 – 192.168.1.25, la correspondance doit être effectuée caractère par caractère : \b192\\.168\\.1\\. () [1-9] &#124;1 [0-9]&#124;2 [0-5]) \b  

  Notez les points suivants :  

- L’adresse IP est traité en tant que chaîne et pas un nombre.  

- La règle est répartie comme suit : \b192\\.168\\.1\\.  

- Cela correspond à n’importe quel valeurs commençant par 192.168.1.  

- Les plages requis pour la partie de l’adresse après la virgule décimale finale correspond à ce qui suit :  

  -   ([1-9] correspond à des adresses se terminant par 1-9  

  -   &#124;1 [0-9] correspond à des adresses se terminant par 10-19  

  -   &#124;Adresses de correspondances 2[0-5]) se terminant par 20-25  

- Notez que les parenthèses doivent être positionnés correctement, afin que vous ne démarrez pas autres parties d’adresses IP correspondantes.  

- Avec le bloc 192 mis en correspondance, nous pouvons écrire une expression similaire pour le bloc de 10 : \b10\\.0\\.0\\. () [1-9] &#124;1 [0-4]) \b  

- Et en les plaçant entre eux, l’expression suivante doit correspondre à toutes les adresses pour « 192.168.1.1~25 » et « 10.0.0.1~14 » : \b192\\.168\\.1\\. () [1-9] &#124;1 [0-9]&#124;2 [0-5]) \b&#124;\b10\\.0\\.0\\. () [1-9] &#124;1 [0-4]) \b  

### <a name="testing-the-expression"></a>L’Expression de test  
 Expressions d’expression régulière peuvent devenir assez délicate, nous vous recommandons fortement d’à l’aide d’un outil de vérification d’expression régulière. Si vous effectuez une recherche sur internet pour « générateur d’expressions regex en ligne », vous trouverez plusieurs utilitaires en ligne bon qui vous permettra de tester vos expressions par rapport à des exemples de données.  

 Lorsque vous testez l’expression, il est important de comprendre à quoi s’attendre à doivent correspondre. Le système en ligne Exchange peut envoyer le nombre d’adresses IP, séparée par des virgules. Les expressions fournies ci-dessus fonctionne pour cela. Toutefois, il est important de réfléchir à ce sujet lorsque vous testez vos expressions regex. Par exemple, il peut utiliser l’exemple suivant d’entrée pour vérifier les exemples ci-dessus :  

 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  

 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10,0.0.1  

## <a name="claim-types"></a>Types de revendications  
 AD FS dans Windows Server 2012 R2 fournit des informations de contexte de demande à l’aide des types de revendications suivants :  

### <a name="x-ms-forwarded-client-ip"></a>X-MS-Forwarded-Client-IP  
 Type de revendication : `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  

 Cette revendication AD FS représente une « meilleure tentative » permettant de vérifier l’adresse IP de l’utilisateur (par exemple, le client Outlook) qui effectue la requête. Cette revendication peut contenir plusieurs adresses IP, y compris l’adresse de chaque serveur proxy qui a transféré la demande.  Cette revendication est remplie à partir de HTTP. La valeur de la revendication peut être une des opérations suivantes :  

-   Une seule adresse IP - adresse IP du client qui est directement connecté à Exchange Online  

> [!NOTE]
>  L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme adresse IP externe d’interface de passerelle ou proxy sortant de l’organisation.  

-   Une ou plusieurs adresses IP  

    -   Si Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur basée sur la valeur de l’en-tête x-forwarded-for, un en-tête non standard qui peut être inclus dans basées sur demande et est pris en charge par nombreux clients, les équilibreurs de charge, et proxys sur le marché.  

    -   Plusieurs adresses IP indiquant l’adresse IP du client et l’adresse de chaque proxy qui a transmis la demande doivent être séparés par une virgule.  

> [!NOTE]
>  Les adresses IP liées à l’infrastructure Exchange Online ne seront pas présents dans la liste.  

> [!WARNING]
>  Exchange Online prend actuellement en charge uniquement les adresses IPV4. Il ne prend pas en charge les adresses IPV6.  

### <a name="x-ms-client-application"></a>X-MS-Client-Application  
 Type de revendication : `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  

 Cette revendication AD FS représente le protocole utilisé par le client final, qui correspond étroitement à l’application utilisée.  Cette revendication est remplie à partir d’un en-tête HTTP qui est actuellement définies uniquement par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification pour AD FS. Selon l’application, la valeur de cette revendication sera une des opérations suivantes :  

-   Dans le cas des appareils qui utilisent Exchange Active Sync, la valeur est Microsoft.Exchange.ActiveSync.  

-   Utilisation du client Microsoft Outlook peut entraîner une des valeurs suivantes :  

    -   Microsoft.Exchange.Autodiscover  

    -   Microsoft.Exchange.OfflineAddressBook  

    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  

    -   Microsoft.Exchange.RPCMicrosoft.Exchange.WebServices  

-   Les autres valeurs possibles pour cet en-tête sont les suivantes :  

    -   Microsoft.Exchange.Powershell  

    -   Microsoft.Exchange.SMTP  

    -   Microsoft.Exchange.Pop  

    -   Microsoft.Exchange.Imap  

### <a name="x-ms-client-user-agent"></a>X-MS-Client-User-Agent  
 Type de revendication : `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  

 Cette revendication AD FS fournit une chaîne représentant le type d’appareil que le client utilise pour accéder au service. Cela peut servir lorsque les clients souhaitent empêcher l’accès pour certains périphériques (tels que les types particuliers de téléphones intelligents).  Exemples de valeurs pour cette revendication incluent (mais ne sont pas limitées à) les valeurs ci-dessous.  

 Le Voici des exemples de ce que peut contenir la valeur de x-ms-user-agent pour un client dont x-ms-application cliente est « Microsoft.Exchange.ActiveSync »  

- Tourbillon/1.0  

- Apple-iPad1C1/812.1  

- Apple-iPhone3C1/811.2  

- Apple-iPhone/704.11  

- Moto-DROID2/4.5.1  

- SAMSUNGSPHD700/100.202  

- Android/0.3  

  Il est également possible que cette valeur est vide.  

### <a name="x-ms-proxy"></a>X-MS-Proxy  
 Type de revendication : `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  

 Cette revendication AD FS indique que la demande est passée par le proxy d’Application Web.  Cette revendication est remplie par le proxy d’Application Web, qui remplit l’en-tête lors du passage de la demande d’authentification au serveur principal de Service de fédération. AD FS puis la convertit en une revendication.  

 La valeur de la revendication est le nom DNS du proxy d’Application Web qui a transmis la demande.  

### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Type de revendication : `http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  

 Type de revendication similaire au x-ms-proxy ci-dessus, ce type de revendication indique si la demande a réussi via le proxy d’application web. Contrairement à x-ms-proxy insidecorporatenetwork est une valeur booléenne avec True indiquant une demande directement au service de fédération à l’intérieur du réseau d’entreprise.  

### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-point de terminaison-absolue-chemin d’accès (actif ou passif)  
 Type de revendication : `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  

 Ce type de revendication peut être utilisé pour déterminer les demandes provenant des clients (complets) « actifs » et « passifs » (web-clients sur navigateur). Ainsi, les requêtes externes à partir des applications de navigateur telles que Outlook Web Access, SharePoint Online ou le portail Office 365 pour être autorisée tandis que les demandes provenant des clients riches tels que Microsoft Outlook sont bloquées.  

 La valeur de la revendication est le nom du service AD FS qui a reçu la demande.  

## <a name="see-also"></a>Voir aussi  
 [Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
