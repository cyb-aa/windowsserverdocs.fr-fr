---
ms.assetid: 5728847d-dcef-4694-9080-d63bfb1fe24b
title: Stratégies de contrôle d’accès dans ADFS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 517582661374c388d44362538da6933a916b0039
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407759"
---
# <a name="access-control-policies-in-windows-server-2012-r2-and-windows-server-2012-ad-fs"></a>Stratégies de Access Control dans Windows Server 2012 R2 et Windows Server 2012 AD FS


Les stratégies décrites dans cet article font appel à deux types de revendications.  

1.  Les revendications AD FS créées en fonction des informations que le AD FS et le proxy d’application Web peuvent inspecter et vérifier, telles que l’adresse IP du client qui se connecte directement à AD FS ou au WAP.  

2.  Les revendications AD FS créées en fonction des informations transmises à AD FS par le client en tant qu’en-têtes HTTP  

>**Important**: les stratégies comme indiqué ci-dessous bloquent les scénarios de jonction de domaine Windows 10 et de connexion qui requièrent l’accès aux points de terminaison supplémentaires suivants

AD FS points de terminaison requis pour la jonction de domaine et la connexion Windows 10
- [nom du service de Fédération]/ADFS/Services/Trust/2005/windowstransport
- [nom du service de Fédération]/ADFS/Services/Trust/13/windowstransport
- [nom du service de Fédération]/ADFS/Services/Trust/2005/usernamemixed
- [nom du service de Fédération]/ADFS/Services/Trust/13/usernamemixed 
- [nom du service de Fédération]/ADFS/Services/Trust/2005/certificatemixed
- [nom du service de Fédération]/ADFS/Services/Trust/13/certificatemixed

>**Important**: les points de terminaison/adfs/services/Trust/2005/windowstransport et/ADFS/Services/Trust/13/windowstransport doivent être activés uniquement pour l’accès intranet, car ils sont destinés à être des points de terminaison intranet qui utilisent la liaison WIA sur HTTPS. Les exposer à l’extranet pourraient permettre à ces points de terminaison de contourner les protections de verrouillage. Ces points de terminaison doivent être désactivés sur le proxy (c’est-à-dire désactivés de l’extranet) pour protéger le verrouillage de compte Active Directory. 

Pour résoudre le, mettez à jour toutes les stratégies qui refusent en fonction de la revendication de point de terminaison pour autoriser l’exception pour les points de terminaison ci-dessus.

Par exemple, la règle ci-dessous :

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  

doit être mis à jour vers :

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "(/adfs/ls/)|(/adfs/services/trust/2005/windowstransport)|(/adfs/services/trust/13/windowstransport)|(/adfs/services/trust/2005/usernamemixed)|(/adfs/services/trust/13/usernamemixed)|(/adfs/services/trust/2005/certificatemixed)|(/adfs/services/trust/13/certificatemixed)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`



> [!NOTE]
>  Les revendications de cette catégorie ne doivent être utilisées que pour implémenter des stratégies d’entreprise et non comme des stratégies de sécurité pour protéger l’accès à votre réseau.  Il est possible pour les clients non autorisés d’envoyer des en-têtes avec des fausses informations comme un moyen d’y accéder.  

Les stratégies décrites dans cet article doivent toujours être utilisées avec une autre méthode d’authentification, telle que le nom d’utilisateur et le mot de passe ou l’authentification multifacteur.  

## <a name="client-access-policies-scenarios"></a>Scénarios de stratégies d’accès client  

|**Scénario**|**Description**| 
| --- | --- | 
|Scénario 1 : bloquer tout accès externe à Office 365|L’accès à Office 365 est autorisé à partir de tous les clients sur le réseau interne de l’entreprise, mais les demandes des clients externes sont refusées en fonction de l’adresse IP du client externe.|  
|Scénario 2 : bloquer tout accès externe à Office 365, à l’exception d’Exchange ActiveSync|L’accès à Office 365 est autorisé à partir de tous les clients sur le réseau d’entreprise interne, ainsi qu’à partir de tous les périphériques clients externes, tels que les téléphones intelligents, qui utilisent Exchange ActiveSync. Tous les autres clients externes, tels que ceux qui utilisent Outlook, sont bloqués.|  
|Scénario 3 : bloquer tout accès externe à Office 365 à l’exception des applications basées sur un navigateur|Bloque l’accès externe à Office 365, à l’exception des applications passives (basées sur un navigateur) telles qu’Outlook Accès web ou SharePoint Online.|  
|Scénario 4 : bloquer tous les accès externes à Office 365, à l’exception des groupes de Active Directory désignés|Ce scénario est utilisé pour tester et valider le déploiement de la stratégie d’accès client. Il bloque l’accès externe à Office 365 uniquement pour les membres d’un ou de plusieurs groupes de Active Directory. Il peut également être utilisé pour fournir un accès externe uniquement aux membres d’un groupe.|  

## <a name="enabling-client-access-policy"></a>Activation de la stratégie d’accès client  
 Pour activer la stratégie d’accès client dans AD FS dans Windows Server 2012 R2, vous devez mettre à jour l’approbation de la partie de confiance Microsoft Office 365 Identity Platform. Choisissez l’un des exemples de scénarios ci-dessous pour configurer les règles de revendication sur l’approbation de partie de confiance **Microsoft Office 365 Identity** qui répond le mieux aux besoins de votre organisation.  

###  <a name="scenario1"></a>Scénario 1 : bloquer tout accès externe à Office 365  
 Ce scénario de stratégie d’accès client autorise l’accès à partir de tous les clients internes et bloque tous les clients externes en fonction de l’adresse IP du client externe. Vous pouvez utiliser les procédures suivantes pour ajouter les règles d’autorisation d’émission appropriées à l’approbation de la partie de confiance Office 365 pour le scénario choisi.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365"></a>Pour créer des règles pour bloquer tout accès externe à Office 365  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **gestion des AD FS**.  

2.  Dans l’arborescence de la console, sous **relations ad fs\relations**, cliquez sur **approbations de partie de confiance**, cliquez avec le bouton droit sur le **Microsoft Office 365 identité** de la plateforme d’identité, puis cliquez sur **modifier les règles de revendication**.  

3.  Dans la boîte de dialogue **modifier les règles de revendication** , sélectionnez l’onglet **règles d’autorisation d’émission** , puis cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle de revendication.  

4.  Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

5.  Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom d’affichage de cette règle, par exemple « si une revendication IP est en dehors de la plage souhaitée, refuser ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivante (remplacez la valeur ci-dessus pour « x-ms-forwarded-client-IP » par une expression IP valide) :  
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");` </br>
6.  Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste règles d’autorisation d’émission avant d’autoriser la règle par défaut **autoriser l’accès à tous les utilisateurs** (la règle de refus est prioritaire, bien qu’elle apparaisse plus haut dans la liste).  Si vous ne disposez pas de la règle d’accès autoriser par défaut, vous pouvez en ajouter une à la fin de votre liste à l’aide du langage de règle de revendication, comme suit :  </br>

    `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true"); ` 

7.  Pour enregistrer les nouvelles règles, dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK**. La liste résultante doit ressembler à ce qui suit.  

     ![](media/Access-Control-Policies-W2K12/clientaccess1.png "ADFS_Client_Access_1") des règles d’authentification d’émission  

###  <a name="scenario2"></a>Scénario 2 : bloquer tout accès externe à Office 365, à l’exception d’Exchange ActiveSync  
 L’exemple suivant autorise l’accès à toutes les applications Office 365, y compris Exchange Online, à partir des clients internes, y compris Outlook. Il bloque l’accès à partir de clients résidant en dehors du réseau d’entreprise, comme indiqué par l’adresse IP du client, à l’exception des clients Exchange ActiveSync tels que les téléphones intelligents.  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-exchange-activesync"></a>Pour créer des règles pour bloquer tout accès externe à Office 365, à l’exception d’Exchange ActiveSync  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **gestion des AD FS**.  

2.  Dans l’arborescence de la console, sous **relations ad fs\relations**, cliquez sur **approbations de partie de confiance**, cliquez avec le bouton droit sur le **Microsoft Office 365 identité** de la plateforme d’identité, puis cliquez sur **modifier les règles de revendication**.  

3.  Dans la boîte de dialogue **modifier les règles de revendication** , sélectionnez l’onglet **règles d’autorisation d’émission** , puis cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle de revendication.  

4.  Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

5.  Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle, par exemple « si une revendication IP est en dehors de la plage souhaitée, émettez la revendication ipoutsiderange ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivante (remplacez la valeur ci-dessus pour « x-ms-forwarded-client-IP » par une expression IP valide) :  

    `c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  

6.  Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste **règles d’autorisation d’émission** .  

7.  Ensuite, dans la boîte de dialogue **modifier les règles de revendication** , sous l’onglet **règles d’autorisation d’émission** , cliquez sur **Ajouter une règle** pour redémarrer l’Assistant règle de revendication.  

8.  Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

9. Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom d’affichage de cette règle, par exemple « si une adresse IP est en dehors de la plage souhaitée et s’il existe une revendication x-ms-client-application non-EAS, Deny ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivante :  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application", Value != "Microsoft.Exchange.ActiveSync"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  
~~~

10. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste **règles d’autorisation d’émission** .  

11. Ensuite, dans la boîte de dialogue **modifier les règles de revendication** , sous l’onglet **règles d’autorisation d’émission** , cliquez sur **Ajouter une règle** pour redémarrer l’Assistant règle de revendication.  

12. Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication,** sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

13. Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle, par exemple « vérifier si la revendication d’application existe ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivante :  

   ```  
   NOT EXISTS([Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application"]) => add(Type = "http://custom/xmsapplication", Value = "fail");  
   ```  

14. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste **règles d’autorisation d’émission** .  

15. Ensuite, dans la boîte de dialogue **modifier les règles de revendication** , sous l’onglet **règles d’autorisation d’émission** , cliquez sur **Ajouter une règle** pour redémarrer l’Assistant règle de revendication.  

16. Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication,** sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

17. Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle, par exemple « refuser aux utilisateurs avec ipoutsiderange true et l’application échouer ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivante :  

`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/xmsapplication", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`</br>  
18. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle s’affiche immédiatement en dessous de la règle précédente et avant d’autoriser l’accès par défaut à tous les utilisateurs dans la liste des règles d’autorisation d’émission (la règle de refus est prioritaire, bien qu’elle apparaisse plus haut dans la liste).  </br>Si vous ne disposez pas de la règle d’accès autoriser par défaut, vous pouvez en ajouter une à la fin de votre liste à l’aide du langage de règle de revendication, comme suit :</br></br>      `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`</br></br>
19. Pour enregistrer les nouvelles règles, dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur OK. La liste résultante doit ressembler à ce qui suit.  

    ![Règles d'autorisation d'émission](media/Access-Control-Policies-W2K12/clientaccess2.png )  

###  <a name="scenario3"></a>Scénario 3 : bloquer tout accès externe à Office 365 à l’exception des applications basées sur un navigateur  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-browser-based-applications"></a>Pour créer des règles pour bloquer tout accès externe à Office 365 à l’exception des applications basées sur un navigateur  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **gestion des AD FS**.  

2.  Dans l’arborescence de la console, sous **relations ad fs\relations**, cliquez sur **approbations de partie de confiance**, cliquez avec le bouton droit sur le **Microsoft Office 365 identité** de la plateforme d’identité, puis cliquez sur **modifier les règles de revendication**.  

3.  Dans la boîte de dialogue **modifier les règles de revendication** , sélectionnez l’onglet **règles d’autorisation d’émission** , puis cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle de revendication.  

4.  Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

5.  Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle, par exemple « si une revendication IP est en dehors de la plage souhaitée, émettez la revendication ipoutsiderange ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivante (remplacez la valeur ci-dessus pour « x-ms-forwarded-client-IP » par une expression IP valide) :  </br>
`c1:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`   
6.  Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste **règles d’autorisation d’émission** .  

7.  Ensuite, dans la boîte de dialogue **modifier les règles de revendication** , sous l’onglet **règles d’autorisation d’émission** , cliquez sur **Ajouter une règle** pour redémarrer l’Assistant règle de revendication.  

8.  Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication,** sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

9. Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom d’affichage de cette règle, par exemple « si une adresse IP est en dehors de la plage souhaitée et que le point de terminaison n’est pas/adfs/ls, refuser ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivante :  


~~~
`c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path", Value != "/adfs/ls/"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = " DenyUsersWithClaim");`  
~~~

10. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste règles d’autorisation d’émission avant d’autoriser la règle par défaut **autoriser l’accès à tous les utilisateurs** (la règle de refus est prioritaire, bien qu’elle apparaisse plus haut dans la liste).  </br></br> Si vous ne disposez pas de la règle d’accès autoriser par défaut, vous pouvez en ajouter une à la fin de votre liste à l’aide du langage de règle de revendication, comme suit :  

   `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`

11. Pour enregistrer les nouvelles règles, dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur **OK**. La liste résultante doit ressembler à ce qui suit.  

    ![Émission](media/Access-Control-Policies-W2K12/clientaccess3.png)  

###  <a name="scenario4"></a>Scénario 4 : bloquer tous les accès externes à Office 365, à l’exception des groupes de Active Directory désignés  
 L’exemple suivant active l’accès à partir de clients internes en fonction de l’adresse IP. Il bloque l’accès à partir de clients résidant en dehors du réseau d’entreprise qui ont une adresse IP de client externe, à l’exception des individus d’un groupe de Active Directory spécifié. procédez comme suit pour ajouter les règles d’autorisation d’émission appropriées à l’approbation de la partie de confiance **Microsoft Office 365 Identity** à l’aide de l’Assistant règle de revendication :  

##### <a name="to-create-rules-to-block-all-external-access-to-office-365-except-for-designated-active-directory-groups"></a>Pour créer des règles pour bloquer tout accès externe à Office 365, à l’exception des groupes de Active Directory désignés  

1.  Dans **Gestionnaire de serveur**, cliquez sur **Outils**, puis sur **gestion des AD FS**.  

2.  Dans l’arborescence de la console, sous **relations ad fs\relations**, cliquez sur **approbations de partie de confiance**, cliquez avec le bouton droit sur le **Microsoft Office 365 identité** de la plateforme d’identité, puis cliquez sur **modifier les règles de revendication**.  

3.  Dans la boîte de dialogue **modifier les règles de revendication** , sélectionnez l’onglet **règles d’autorisation d’émission** , puis cliquez sur **Ajouter une règle** pour démarrer l’Assistant règle de revendication.  

4.  Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication**, sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

5.  Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom d’affichage de cette règle, par exemple « si une revendication IP est en dehors de la plage souhaitée, émettez la revendication ipoutsiderange. » Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivante (remplacez la valeur ci-dessus pour « x-ms-forwarded-client-IP » par une expression IP valide) :  


~~~
`c1:[Type == "http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip", Value =~ "^(?!192\.168\.1\.77|10\.83\.118\.23)"] && c2:[Type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", Value == "false"] => issue(Type = "http://custom/ipoutsiderange", Value = "true");`  
~~~

6. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste **règles d’autorisation d’émission** .  

7. Ensuite, dans la boîte de dialogue **modifier les règles de revendication** , sous l’onglet **règles d’autorisation d’émission** , cliquez sur **Ajouter une règle** pour redémarrer l’Assistant règle de revendication.  

8. Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication,** sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

9. Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle, par exemple « vérifier le SID du groupe ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivante (remplacez « GroupSID » par le SID réel du groupe Active Directory que vous utilisez) :  

    `NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value == "S-1-5-32-100"]) => add(Type = "http://custom/groupsid", Value = "fail");`  

10. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle apparaît dans la liste **règles d’autorisation d’émission** .  

11. Ensuite, dans la boîte de dialogue **modifier les règles de revendication** , sous l’onglet **règles d’autorisation d’émission** , cliquez sur **Ajouter une règle** pour redémarrer l’Assistant règle de revendication.  

12. Dans la page **Sélectionner un modèle de règle** , sous modèle de règle de **revendication,** sélectionnez **Envoyer des revendications à l’aide d’une règle personnalisée**, puis cliquez sur **suivant**.  

13. Dans la page **configurer la règle** , sous nom de la règle de **revendication**, tapez le nom complet de cette règle, par exemple « refuser les utilisateurs avec ipoutsiderange true et GroupSID Fail ». Sous **règle personnalisée**, tapez ou collez la syntaxe de langage de règle de revendication suivante :  

   `c1:[Type == "http://custom/ipoutsiderange", Value == "true"] && c2:[Type == "http://custom/groupsid", Value == "fail"] => issue(Type = "http://schemas.microsoft.com/authorization/claims/deny", Value = "DenyUsersWithClaim");`  

14. Cliquez sur **Terminer**. Vérifiez que la nouvelle règle s’affiche immédiatement en dessous de la règle précédente et avant d’autoriser l’accès par défaut à tous les utilisateurs dans la liste des règles d’autorisation d’émission (la règle de refus est prioritaire, bien qu’elle apparaisse plus haut dans la liste).  </br></br>Si vous ne disposez pas de la règle d’accès autoriser par défaut, vous pouvez en ajouter une à la fin de votre liste à l’aide du langage de règle de revendication, comme suit :  

   `c:[] => issue(Type = "http://schemas.microsoft.com/authorization/claims/permit", Value = "true");`  

15. Pour enregistrer les nouvelles règles, dans la boîte de dialogue **modifier les règles de revendication** , cliquez sur OK. La liste résultante doit ressembler à ce qui suit.  

     ![Émission](media/Access-Control-Policies-W2K12/clientaccess4.png)  

##  <a name="buildingip"></a>Génération de l’expression de plage d’adresses IP  
 La revendication x-ms-forwarded-client-IP est remplie à partir d’un en-tête HTTP qui est actuellement défini uniquement par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification à AD FS. La valeur de la revendication peut être l’une des suivantes :  

> [!NOTE]
>  Exchange Online ne prend actuellement en charge que les adresses IPV4 et non IPV6.  

-   Une seule adresse IP : l’adresse IP du client qui est directement connecté à Exchange Online  

> [!NOTE]
> - L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme l’adresse IP de l’interface externe du proxy ou de la passerelle sortants de l’organisation.  
>   -   Les clients qui sont connectés au réseau d’entreprise par un VPN ou par Microsoft DirectAccess (DA) peuvent apparaître comme des clients d’entreprise internes ou comme clients externes, en fonction de la configuration du VPN ou de DA.  

-   Une ou plusieurs adresses IP : quand Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur en fonction de la valeur de l’en-tête x-forwarded-for, d’un en-tête non standard qui peut être inclus dans les requêtes basées sur HTTP et est pris en charge par de nombreux clients, équilibreurs de charge et proxys sur le marché.  

> [!NOTE]
> 1. Plusieurs adresses IP, indiquant l’adresse IP du client et l’adresse de chaque proxy qui a transmis la demande, sont séparées par une virgule.  
>    2. Les adresses IP associées à l’infrastructure Exchange Online ne figureront pas dans la liste.  

### <a name="regular-expressions"></a>Expressions régulières  
 Lorsque vous devez faire correspondre une plage d’adresses IP, il devient nécessaire de construire une expression régulière pour effectuer la comparaison. Dans la prochaine série d’étapes, nous fournirons des exemples de construction d’une telle expression pour qu’elle corresponde aux plages d’adresses suivantes (Notez que vous devrez modifier ces exemples pour qu’ils correspondent à votre plage d’adresses IP publiques) :  

- 192.168.1.1 – 192.168.1.25  

- 10.0.0.1 – 10.0.0.14  

  Tout d’abord, le modèle de base qui correspondra à une seule adresse IP est le suivant : \b # # #\\. # # #\\. # # #\\. # # # \b  

  Si vous étendez cela, nous pouvons faire correspondre deux adresses IP différentes avec une expression ou comme suit : \b # # #\\. # # #\\. # # #\\.&#124;# # # \b \b # # #\\. # # #\\. # # #. # # # \b\\  

  Par conséquent, voici un exemple qui correspond à deux adresses (telles que 192.168.1.1 ou 10.0.0.1) : \b192\\. 168\\1\\. 1 \&#124;b \b10\\0 0\\. 0\\. 1 \ b  

  Cela vous donne la technique qui vous permet d’entrer un nombre quelconque d’adresses. Si une plage d’adresses doit être autorisée, par exemple 192.168.1.1 – 192.168.1.25, la correspondance doit être effectuée caractère par caractère : \b192\\. 168\\. 1\\. ([1-9]&#124;1 [0-9]&#124;2 [0-5]) \b  

  Notez les points suivants :  

- L’adresse IP est traitée comme une chaîne et non un nombre.  

- La règle est décomposée comme suit : \b192\\. 168\\. 1\\.  

- Cela correspond à toute valeur commençant par 192.168.1.  

- Les éléments suivants correspondent aux plages requises pour la partie de l’adresse après la virgule décimale finale :  

  -   ([1-9] correspond aux adresses se terminant par 1-9  

  -   &#124;1 [0-9] correspond aux adresses se terminant par 10-19  

  -   &#124;2 [0-5]) correspond à des adresses se terminant par 20-25  

- Notez que les parenthèses doivent être correctement positionnées, de sorte que vous ne commencez pas à mettre en correspondance d’autres parties des adresses IP.  

- Une fois le bloc 192 mis en correspondance, nous pouvons écrire une expression similaire pour le bloc 10 : \b10\\0\\. 0\\. ([1-9]&#124;1 [0-4]) \b  

- Et en les rassemblant, l’expression suivante doit correspondre à toutes les adresses pour « 192.168.1.1 ~ 25 » et « 10.0.0.1 ~ 14 » : \b192\\. 168\\. 1\\. ([1-9]&#124;1 [0-9]&#124;2 [0-5]) \b&#124;\b10\\0\\. 0\\. ([1-9]&#124;1 [0-4]) \b  

### <a name="testing-the-expression"></a>Test de l’expression  
 Les expressions Regex peuvent devenir assez difficiles. nous vous recommandons donc vivement d’utiliser un outil de vérification Regex. Si vous effectuez une recherche sur Internet pour « générateur d’expressions Regex en ligne », vous trouverez plusieurs utilitaires en ligne de qualité qui vous permettront de tester vos expressions par rapport à des exemples de données.  

 Lors du test de l’expression, il est important de comprendre ce qu’il faut s’attendre à trouver. Le système Exchange Online peut envoyer de nombreuses adresses IP, séparées par des virgules. Les expressions fournies ci-dessus fonctionnent pour cela. Toutefois, il est important de réfléchir à ce point lorsque vous testez vos expressions Regex. Par exemple, il est possible d’utiliser l’exemple d’entrée suivant pour vérifier les exemples ci-dessus :  

 192.168.1.1, 192.168.1.2, 192.169.1.1. 192.168.12.1, 192.168.1.10, 192.168.1.25, 192.168.1.26, 192.168.1.30, 1192.168.1.20  

 10.0.0.1, 10.0.0.5, 10.0.0.10, 10.0.1.0, 10.0.1.1, 110.0.0.1, 10.0.0.14, 10.0.0.15, 10.0.0.10, 10, 0.0.1  

## <a name="claim-types"></a>Types de revendications  
 AD FS dans Windows Server 2012 R2 fournit des informations de contexte de requête à l’aide des types de revendications suivants :  

### <a name="x-ms-forwarded-client-ip"></a>X-MS-forwarded-client-IP  
 Type de revendication : `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-forwarded-client-ip`  

 Cette revendication de AD FS représente une « meilleure tentative » pour déterminer l’adresse IP de l’utilisateur (par exemple, le client Outlook) à l’origine de la demande. Cette revendication peut contenir plusieurs adresses IP, y compris l’adresse de chaque proxy qui a transféré la demande.  Cette revendication est renseignée à partir d’un HTTP. La valeur de la revendication peut être l’une des suivantes :  

-   Une seule adresse IP : l’adresse IP du client qui est directement connecté à Exchange Online  

> [!NOTE]
>  L’adresse IP d’un client sur le réseau d’entreprise s’affiche comme l’adresse IP de l’interface externe du proxy ou de la passerelle sortants de l’organisation.  

-   Une ou plusieurs adresses IP  

    -   Si Exchange Online ne peut pas déterminer l’adresse IP du client qui se connecte, il définit la valeur en fonction de la valeur de l’en-tête x-forwarded-for, d’un en-tête non standard qui peut être inclus dans les demandes HTTP et est pris en charge par de nombreux clients, les équilibrages de charge et proxies sur le marché.  

    -   Plusieurs adresses IP indiquant l’adresse IP du client et l’adresse de chaque proxy qui a transmis la demande sont séparées par une virgule.  

> [!NOTE]
>  Les adresses IP associées à l’infrastructure Exchange Online ne seront pas présentes dans la liste.  

> [!WARNING]
>  Exchange Online ne prend actuellement en charge que les adresses IPV4. il ne prend pas en charge les adresses IPV6.  

### <a name="x-ms-client-application"></a>X-MS-client-application  
 Type de revendication : `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-application`  

 Cette revendication de AD FS représente le protocole utilisé par le client final, qui correspond vaguement à l’application utilisée.  Cette revendication est remplie à partir d’un en-tête HTTP qui est actuellement défini uniquement par Exchange Online, qui remplit l’en-tête lors du passage de la demande d’authentification à AD FS. Selon l’application, la valeur de cette revendication sera l’une des suivantes :  

-   Dans le cas des appareils qui utilisent Exchange Active Sync, la valeur est Microsoft. Exchange. ActiveSync.  

-   L’utilisation du client Microsoft Outlook peut avoir l’une des valeurs suivantes :  

    -   Découverte automatique Microsoft. Exchange.  

    -   Microsoft. Exchange. OfflineAddressBook  

    -   Microsoft. Exchange. RPCMicrosoft. Exchange. WebServices  

    -   Microsoft. Exchange. RPCMicrosoft. Exchange. WebServices  

-   Les autres valeurs possibles pour cet en-tête sont les suivantes :  

    -   Microsoft. Exchange. PowerShell  

    -   Microsoft. Exchange. SMTP  

    -   Microsoft. Exchange. pop  

    -   Microsoft. Exchange. IMAP  

### <a name="x-ms-client-user-agent"></a>X-MS-client-user-agent  
 Type de revendication : `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent`  

 Cette revendication de AD FS fournit une chaîne représentant le type d’appareil utilisé par le client pour accéder au service. Cela peut être utilisé lorsque les clients souhaitent empêcher l’accès à certains appareils (tels que des types particuliers de téléphones intelligents).  Voici des exemples de valeurs pour cette revendication : les valeurs ci-dessous (sans s’y limiter).  

 Voici des exemples de ce que la valeur x-ms-user-agent peut contenir pour un client dont la valeur x-ms-client-application est « Microsoft. Exchange. ActiveSync »  

- Vortex/1.0  

- Apple-iPad1C1/812.1  

- Apple-iPhone3C1/811.2  

- Apple-iPhone/704.11  

- Moto-DROID2/4.5.1  

- SAMSUNGSPHD700/100.202  

- Android/0.3  

  Il est également possible que cette valeur soit vide.  

### <a name="x-ms-proxy"></a>X-MS-proxy  
 Type de revendication : `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-proxy`  

 Cette revendication de AD FS indique que la demande est passée par le proxy d’application Web.  Cette revendication est remplie par le proxy d’application Web, qui remplit l’en-tête lors du passage de la demande d’authentification à l’service FS (Federation Service) back end. AD FS ensuite le convertit en revendication.  

 La valeur de la revendication est le nom DNS du proxy d’application Web qui a transmis la demande.  

### <a name="insidecorporatenetwork"></a>InsideCorporateNetwork  
 Type de revendication : `http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork`  

 Comme pour le type de revendication x-ms-proxy ci-dessus, ce type de revendication indique si la demande est passée par le proxy d’application Web. Contrairement à x-ms-proxy, insidecorporatenetwork est une valeur booléenne avec true indiquant une demande directement au service de Fédération à partir du réseau d’entreprise.  

### <a name="x-ms-endpoint-absolute-path-active-vs-passive"></a>X-MS-Endpoint-Absolute-Path (actif/passif)  
 Type de revendication : `http://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path`  

 Ce type de revendication peut être utilisé pour déterminer les demandes provenant de clients « actifs » (riches) par rapport aux clients « passifs » (basés sur un navigateur Web). Cela permet aux requêtes externes provenant d’applications basées sur un navigateur, telles que le Accès web Outlook, SharePoint Online ou le portail Office 365, d’être autorisées pendant que les demandes provenant de clients enrichis, telles que Microsoft Outlook, sont bloquées.  

 La valeur de la revendication est le nom du service AD FS qui a reçu la demande.  

## <a name="see-also"></a>Voir aussi  
 [Opérations d’AD FS](../../ad-fs/AD-FS-2016-Operations.md)
