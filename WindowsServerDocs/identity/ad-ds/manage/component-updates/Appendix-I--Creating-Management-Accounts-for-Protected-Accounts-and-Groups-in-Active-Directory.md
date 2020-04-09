---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: Annexe I-création de comptes de gestion pour les comptes et les groupes protégés dans Active Directory
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c2141e4fad564579fd687b2dfc7e4a12e1634acb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80823482"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Annexe I : Création de comptes de gestion pour les comptes protégés et les groupes dans Active Directory

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L’une des difficultés liées à l’implémentation d’un modèle de Active Directory qui ne repose pas sur l’appartenance permanente à des groupes à privilèges élevés est qu’il doit exister un mécanisme pour remplir ces groupes lorsque l’appartenance temporaire aux groupes est requise. Certaines solutions privilégiées de gestion des identités requièrent que les comptes de service du logiciel bénéficient d’une appartenance permanente à des groupes tels que DA ou administrateurs dans chaque domaine de la forêt. Toutefois, il n’est techniquement pas nécessaire que les solutions Privileged Identity Management (PIM) exécutent leurs services dans des contextes à privilèges élevés.  
  
Cette annexe fournit des informations que vous pouvez utiliser pour implémenter des solutions PIM tierces ou implémentées en mode natif afin de créer des comptes avec des privilèges limités et pouvant être contrôlés de manière stricte, mais qui peuvent être utilisés pour remplir des groupes privilégiés dans Active Directory lorsque l’élévation temporaire est nécessaire. Si vous implémentez PIM en tant que solution native, ces comptes peuvent être utilisés par le personnel administratif pour exécuter le remplissage de groupe temporaire. Si vous implémentez PIM par le biais d’un logiciel tiers, vous pourrez peut-être adapter ces comptes pour qu’ils fonctionnent en tant que comptes de service.  
  
> [!NOTE]  
> Les procédures décrites dans cette annexe fournissent une approche de la gestion des groupes à privilèges élevés dans Active Directory. Vous pouvez adapter ces procédures en fonction de vos besoins, ajouter des restrictions supplémentaires ou omettre certaines des restrictions décrites ici.  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Création de comptes de gestion pour les comptes et les groupes protégés dans Active Directory

La création de comptes qui peuvent être utilisés pour gérer l’appartenance à des groupes privilégiés sans nécessiter que les comptes de gestion disposent de droits et d’autorisations excessifs se compose de quatre activités générales qui sont décrites dans les instructions pas à pas ci-dessous :  
  
1.  Tout d’abord, vous devez créer un groupe qui gérera les comptes, car ces comptes doivent être gérés par un ensemble limité d’utilisateurs approuvés. Si vous n’avez pas encore une structure d’unité d’organisation qui prend en charge la répartition des comptes et des systèmes privilégiés et protégés de la population générale dans le domaine, vous devez en créer une. Bien que des instructions spécifiques ne soient pas fournies dans cette annexe, les captures d’écran présentent un exemple d’une telle hiérarchie d’UO.  
  
2.  Créez les comptes de gestion. Ces comptes doivent être créés en tant que comptes d’utilisateur « standard » et ne pas avoir de droits d’utilisateur au-delà de ceux qui sont déjà accordés aux utilisateurs par défaut.  
  
3.  Implémentez des restrictions sur les comptes de gestion qui les rendent utilisables uniquement dans le but spécialisé pour lequel ils ont été créés, en plus de contrôler qui peut activer et utiliser les comptes (le groupe que vous avez créé à la première étape).  
  
4.  Configurez les autorisations sur l’objet AdminSDHolder dans chaque domaine pour permettre aux comptes de gestion de modifier l’appartenance des groupes privilégiés dans le domaine.  
  
Vous devez tester minutieusement toutes ces procédures et les modifier en fonction des besoins de votre environnement avant de les implémenter dans un environnement de production. Vous devez également vérifier que tous les paramètres fonctionnent comme prévu (certaines procédures de test sont fournies dans cette annexe) et vous devez tester un scénario de récupération d’urgence dans lequel les comptes de gestion ne peuvent pas être utilisés pour remplir des groupes protégés à des fins de récupération. Pour plus d’informations sur la sauvegarde et la restauration de Active Directory, consultez le [Guide pas à pas](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx)de la AD DS de la sauvegarde et de la récupération.  
  
> [!NOTE]  
> En implémentant les étapes décrites dans cette annexe, vous allez créer des comptes qui seront en mesure de gérer l’appartenance de tous les groupes protégés dans chaque domaine, non seulement les groupes à privilèges élevés Active Directory comme EAs, DAs et BAs. Pour plus d’informations sur les groupes protégés dans Active Directory, consultez l' [annexe C : comptes et groupes protégés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Instructions pas à pas pour la création de comptes de gestion pour les groupes protégés  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Création d’un groupe pour activer et désactiver des comptes de gestion

Les mots de passe des comptes de gestion doivent être réinitialisés à chaque utilisation et doivent être désactivés lorsque les activités nécessitant ces derniers sont terminées. Bien que vous deviez également envisager d’implémenter des exigences d’ouverture de session par carte à puce pour ces comptes, il s’agit d’une configuration facultative. ces instructions partent du principe que les comptes de gestion seront configurés avec un nom d’utilisateur et un mot de passe long et complexe comme contrôles minimaux. Dans cette étape, vous allez créer un groupe qui dispose des autorisations pour réinitialiser le mot de passe sur les comptes de gestion et pour activer et désactiver les comptes.  
  
Pour créer un groupe afin d’activer et de désactiver les comptes de gestion, procédez comme suit :  
  
1.  Dans la structure d’unité d’organisation dans laquelle vous hébergerez les comptes de gestion, cliquez avec le bouton droit sur l’unité d’organisation dans laquelle vous souhaitez créer le groupe, cliquez sur **nouveau** , puis sur **groupe**.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  Dans la boîte de dialogue **nouvel objet-groupe** , entrez un nom pour le groupe. Si vous envisagez d’utiliser ce groupe pour « activer » tous les comptes de gestion de votre forêt, faites-en un groupe de sécurité universel. Si vous avez une forêt à domaine unique ou si vous envisagez de créer un groupe dans chaque domaine, vous pouvez créer un groupe de sécurité global. Cliquez sur **OK** pour créer le groupe.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Cliquez avec le bouton droit sur le groupe que vous venez de créer, cliquez sur **Propriétés**, puis sur l’onglet **objet** . Dans la boîte de dialogue propriété de l' **objet** du groupe, sélectionnez **protéger l’objet contre une suppression accidentelle**, ce qui empêchera les utilisateurs autorisés autrement de supprimer le groupe, mais également de le déplacer vers une autre unité d’organisation, sauf si l’attribut est désélectionné pour la première fois.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > Si vous avez déjà configuré des autorisations sur les unités d’organisation parentes du groupe pour limiter l’administration à un ensemble limité d’utilisateurs, vous n’aurez peut-être pas besoin d’effectuer les étapes suivantes. Ils sont fournis ici afin que même si vous n’avez pas encore implémenté un contrôle administratif limité sur la structure de l’unité d’organisation dans laquelle vous avez créé ce groupe, vous pouvez sécuriser le groupe contre toute modification par des utilisateurs non autorisés.  
  
4.  Cliquez sur l’onglet **membres** et ajoutez les comptes des membres de votre équipe qui seront chargés de l’activation des comptes de gestion ou du remplissage des groupes protégés si nécessaire.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Si vous ne l’avez pas encore fait, dans la console **utilisateurs et ordinateurs Active Directory** , cliquez sur **Afficher** , puis sélectionnez **fonctionnalités avancées**. Cliquez avec le bouton droit sur le groupe que vous venez de créer, cliquez sur **Propriétés**, puis sur l’onglet **sécurité** . Sous l’onglet **sécurité** , cliquez sur **avancé**.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  Dans la boîte de dialogue **paramètres de sécurité avancés pour [groupe]** , cliquez sur **désactiver l’héritage**. Lorsque vous y êtes invité, cliquez sur **convertir les autorisations héritées en autorisations explicites sur cet objet**, puis cliquez sur **OK** pour revenir à la boîte de dialogue **sécurité** du groupe.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  Sous l’onglet **sécurité** , supprimez les groupes qui ne doivent pas être autorisés à accéder à ce groupe. Par exemple, si vous ne souhaitez pas que les utilisateurs authentifiés puissent lire le nom et les propriétés générales du groupe, vous pouvez supprimer cette entrée du contrôle d’accès. Vous pouvez également supprimer des ACE, telles que celles pour les opérateurs de compte et l’accès compatible avec les serveurs antérieurs à Windows 2000. Toutefois, vous devez conserver un ensemble minimal d’autorisations sur les objets en place. Laissez les ACE suivantes intactes :  
  
    -   RYTHME  
  
    -   SYSTEM  
  
    -   Admins du domaine  
  
    -   Administrateurs de l'entreprise  
  
    -   Administrateurs  
  
    -   Groupe d’accès d’autorisation Windows (le cas échéant)  
  
    -   CONTRÔLEURS DE DOMAINE D’ENTREPRISE  
  
    Bien qu’il puisse paraître non intuitifs d’autoriser les groupes dotés de privilèges les plus élevés dans Active Directory à gérer ce groupe, votre objectif en matière d’implémentation de ces paramètres est de ne pas empêcher les membres de ces groupes d’apporter des modifications autorisées. Au lieu de cela, l’objectif est de s’assurer que, lorsque vous avez l’occasion d’exiger des niveaux de privilège très élevés, les modifications autorisées échoueront. C’est pour cette raison que la modification de l’imbrication, des droits et des autorisations par défaut des groupes privilégiés est déconseillée dans ce document. En laissant intactes les structures par défaut et en vidant l’appartenance des groupes de privilèges les plus élevés dans l’annuaire, vous pouvez créer un environnement plus sécurisé qui fonctionne toujours comme prévu.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Si vous n’avez pas déjà configuré des stratégies d’audit pour les objets de la structure d’unité d’organisation où vous avez créé ce groupe, vous devez configurer l’audit pour consigner les modifications apportées à ce groupe.  
  
8.  Vous avez terminé la configuration du groupe qui sera utilisé pour « extraire » les comptes de gestion lorsqu’ils sont nécessaires et « archiver » les comptes lorsque leurs activités sont terminées.  
  
#### <a name="creating-the-management-accounts"></a>Création des comptes de gestion

Vous devez créer au moins un compte qui sera utilisé pour gérer l’appartenance des groupes privilégiés dans votre installation Active Directory, et de préférence un deuxième compte pour servir de sauvegarde. Que vous choisissiez de créer les comptes de gestion dans un domaine unique dans la forêt et de leur accorder des fonctionnalités de gestion pour tous les groupes protégés par les domaines, ou si vous choisissez d’implémenter des comptes de gestion dans chaque domaine de la forêt, les procédures sont les mêmes.  
  
> [!NOTE]  
> Les étapes décrites dans ce document supposent que vous n’avez pas encore implémenté les contrôles d’accès en fonction du rôle et Privileged Identity Management pour Active Directory. Par conséquent, certaines procédures doivent être effectuées par un utilisateur dont le compte est membre du groupe Admins du domaine pour le domaine en question.  
>   
> Lorsque vous utilisez un compte avec des privilèges DA, vous pouvez ouvrir une session sur un contrôleur de domaine pour effectuer les activités de configuration. Les étapes qui ne nécessitent pas de privilèges DA peuvent être effectuées par des comptes avec moins de privilèges qui sont connectés aux stations de travail d’administration. Les captures d’écran qui affichent des boîtes de dialogue frontalières dans la couleur bleue plus claire représentent des activités qui peuvent être effectuées sur un contrôleur de domaine. Les captures d’écran qui affichent des boîtes de dialogue dans la couleur bleue plus sombre représentent les activités qui peuvent être effectuées sur les stations de travail d’administration avec des comptes disposant de privilèges limités.  
  
Pour créer les comptes de gestion, procédez comme suit :  
  
1. Connectez-vous à un contrôleur de domaine à l’aide d’un compte membre du groupe DA du domaine.  

2. Lancez **Active Directory utilisateurs et ordinateurs** , puis accédez à l’unité d’organisation dans laquelle vous allez créer le compte de gestion.  

3. Cliquez avec le bouton droit sur l’unité d’organisation, puis cliquez sur **nouveau** et sur **utilisateur**.  

4. Dans la boîte de dialogue **nouvel objet-utilisateur** , entrez les informations de nom de votre choix pour le compte, puis cliquez sur **suivant**.  

   ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. Fournissez un mot de passe initial pour le compte d’utilisateur. l' **utilisateur doit modifier le mot de passe à la prochaine ouverture de session**, sélectionner l' **utilisateur ne peut pas modifier le mot de passe** et le **compte est désactivé**, puis cliquez sur **suivant**.  

   ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. Vérifiez que les détails du compte sont corrects, puis cliquez sur **Terminer**.  

7. Cliquez avec le bouton droit sur l’objet utilisateur que vous venez de créer, puis cliquez sur **Propriétés**.  

8. Cliquez sur l’onglet **compte** .  

9. Dans le champ **options de compte** , sélectionnez l’indicateur le **compte est sensible et ne peut pas être délégué** , sélectionnez l’indicateur ce compte **prend en charge le chiffrement AES 128 bits Kerberos** et/ou le **compte ce compte prend en charge le chiffrement Kerberos AES 256** , puis cliquez sur **OK**.  

   ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > Étant donné que ce compte, comme d’autres comptes, aura une fonction limitée, mais puissante, le compte ne doit être utilisé que sur des ordinateurs hôtes d’administration sécurisés. Pour tous les ordinateurs hôtes d’administration sécurisés dans votre environnement, vous devez envisager d’implémenter le paramètre stratégie de groupe **sécurité réseau : configurer les types de chiffrement autorisés pour Kerberos** afin d’autoriser uniquement les types de chiffrement les plus sécurisés que vous pouvez implémenter pour les hôtes sécurisés.  
   >
   > Bien que l’implémentation de types de chiffrement plus sécurisés pour les hôtes n’atténue pas les attaques par vol d’informations d’identification, l’utilisation et la configuration appropriées des hôtes sécurisés. La définition de types de chiffrement renforcés pour les hôtes qui sont utilisés uniquement par les comptes privilégiés réduit simplement la surface d’attaque globale des ordinateurs.  
   >
   > Pour plus d’informations sur la configuration des types de chiffrement sur les systèmes et les comptes, consultez [configurations Windows pour le type de chiffrement pris en charge par Kerberos](https://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
   >
   > Ces paramètres sont pris en charge uniquement sur les ordinateurs exécutant Windows Server 2012, Windows Server 2008 R2, Windows 8 ou Windows 7.  
  
10. Sous l’onglet **objet** , sélectionnez **protéger l’objet contre une suppression accidentelle**. Cela empêchera non seulement la suppression de l’objet (même par les utilisateurs autorisés), mais empêchera son déplacement vers une unité d’organisation différente dans votre hiérarchie AD DS, sauf si la case à cocher est désactivée pour la première fois par un utilisateur autorisé à modifier l’attribut.  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. Cliquez sur l’onglet **contrôle à distance** .  

12. Désactivez l’indicateur **activer le contrôle à distance** . Il ne doit jamais être nécessaire pour le personnel du support technique de se connecter aux sessions de ce compte pour implémenter des correctifs.  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

    > [!NOTE]  
    > Chaque objet de Active Directory doit avoir un propriétaire informatique désigné et un propriétaire d’entreprise désigné, comme décrit dans la section [planification de la compromission](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Si vous effectuez le suivi de la propriété des objets AD DS dans Active Directory (par opposition à une base de données externe), vous devez entrer les informations de propriété appropriées dans les propriétés de cet objet.  
    >
    > Dans ce cas, le dirigeant d’entreprise est probablement une division informatique, andthere n’est pas une interdiction sur les propriétaires d’entreprise. Le point d’établissement de la propriété des objets est de vous permettre d’identifier les contacts lorsque des modifications doivent être apportées aux objets, peut-être des années à partir de leur création initiale.  

13. Cliquez sur l’onglet **organisation** .  

14. Entrez les informations requises dans vos normes d’objets AD DS.  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. Cliquez sur l’onglet **appels entrants** .  

16. Dans le champ **autorisation d’accès au réseau** , sélectionnez **refuser l’accès**. Ce compte ne doit jamais être connecté via une connexion à distance.  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

    > [!NOTE]  
    > Il est peu probable que ce compte soit utilisé pour se connecter à des contrôleurs de domaine en lecture seule (RODC) dans votre environnement. Toutefois, si vous souhaitez que le compte se connecte à un contrôleur de domaine en lecture seule, vous devez ajouter ce compte au groupe de réplication de mot de passe RODC refusé afin que son mot de passe ne soit pas mis en cache sur le contrôleur de domaine en lecture seule.  
    >
    > Même si le mot de passe du compte doit être réinitialisé après chaque utilisation et que le compte doit être désactivé, l’implémentation de ce paramètre n’a pas d’effet nuisible sur le compte, et il peut être utile dans les situations où un administrateur oublie de réinitialiser le mot de passe du compte et de le désactiver.  

17. Cliquez sur l’onglet **Membre de**.  

18. Cliquez sur **Ajouter**.  

19. Tapez **refuser le mot de passe RODC groupe de réplication** dans la boîte de dialogue **Sélectionner les utilisateurs, les contacts et les ordinateurs** , puis cliquez sur **vérifier les noms**. Lorsque le nom du groupe est souligné dans le sélecteur d’objets, cliquez sur **OK** et vérifiez que le compte est désormais membre des deux groupes affichés dans la capture d’écran suivante. N’ajoutez pas le compte à des groupes protégés.  

20. Cliquez sur **OK**.  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. Cliquez sur l’onglet **sécurité** , puis sur **avancé**.  

22. Dans la boîte de dialogue **paramètres de sécurité avancés** , cliquez sur **désactiver l’héritage** et copiez les autorisations héritées en tant qu’autorisations explicites, puis cliquez sur **Ajouter**.  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. Dans la boîte **de dialogue entrée d’autorisation pour [compte]** , cliquez sur **Sélectionner un principal** et ajoutez le groupe que vous avez créé dans la procédure précédente. Faites défiler vers le bas de la boîte de dialogue, puis cliquez sur **Effacer tout** pour supprimer toutes les autorisations par défaut.  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. Faites défiler vers le haut de la boîte de dialogue **entrée d’autorisation** . Vérifiez que la liste déroulante **type** est définie sur **autoriser**et, dans la liste déroulante **s’applique à** , sélectionnez **cet objet uniquement**.  

25. Dans le champ **autorisations** , sélectionnez **Lire toutes les propriétés**, **lire les autorisations**et réinitialiser le **mot de passe**.  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. Dans le champ **Propriétés** , sélectionnez **lire userAccountControl** et **Write userAccountControl**.  

27. Cliquez à nouveau **OK** **sur OK dans**la boîte de dialogue Paramètres de **sécurité avancés** .  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

    > [!NOTE]  
    > L’attribut **UserAccountControl** contrôle plusieurs options de configuration de compte. Vous ne pouvez pas accorder l’autorisation de modifier uniquement certaines des options de configuration lorsque vous accordez une autorisation d’accès en écriture à l’attribut.  

28. Dans le champ **noms de groupes ou d’utilisateurs** de l’onglet **sécurité** , supprimez les groupes qui ne doivent pas être autorisés à accéder au compte ou à le gérer. Ne supprimez pas les groupes qui ont été configurés avec des entrées de contrôle d’accès en refus, tels que le groupe tout le monde et le compte auto-calculé (cette entrée de contrôle d’accès a été définie lorsque l’indicateur l' **utilisateur ne peut pas modifier le mot de passe** a été activé pendant la création du compte. Par ailleurs, ne supprimez pas le groupe que vous venez d’ajouter, le compte système ou les groupes tels que EA, DA, BA ou le groupe d’accès d’autorisation Windows.  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. Cliquez sur **avancé** et vérifiez que la boîte de dialogue Paramètres de sécurité avancés ressemble à la capture d’écran suivante.  

30. Cliquez sur **OK**, puis à nouveau sur **OK** pour fermer la boîte de dialogue des propriétés du compte.  

    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. La configuration du premier compte de gestion est maintenant terminée. Vous allez tester le compte dans une procédure ultérieure.  

##### <a name="creating-additional-management-accounts"></a>Création de comptes de gestion supplémentaires

Vous pouvez créer des comptes de gestion supplémentaires en répétant les étapes précédentes, en copiant le compte que vous venez de créer ou en créant un script pour créer des comptes avec les paramètres de configuration souhaités. Notez, toutefois, que si vous copiez le compte que vous venez de créer, la plupart des paramètres et ACL personnalisés ne seront pas copiés vers le nouveau compte et vous devrez répéter la plupart des étapes de configuration.  
  
À la place, vous pouvez créer un groupe auquel vous déléguez des droits pour remplir et supprimer le remplissage des groupes protégés, mais vous devez sécuriser le groupe et les comptes qu’il contient. Comme il doit y avoir très peu de comptes dans votre annuaire qui peuvent gérer l’appartenance à des groupes protégés, la création de comptes individuels peut être l’approche la plus simple.  
  
Quelle que soit la façon dont vous choisissez de créer un groupe dans lequel vous placez les comptes de gestion, vous devez vous assurer que chaque compte est sécurisé comme décrit précédemment. Vous devez également envisager d’implémenter des restrictions d’objet de stratégie de groupe similaires à celles décrites dans l' [annexe D : sécurisation des comptes administrateur intégrés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
##### <a name="auditing-management-accounts"></a>Audit des comptes de gestion

Vous devez configurer l’audit sur le compte à journaliser au minimum toutes les écritures dans le compte. Cela vous permet non seulement d’identifier correctement l’activation du compte et la réinitialisation de son mot de passe lors de l’utilisation autorisée, mais également d’identifier les tentatives effectuées par les utilisateurs non autorisés pour manipuler le compte. Les écritures ayant échoué sur le compte doivent être capturées dans votre système SIEM (Security information and Event Monitoring) (le cas échéant) et déclencher des alertes qui notifient au personnel chargé d’examiner les compromissions potentielles.  
  
Les solutions SIEM prennent des informations sur les événements des sources de sécurité impliquées (par exemple, les journaux d’événements, les données d’application, les flux réseau, les produits anti-programme malveillant et les sources de détection d’intrusion), rassemblent les données et tentent d’effectuer des vues intelligentes et des actions proactives. Il existe de nombreuses solutions SIEM commerciales et de nombreuses entreprises créent des implémentations privées. Une application SIEM bien conçue et implémentée de manière appropriée peut améliorer considérablement la surveillance de la sécurité et les fonctionnalités de réponse aux incidents. Toutefois, les capacités et la précision varient considérablement d’une solution à l’autre. Les SIEM n’entrent pas dans le cadre de ce document, mais les recommandations d’événements spécifiques doivent être prises en compte par tout responsable de l’implémentation SIEM.  
  
Pour plus d’informations sur les paramètres de configuration d’audit recommandés pour les contrôleurs de domaine, consultez [surveillance des Active Directory pour les signes de compromission](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Les paramètres de configuration spécifiques au contrôleur de domaine sont fournis dans [surveillance Active Directory pour les signes de compromission](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Activation des comptes de gestion pour modifier l’appartenance des groupes protégés

Dans cette procédure, vous allez configurer des autorisations sur l’objet AdminSDHolder du domaine pour permettre aux comptes de gestion nouvellement créés de modifier l’appartenance des groupes protégés dans le domaine. Cette procédure ne peut pas être effectuée via une interface utilisateur graphique (GUI).  
  
Comme indiqué dans [l’annexe C : comptes et groupes protégés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), la liste de contrôle d’accès sur l’objet AdminSDHolder d’un domaine est effectivement « copiée » sur les objets protégés lors de l’exécution de la tâche SDPROP. Les groupes et les comptes protégés n’héritent pas des autorisations de l’objet AdminSDHolder ; leurs autorisations sont explicitement définies pour correspondre à celles de l’objet AdminSDHolder. Par conséquent, lorsque vous modifiez des autorisations sur l’objet AdminSDHolder, vous devez les modifier pour les attributs appropriés au type de l’objet protégé que vous ciblez.  
  
Dans ce cas, vous allez accorder les comptes de gestion nouvellement créés pour leur permettre de lire et d’écrire l’attribut members sur les objets de groupe. Toutefois, l’objet AdminSDHolder n’est pas un objet de groupe et les attributs de groupe ne sont pas exposés dans l’éditeur de liste de contrôle d’accès graphique. C’est pour cette raison que vous allez implémenter les modifications d’autorisations via l’utilitaire de ligne de commande dsacls. Pour accorder aux comptes de gestion (désactivés) les autorisations nécessaires pour modifier l’appartenance de groupes protégés, procédez comme suit :  
  
1. Connectez-vous à un contrôleur de domaine, de préférence le contrôleur de domaine détenant le rôle d’émulateur de contrôleur de domaine principal (émulateur), avec les informations d’identification d’un compte d’utilisateur qui a été créé en tant que membre du groupe DA dans le domaine.  
  
   ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2. Ouvrez une invite de commandes avec élévation de privilèges en cliquant avec le bouton droit sur **invite de commandes** , puis cliquez sur **exécuter en tant qu’administrateur**.  
  
   ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3. Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  
  
   ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
   > [!NOTE]  
   > Pour plus d’informations sur l’élévation et le contrôle de compte d’utilisateur (UAC) dans Windows, consultez [processus et interactions UAC](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) sur le site Web TechNet.  
  
4. À l’invite de commandes, tapez (en remplaçant vos informations spécifiques au domaine) **dsacls [nom unique de l’objet AdminSDHolder dans votre domaine]/g [nom UPN du compte de gestion] : RPWP ; membre**.  
  
   ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
   La commande précédente (qui ne respecte pas la casse) fonctionne comme suit :  
  
   - Dsacls définit ou affiche des ACE sur les objets d’annuaire  
  
   - CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = msft identifie l’objet à modifier.  
  
   - /G indique qu’une entrée ACE Grant est en cours de configuration  
  
   - PIM001@tailspintoys.msft est le nom d’utilisateur principal (UPN) du principal de sécurité auquel les ACE seront accordées.  
  
   - RPWP accorde des autorisations de propriété Read et Write Property  
  
   - Member est le nom de la propriété (attribut) sur laquelle les autorisations seront définies.  
  
   Pour plus d’informations sur l’utilisation de **dsacls**, tapez DSACLS sans aucun paramètre dans une invite de commandes.  
  
   Si vous avez créé plusieurs comptes de gestion pour le domaine, vous devez exécuter la commande dsacls pour chaque compte. Lorsque vous avez terminé la configuration de la liste de contrôle d’accès sur l’objet AdminSDHolder, vous devez forcer l’exécution de SDProp ou attendre la fin de l’exécution planifiée. Pour plus d’informations sur l’exécution forcée de SDProp, consultez « exécution de SDProp manuellement » dans [annexe C : comptes et groupes protégés dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
   Quand SDProp a été exécuté, vous pouvez vérifier que les modifications que vous avez apportées à l’objet AdminSDHolder ont été appliquées à des groupes protégés dans le domaine. Vous ne pouvez pas vérifier cela en consultant l’ACL sur l’objet AdminSDHolder pour les raisons décrites précédemment, mais vous pouvez vérifier que les autorisations ont été appliquées en affichant les listes de contrôle d’accès sur les groupes protégés.  
  
5. Dans **Active Directory utilisateurs et ordinateurs**, vérifiez que vous avez activé **fonctionnalités avancées**. Pour ce faire, cliquez sur **Afficher**, recherchez le groupe **Admins du domaine** , cliquez avec le bouton droit sur le groupe, puis cliquez sur **Propriétés**.  
  
6. Cliquez sur l’onglet **sécurité** et cliquez sur **avancé** pour ouvrir la boîte **de dialogue Paramètres de sécurité avancés pour Admins du domaine** .  
  
   ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7. Sélectionnez **autoriser l’ACE pour le compte de gestion** , puis cliquez sur **modifier**. Vérifiez que le compte dispose uniquement des autorisations **lire les membres** et **écrire des membres** sur le groupe DA, puis cliquez sur **OK**.  
  
8. Cliquez sur **OK** dans la boîte de dialogue **paramètres de sécurité avancés** , puis cliquez à nouveau sur **OK** pour fermer la boîte de dialogue des propriétés du groupe DA.  
  
   ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. Vous pouvez répéter les étapes précédentes pour d’autres groupes protégés dans le domaine. les autorisations doivent être identiques pour tous les groupes protégés. Vous avez maintenant terminé la création et la configuration des comptes de gestion pour les groupes protégés dans ce domaine.  
  
    > [!NOTE]  
    > Tout compte qui a l’autorisation d’écrire l’appartenance à un groupe dans Active Directory peut également s’ajouter à ce groupe. Ce comportement est normal et ne peut pas être désactivé. Pour cette raison, vous devez toujours conserver les comptes de gestion désactivés lorsqu’ils ne sont pas utilisés et surveiller attentivement les comptes lorsqu’ils sont désactivés et lorsqu’ils sont en cours d’utilisation.  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>Vérification des paramètres de configuration de groupe et de compte

Maintenant que vous avez créé et configuré des comptes de gestion qui peuvent modifier l’appartenance à des groupes protégés dans le domaine (qui comprend les groupes EA, DA et BA les plus privilégiés), vous devez vérifier que les comptes et leur groupe d’administration ont été créés correctement. La vérification comprend les tâches générales suivantes :  
  
1.  Testez le groupe qui peut activer et désactiver les comptes de gestion pour vérifier que les membres du groupe peuvent activer et désactiver les comptes et réinitialiser leurs mots de passe, mais ne peuvent pas effectuer d’autres activités administratives sur les comptes de gestion.  
  
2.  Testez les comptes de gestion pour vérifier qu’ils peuvent ajouter et supprimer des membres dans des groupes protégés dans le domaine, mais ne peuvent pas modifier les autres propriétés des comptes et des groupes protégés.  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>Tester le groupe qui activera et désactivera les comptes de gestion
  
1.  Pour tester l’activation d’un compte de gestion et la réinitialisation de son mot de passe, connectez-vous à une station de travail d’administration sécurisée à l’aide d’un compte membre du groupe que vous avez créé à l' [annexe I : création de comptes de gestion pour les comptes et les groupes protégés dans Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Ouvrez **Active Directory utilisateurs et ordinateurs**, cliquez avec le bouton droit sur le compte de gestion, puis cliquez sur **activer le compte**.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Une boîte de dialogue doit s’afficher pour confirmer que le compte a été activé.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  Ensuite, réinitialisez le mot de passe sur le compte de gestion. Pour ce faire, cliquez à nouveau avec le bouton droit sur le compte, puis cliquez sur **Réinitialiser le mot de passe**.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Tapez un nouveau mot de passe pour le compte dans les champs **nouveau mot** de passe et **confirmer le mot de passe** , puis cliquez sur **OK**.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Une boîte de dialogue s’affiche, confirmant que le mot de passe du compte a été réinitialisé.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Essayez à présent de modifier les propriétés supplémentaires du compte de gestion. Cliquez avec le bouton droit sur le compte, cliquez sur **Propriétés**, puis sur l’onglet **contrôle à distance** .  
  
8.  Sélectionnez **activer le contrôle à distance** , puis cliquez sur **appliquer**. L’opération doit échouer et un message d’erreur d' **accès refusé** doit s’afficher.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Cliquez sur l’onglet **compte** pour le compte et tentez de modifier le nom du compte, les heures d’ouverture de session ou les stations de travail d’ouverture de session. Tout doit échouer, et les options de compte qui ne sont pas contrôlées par l’attribut **UserAccountControl** doivent être grisées et indisponibles pour modification.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Essayez d’ajouter le groupe d’administration à un groupe protégé, tel que le groupe DA. Lorsque vous cliquez sur **OK**, un message s’affiche, vous informant que vous ne disposez pas des autorisations nécessaires pour modifier le groupe.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Effectuez des tests supplémentaires si nécessaire pour vérifier que vous ne pouvez pas configurer quoi que ce soit sur le compte de gestion, à l’exception des paramètres **UserAccountControl** et des réinitialisations de mot de passe.  
  
    > [!NOTE]  
    > L’attribut **UserAccountControl** contrôle plusieurs options de configuration de compte. Vous ne pouvez pas accorder l’autorisation de modifier uniquement certaines des options de configuration lorsque vous accordez une autorisation d’accès en écriture à l’attribut.  
  
##### <a name="test-the-management-accounts"></a>Tester les comptes de gestion

Maintenant que vous avez activé un ou plusieurs comptes qui peuvent modifier l’appartenance à des groupes protégés, vous pouvez tester les comptes pour vous assurer qu’ils peuvent modifier l’appartenance à un groupe protégé, mais ne peuvent pas effectuer d’autres modifications sur les comptes et les groupes protégés.  
  
1.  Connectez-vous à un hôte d’administration sécurisé en tant que premier compte de gestion.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Lancez **Active Directory utilisateurs et ordinateurs** et recherchez le **groupe Admins du domaine**.  
  
3.  Cliquez avec le bouton droit sur le groupe **Admins du domaine** , puis cliquez sur **Propriétés**.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  Dans les **Propriétés Admins du domaine**, cliquez sur l’onglet **membres** , puis **sur** ajouter. Entrez le nom d’un compte disposant de privilèges d’administrateur de domaine temporaire, puis cliquez sur **vérifier les noms**. Lorsque le nom du compte est souligné, cliquez sur **OK** pour revenir à l’onglet **membres** .  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  Sous l’onglet **membres** de la boîte de dialogue **Propriétés de Admins du domaine** , cliquez sur **appliquer**. Après avoir cliqué sur **appliquer**, le compte doit rester membre du groupe DA et ne pas recevoir de messages d’erreur.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Cliquez sur l’onglet **géré par** dans la boîte de dialogue **Propriétés de Admins du domaine** et vérifiez que vous ne pouvez pas entrer de texte dans les champs et que tous les boutons sont grisés.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Cliquez sur l’onglet **général** dans la boîte de dialogue **Propriétés de Admins du domaine** et vérifiez que vous ne pouvez pas modifier les informations relatives à cet onglet.  
  
    ![création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Répétez ces étapes pour les groupes protégés supplémentaires, si nécessaire. Lorsque vous avez terminé, connectez-vous à un hôte d’administration sécurisé avec un compte membre du groupe que vous avez créé pour activer et désactiver les comptes de gestion. Ensuite, réinitialisez le mot de passe sur le compte de gestion que vous venez de tester et désactivez le compte. Vous avez terminé la configuration des comptes de gestion et le groupe qui sera responsable de l’activation et de la désactivation des comptes.  
