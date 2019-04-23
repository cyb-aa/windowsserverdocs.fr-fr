---
ms.assetid: 13fe87d9-75cf-45bc-a954-ef75d4423839
title: Annexe I - création de comptes de gestion des comptes protégés et des groupes dans Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c71b96f6c44cfc2b14b4c5d203f876e55cc728ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855630"
---
# <a name="appendix-i-creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Annexe i : Création de gestion de comptes pour les comptes protégés et groupes dans Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un des défis de l’implémentation d’un modèle d’Active Directory qui ne repose pas sur l’appartenance permanente aux groupes disposant de privilèges élevés est qu’il doit y avoir un mécanisme pour remplir ces groupes quand temporaire les adhésions aux groupes est nécessaire. Certaines solutions de gestion des identités privilégiées nécessitent que les comptes de service du logiciel ont été accordées à l’appartenance permanente aux groupes tels que DA ou les administrateurs dans chaque domaine dans la forêt. Toutefois, il n’est techniquement pas nécessaire pour les solutions de Privileged Identity Management (PIM) exécuter leurs services dans de tels contextes disposant de privilèges élevés.  
  
Cette annexe fournit des informations que vous pouvez utiliser pour les solutions PIM par des tiers ou en mode natif implémentées pour créer des comptes disposant de privilèges limités et peuvent être rigoureusement contrôlées, mais peuvent être utilisées pour remplir des groupes privilégiés dans Active Directory lorsque élévation temporaire est nécessaire. Si vous implémentez PIM de façon native, ces comptes peuvent servir par personnel d’administration pour effectuer le remplissage de groupe temporaire, et si vous implémentez PIM via un logiciel tiers, vous pourrez peut-être adapter ces comptes pour fonctionner en tant que service comptes.  
  
> [!NOTE]  
> Les procédures décrites dans cette annexe fournissent une approche à la gestion des groupes disposant de privilèges élevés dans Active Directory. Vous pouvez adapter ces procédures pour répondre à vos besoins, des restrictions supplémentaires, ou omettre certaines des restrictions qui sont décrites ici.  
  
## <a name="creating-management-accounts-for-protected-accounts-and-groups-in-active-directory"></a>Création de gestion de comptes pour les comptes protégés et groupes dans Active Directory

Création de comptes qui permettre servir à gérer l’appartenance des groupes privilégiés sans nécessiter les comptes de gestion à attribuer les droits excessifs et les autorisations se compose de quatre activités générales qui sont décrites dans les instructions pas à pas qui suivi :  
  
1.  Tout d’abord, vous devez créer un groupe qui gèrera les comptes, étant donné que ces comptes doivent être gérés par un ensemble limité d’utilisateurs approuvés. Si vous n’avez pas déjà d’une structure d’unité d’organisation qui prend en charge des comptes privilégiés et protégés différenciant et les systèmes à partir de la population en général dans le domaine, vous devez en créer une. Bien que les instructions spécifiques ne sont pas fournies dans cette annexe, captures d’écran montrent un exemple d’une telle hiérarchie d’unité d’organisation.  
  
2.  Créez les comptes de gestion. Ces comptes doivent être créés en tant que comptes d’utilisateur « standard » et aucun utilisateur des droits au-delà de ceux qui sont déjà accordées aux utilisateurs par défaut.  
  
3.  Implémenter des restrictions sur les comptes de gestion qui les rendent utilisable uniquement dans le but spécialisé pour lequel elles ont été créées, en plus de contrôler qui peut activer et utiliser les comptes (le groupe que vous avez créé dans la première étape).  
  
4.  Configurer les autorisations sur l’objet AdminSDHolder dans chaque domaine pour les comptes de gestion modifier l’appartenance des groupes privilégiés dans le domaine.  
  
Vous devez soigneusement tester l’ensemble de ces procédures et modifiez-les en fonction des besoins de votre environnement avant de les implémenter dans un environnement de production. Vous devez également vérifier que tous les paramètres fonctionnent comme prévu (certaines procédures de tests sont fournis dans cette annexe), et vous devez tester un scénario de récupération d’urgence dans lequel les comptes de gestion ne sont pas disponibles pour être utilisés pour remplir les groupes protégés pour la récupération à des fins. Pour plus d’informations sur la sauvegarde et restauration d’Active Directory, consultez le [AD DS Guide de sauvegarde et récupération pas à pas](https://technet.microsoft.com/library/cc771290(v=ws.10).aspx).  
  
> [!NOTE]  
> En implémentant les étapes décrites dans cette annexe, vous allez créer des comptes qui seront en mesure de gérer l’appartenance de tous les groupes protégés dans chaque domaine, non seulement les groupes Active Directory de privilège le plus élevé tels que EAs, DAs et des services BAs. Pour plus d’informations sur les groupes protégés dans Active Directory, consultez [annexe c : Comptes et groupes dans Active Directory protégés](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
### <a name="step-by-step-instructions-for-creating-management-accounts-for-protected-groups"></a>Obtenir des Instructions détaillées pour la création de comptes de gestion pour les groupes protégés  
  
#### <a name="creating-a-group-to-enable-and-disable-management-accounts"></a>Création d’un groupe pour activer et désactiver des comptes de gestion

Comptes de gestion doivent avoir leurs mots de passe réinitialisés à chaque utilisation et doivent être désactivées lorsque leur demandant des activités sont terminées. Bien que vous pouvez également envisager d’implémenter les conditions d’ouverture de session de carte à puce pour ces comptes, il est une configuration facultative, et ces instructions supposent que les comptes de gestion seront configurés avec un nom d’utilisateur et le mot de passe long et complexe en tant que valeur minimale contrôles. Dans cette étape, vous allez créer un groupe qui dispose des autorisations pour réinitialiser le mot de passe sur les comptes de gestion et à activer et désactiver les comptes.  
  
Pour créer un groupe pour activer et désactiver des comptes de gestion, procédez comme suit :  
  
1.  Dans la structure d’unité d’organisation où vous serez hébergeant les comptes de gestion, cliquez sur l’unité d’organisation où vous souhaitez créer le groupe, cliquez sur **New** et cliquez sur **groupe**.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_115.png)  
  
2.  Dans le **nouvel objet - groupe** boîte de dialogue, entrez un nom pour le groupe. Si vous envisagez d’utiliser ce groupe pour tous les comptes de gestion dans votre forêt « activer », rendre un groupe universel de sécurité. Si vous avez une forêt à domaine unique ou si vous envisagez de créer un groupe dans chaque domaine, vous pouvez créer un groupe de sécurité global. Cliquez sur **OK** pour créer le groupe.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_116.png)  
  
3.  Cliquez avec le bouton droit sur le groupe que vous venez de créer, cliquez sur **Propriétés**, puis sur l’onglet **Objet**. Dans le groupe **propriété d’objet** boîte de dialogue, sélectionnez **protéger l’objet des suppressions accidentelles**, ce qui empêchera les utilisateurs autorisés de supprimer le groupe, mais également de le déplacer vers une autre unité d’organisation, sauf si l’attribut est d’abord désélectionné.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_117.png)  
  
    > [!NOTE]  
    > Si vous avez déjà configuré des autorisations sur le parent du groupe d’unités d’organisation pour restreindre l’administration à un ensemble limité d’utilisateurs, vous devrez peut-être pas effectuer les étapes suivantes. Elles sont fournies ici afin que même si vous n’avez pas encore implémenté un contrôle limité sur la structure d’unité d’organisation dans lequel vous avez créé ce groupe, vous pouvez sécuriser le groupe contre la modification par les utilisateurs non autorisés.  
  
4.  Cliquez sur le **membres** onglet, puis ajoutez les comptes pour les membres de votre équipe qui seront chargées de l’activation de comptes de gestion ou le remplissage protégé groupes lorsque cela est nécessaire.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_118.png)  
  
5.  Si vous n'avez pas déjà fait, dans le **Active Directory Users and Computers** de la console, cliquez sur **vue** et sélectionnez **fonctionnalités avancées**. Cliquez sur le groupe que vous venez de créer, cliquez sur **propriétés**, puis cliquez sur le **sécurité** onglet. Sous l’onglet **Sécurité**, cliquez sur **Avancé**.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_119.png)  
  
6.  Dans le **Advanced Security Settings for [groupe]** boîte de dialogue, cliquez sur **désactiver l’héritage**. Lorsque vous y êtes invité, cliquez sur **convertir les autorisations héritées en autorisations explicites sur cet objet**, puis cliquez sur **OK** à retourner pour le groupe **sécurité** boîte de dialogue.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_120.png)  
  
7.  Sur le **sécurité** onglet, supprimez les groupes qui ne doivent pas être autorisées à accéder à ce groupe. Par exemple, si vous ne souhaitez pas les utilisateurs authentifiés pour être en mesure de lire le nom du groupe et les propriétés générales, vous pouvez supprimer cet ACE. Vous pouvez également supprimer les ACE, telles que celles pour les opérateurs et les versions antérieures de Windows 2000 Server compatible accès au compte. Vous devez, toutefois, conservez un ensemble minimal d’autorisations d’objet. Garantira l’ACE suivantes :  
  
    -   SELF  
  
    -   SYSTÈME  
  
    -   Administrateurs du domaine  
  
    -   Administrateurs de l’entreprise  
  
    -   Administrateurs  
  
    -   Groupe d’accès d’autorisation Windows (le cas échéant)  
  
    -   CONTRÔLEURS DE DOMAINE D’ENTREPRISE  
  
    Bien qu’il peut sembler non intuitive pour autoriser les groupes disposant de privilèges plus élevés dans Active Directory pour gérer ce groupe, votre objectif dans l’implémentation de ces paramètres est ne pas pour empêcher des membres de ces groupes d’apporter des modifications autorisées. Au lieu de cela, l’objectif est de vous assurer que lorsque vous avez l’occasion pour exiger des privilèges très élevés, modifications autorisées réussit. C’est pour cette raison que modification par défaut privilégiés imbrication de groupes, droits, et les autorisations sont déconseillées dans ce document. En conservant les structures de valeur par défaut et vider l’appartenance des groupes de privilège le plus élevés dans le répertoire, vous pouvez créer un environnement plus sécurisé qui fonctionne toujours comme prévu.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_121.png)  
  
    > [!NOTE]  
    > Si vous n’avez pas déjà configuré les stratégies d’audit pour les objets dans la structure d’unité d’organisation où vous avez créé ce groupe, vous devez configurer l’audit pour enregistrer les modifications de ce groupe.  
  
8.  Vous avez terminé la configuration du groupe qui sera utilisé pour « check out » gestion des comptes lorsqu’ils sont nécessaires et « archiver » les comptes lors de leurs activités ont été effectuées.  
  
#### <a name="creating-the-management-accounts"></a>Créer les comptes de gestion

Vous devez créer au moins un compte qui sera utilisé pour gérer l’appartenance des groupes privilégiés dans votre installation de Active Directory et, de préférence un deuxième compte pour servir de sauvegarde. Si vous choisissez Créer les comptes de gestion dans un domaine unique dans la forêt et leur accorder des fonctionnalités de gestion pour les domaines de tous les groupes protégés, ou si vous choisissez d’implémenter des comptes de gestion dans chaque domaine dans la forêt, les procédures sont en fait le même.  
  
> [!NOTE]  
> Les étapes de ce document supposent que vous n'avez pas encore implémenté les contrôles d’accès en fonction du rôle et privileged identity management pour Active Directory. Par conséquent, certaines procédures doivent être effectuées par un utilisateur dont le compte est membre du groupe Admins du domaine pour le domaine en question.  
>   
> Lorsque vous utilisez un compte avec des privilèges DA, vous pouvez vous connecter à un contrôleur de domaine pour effectuer les activités de configuration. Les étapes qui ne nécessitent pas de privilèges de DA peuvent être effectuées par les comptes de moins de privilèges qui sont connectés à des stations de travail d’administration. Captures d’écran montrant les boîtes de dialogue délimités en couleur bleu clair représentent des activités qui peuvent être effectuées sur un contrôleur de domaine. Captures d’écran qui affichent des boîtes de dialogue dans la couleur bleu foncée représentent des activités qui peuvent être effectuées sur des stations de travail d’administration avec des comptes disposant de privilèges limités.  
  
Pour créer les comptes de gestion, procédez comme suit :  
  
1. Ouvrez une session un contrôleur de domaine avec un compte qui est membre du groupe du domaine DA.  

2. Lancez **Active Directory Users and Computers** et accédez à l’unité d’organisation où vous allez créer le compte d’administration.  

3. Cliquez sur l’unité d’organisation et sur **New** et cliquez sur **utilisateur**.  

4. Dans le **nouvel objet - utilisateur** boîte de dialogue zone, entrez vos informations d’affectation de noms souhaitées pour le compte et cliquez sur **suivant**.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_122.png)  
  
5. Fournir un mot de passe initial pour le compte d’utilisateur, désactivez **utilisateur doit changer de mot de passe à la prochaine ouverture de session**, sélectionnez **utilisateur ne peut pas modifier le mot de passe** et **compte est désactivé**, et Cliquez sur **suivant**.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_123.png)  

6. Vérifiez que les détails du compte sont correctes, cliquez sur **Terminer**.  

7. Cliquez sur l’objet utilisateur que vous venez de créer et cliquez sur **propriétés**.  

8. Cliquez sur le **compte** onglet.  

9. Dans le **Options de compte** champ, sélectionnez le **compte est sensible et ne peut pas être délégué** indicateur, sélectionnez le **ce compte prend en charge le chiffrement Kerberos AES 128 bits** et/ou le **ce compte prend en charge le chiffrement Kerberos AES 256** indicateur, puis cliquez sur **OK**.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_124.png)  

   > [!NOTE]  
   > Étant donné que ce compte, comme les autres comptes, aura une fonction limitée, mais puissante, le compte doit uniquement être utilisé sur des hôtes d’administration sécurisées. Pour tous les hôtes d’administration sécurisées dans votre environnement, vous devez envisager d’implémenter le paramètre de stratégie de groupe **sécurité réseau : Configurer les types de chiffrement autorisés pour Kerberos** pour autoriser uniquement les types de chiffrement plus sécurisés, vous pouvez implémenter pour des hôtes sécurisés.  
   >
   > Implémentation des types de chiffrement plus sécurisés pour les ordinateurs hôtes ne limite pas les attaques de vol d’informations d’identification, l’utilisation appropriée et la configuration des hôtes sécurisées est le cas. La définition de types de chiffrement plus forts pour les hôtes qui sont uniquement utilisés par les comptes privilégiés réduit simplement la surface d’attaque globale des ordinateurs.  
   >
   > Pour plus d’informations sur la configuration des types de chiffrement sur les systèmes et les comptes, consultez [Configurations Windows pour le Type de chiffrement pris en charge Kerberos](http://blogs.msdn.com/b/openspecification/archive/2011/05/31/windows-configurations-for-kerberos-supported-encryption-type.aspx).  
   >
   > Ces paramètres sont pris en charge uniquement sur les ordinateurs exécutant Windows Server 2012, Windows Server 2008 R2, Windows 8 ou Windows 7.  
  
10. Sur le **objet** onglet, sélectionnez **protéger l’objet des suppressions accidentelles**. Cela uniquement empêchera pas l’objet en cours de suppression (même par les utilisateurs autorisés), mais vous empêche de s’est déplacé vers une autre unité d’organisation dans votre hiérarchie AD DS, à moins que la case à cocher est désactivée tout d’abord par un utilisateur avec l’autorisation de modifier l’attribut.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_125.png)  

11. Cliquez sur le **contrôle à distance** onglet.  

12. Effacer la **activer le contrôle à distance** indicateur. Il ne doit jamais être nécessaire pour le personnel de support pour vous connecter à des sessions de ce compte pour implémenter des corrections.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_126.png)  

   > [!NOTE]  
   > Chaque objet dans Active Directory doit avoir un propriétaire de l’informatique désigné et un propriétaire de l’entreprise désigné, comme décrit dans [planification des compromis](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md). Si vous effectuez le suivi la propriété des objets AD DS dans Active Directory (par opposition à une base de données externe), vous devez entrer les informations de la propriété appropriée dans les propriétés de cet objet.  
   >
   > Dans ce cas, le propriétaire de l’entreprise est très probablement une division informatique, andthere n’est pas l’interdiction des chefs d’entreprise étant également les propriétaires de l’informatique. Le point de mise en place de la propriété d’objets consiste à vous permettre d’identifier les contacts lorsque les modifications doivent être apportées aux objets, peut-être ans à partir de leur création initiale.  

13. Cliquez sur le **organisation** onglet.  

14. Entrez les informations requises dans les normes de votre objet AD DS.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_127.png)  

15. Cliquez sur le **Dial-in** onglet.  

16. Dans le **l’autorisation d’accès réseau** champ, sélectionnez **refuser l’accès**. Ce compte ne doit jamais besoin de se connecter via une connexion à distance.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_128.png)  

   > [!NOTE]  
   > Il est peu probable que ce compte sera utilisé pour vous connecter à des contrôleurs de domaine en lecture seule (RODC) dans votre environnement. Toutefois, doit-elle circonstance jamais requièrent que le compte pour vous connecter à un RODC, vous devez ajouter ce compte pour le groupe de réplication de mot de passe RODC refusée afin que son mot de passe n’est pas mis en cache sur le RODC.  
   >
   > Bien que le mot de passe doit être réinitialisé après chaque utilisation et le compte doit être désactivé, mise en œuvre de ce paramètre n’a pas un effet nuisible sur le compte, et cela peut vous aider dans les situations dans lesquelles un administrateur oublie réinitialiser le compte mot de passe et le désactiver.  

17. Cliquez sur l’onglet **Membre de**.  

18. Cliquez sur **Ajouter**.  

19. Type **groupe de réplication de mot de passe RODC refusée** dans le **sélectionner des utilisateurs, Contacts, ordinateurs** boîte de dialogue et cliquez sur **vérifier les noms**. Lorsque le nom du groupe est souligné dans le sélecteur d’objets, cliquez sur **OK** et vérifiez que le compte est maintenant un membre des deux groupes affichés dans la capture d’écran suivante. N’ajoutez pas le compte à des groupes protégés.  

20. Cliquez sur **OK**.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_129.png)  

21. Cliquez sur le **sécurité** onglet et cliquez sur **avancé**.  

22. Dans le **paramètres de sécurité avancés** boîte de dialogue, cliquez sur **désactiver l’héritage** et copier les autorisations héritées en autorisations explicites, puis cliquez sur **ajouter**.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_130.png)  

23. Dans le **entrée d’autorisation pour [compte]** boîte de dialogue, cliquez sur **sélectionner un principal** et ajoutez le groupe que vous avez créé dans la procédure précédente. Faites défiler vers le bas de la boîte de dialogue et cliquez sur **Effacer tout** pour supprimer toutes les autorisations par défaut.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_131.png)  

24. Faites défiler vers le haut de la **entrée d’autorisation** boîte de dialogue. Vérifiez que le **Type** liste déroulante est définie sur **autoriser**, puis, dans le **s’applique à** liste déroulante, sélectionnez **cet objet uniquement**.  

25. Dans le **autorisations** champ, sélectionnez **lire toutes les propriétés**, **des autorisations de lecture**, et **réinitialisation de mot de passe**.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_132.png)  

26. Dans le **propriétés** champ, sélectionnez **lire userAccountControl** et **écrire userAccountControl**.  

27. Cliquez sur **OK**, **OK** à nouveau dans le **paramètres de sécurité avancés** boîte de dialogue.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_133.png)  

   > [!NOTE]  
   > Le **userAccountControl** attribut contrôle plusieurs options de configuration de compte. Vous ne peut pas accorder l’autorisation de modifier uniquement certaines des options de configuration lorsque vous accordez l’autorisation d’écriture dans l’attribut.  

28. Dans le **noms d’utilisateur ou groupe** champ la **sécurité** onglet, supprimez les groupes qui ne doivent pas être autorisés à accéder ou de gérer le compte. Ne supprimez pas tous les groupes qui ont été configurés avec un ACE de refus, telles que le groupe tout le monde et SELF calculée compte (cet ACE a été défini lorsque le **utilisateur ne peut pas modifier le mot de passe** indicateur a été activé lors de la création du compte. Également ne supprimez pas le groupe que vous venez d’ajouter, le compte système ou des groupes tels que EA, DA, BA ou le groupe d’accès d’autorisation Windows.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_134.png)  

29. Cliquez sur **avancé** et vérifiez que la boîte de dialogue Paramètres de sécurité avancés est similaire à la capture d’écran suivante.  

30. Cliquez sur **OK**, et **OK** à nouveau pour fermer la boîte de dialogue Propriétés du compte.  

   ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_135.png)  

31. Le programme d’installation du premier compte de gestion est maintenant terminée. Vous allez tester le compte dans une procédure ultérieure.  

##### <a name="creating-additional-management-accounts"></a>Création de comptes d’administration supplémentaire

Vous pouvez créer des comptes de gestion supplémentaires en répétant les étapes précédentes, en copiant le compte que vous venez de créer ou en créant un script pour créer des comptes avec vos paramètres de configuration souhaitée. Toutefois, notez que si vous copiez le compte que vous venez de créer, la plupart des paramètres personnalisés et des ACL ne seront pas copiées vers le nouveau compte et vous devrez répéter la plupart des étapes de configuration.  
  
Vous pouvez créer à la place d’un groupe auquel vous déléguez les droits pour remplir et unpopulate groupes protégés, mais vous devez sécuriser le groupe et les comptes que vous placez dans celui-ci. Il doit y avoir très peu de comptes dans votre annuaire qui ont la possibilité de gérer les membres de groupes protégés, créer des comptes individuels peut être l’approche la plus simple.  
  
Quelle que soit la façon dont vous choisissez Créer un groupe dans lequel vous placez les comptes de gestion, vous devez vous assurer que chaque compte est sécurisé comme décrit précédemment. Vous devez également envisager d’implémenter des restrictions de stratégie de groupe similaires à ceux décrits dans [annexe d : Sécurisation des comptes d’administrateur intégré dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md).  
  
##### <a name="auditing-management-accounts"></a>L’audit des comptes de gestion

Vous devez configurer l’audit sur le compte pour vous connecter, au minimum, toutes les écritures dans le compte. Ainsi, que vous non seulement identifier réussie de l’activation du compte et de réinitialiser son mot de passe pendant les utilisations autorisées, mais également identifier les tentatives effectuées par les utilisateurs non autorisés pour manipuler le compte. Échec d’écritures sur le compte doivent être capturées dans votre système d’informations sur la sécurité et des événements de surveillance (SIEM) (le cas échéant) et doivent déclencher des alertes qui fournissent une notification au personnel responsable de l’examen des violations potentielles.  
  
Les solutions SIEM prennent des informations sur les événements de sécurité impliqués sources (par exemple, journaux des événements, données d’application, flux de réseau, aux produits anti-programme malveillant et sources de détection d’intrusion), la collecte des données et tentent de définir des vues intelligents et des mesures proactives . Il existe de nombreuses solutions SIEM commerciales et de nombreuses entreprises créer des implémentations privées. Un serveur SIEM bien conçu et implémenté de façon appropriée peut considérablement améliorer les capacités de réponse aux incidents et de surveillance de la sécurité. Toutefois, fonctionnalités et la précision varient considérablement entre les solutions. SIEM n’entrent pas dans le cadre de ce document, mais tenez compte les recommandations d’événement spécifique contenues par l’implémenteur de n’importe quel serveur SIEM.  
  
Pour plus d’informations sur les paramètres de configuration d’audit recommandés pour les contrôleurs de domaine, consultez [surveillance d’Active Directory pour les signes de compromission](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md). Paramètres de configuration spécifique au contrôleur de domaine sont fournies dans [surveillance d’Active Directory pour les signes de compromission](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md).  
  
#### <a name="enabling-management-accounts-to-modify-the-membership-of-protected-groups"></a>Activation de comptes d’administration modifier l’appartenance à des groupes protégés

Dans cette procédure, vous allez configurer les autorisations sur l’objet de AdminSDHolder du domaine pour autoriser les comptes d’administration nouvellement créé modifier l’appartenance à des groupes protégés dans le domaine. Cette procédure ne peut pas être effectuée via une interface utilisateur graphique (GUI).  
  
Comme indiqué dans [annexe c : Comptes et groupes dans Active Directory protégés](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md), l’ACL sur AdminSDHolder d’un domaine objet est effectivement « copié » pour les objets protégés lors de l’exécution de la tâche SDProp. Comptes et groupes protégés n’héritent pas de leurs autorisations de l’objet AdminSDHolder ; leurs autorisations sont définies explicitement correspondent à celles de l’objet AdminSDHolder. Par conséquent, lorsque vous modifiez les autorisations sur l’objet AdminSDHolder, vous devez les modifier pour les attributs qui sont appropriés pour le type de l’objet protégé que vous ciblez.  
  
Dans ce cas, vous autorisez les comptes d’administration nouvellement créé pour permettre à lire et écrire les membres d’attribut sur les objets de groupe. Toutefois, l’objet AdminSDHolder n’est pas un objet de groupe et attributs de groupe ne sont pas exposées dans l’éditeur ACL graphique. C’est pour cette raison que vous allez implémenter les modifications d’autorisations par le biais de l’utilitaire de ligne de commande Dsacls. Pour accorder des autorisations de comptes pour modifier l’appartenance à des groupes protégés la gestion (désactivée), procédez comme suit :  
  
1.  Ouvrez une session un contrôleur de domaine, de préférence le contrôleur de domaine détenant le rôle d’émulateur de contrôleur de domaine principal (PDCE), avec les informations d’identification d’un compte d’utilisateur qui a été apportée à un membre du groupe DA dans le domaine.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_136.png)  
  
2.  Ouvrez une invite de commandes avec élévation de privilèges en cliquant en **invite de commandes** et cliquez sur **exécuter en tant qu’administrateur**.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_137.gif)  
  
3.  Lorsque vous êtes invité à approuver l’élévation, cliquez sur **Oui**.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_138.gif)  
  
    > [!NOTE]  
    > Pour plus d’informations sur l’élévation et compte le contrôle utilisateur (UAC) dans Windows, consultez [processus et Interactions UAC](https://technet.microsoft.com/library/dd835561(v=WS.10).aspx) sur le site Web TechNet.  
  
4.  À l’invite de commandes, tapez (en substituant vos informations spécifiques à un domaine) **Dsacls [nom unique de l’objet AdminSDHolder dans votre domaine] /G [compte de gestion UPN] : RPWP ; membre**.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_139.gif)  
  
    La commande précédente (qui ne respecte pas la casse) fonctionne comme suit :  
  
    -   DSACLS définit ou affiche des ACE sur les objets d’annuaire  
  
    -   CN = AdminSDHolder, CN = System, DC = TailSpinToys, DC = msft identifie l’objet à modifier  
  
    -   /G indique qu’une instruction grant ACE est en cours de configuration  
  
    -   PIM001@tailspintoys.msft est le nom Principal utilisateur (UPN) du principal de sécurité qui recevra les ACE  
  
    -   RPWP accorde propriété autorisations lecture et écriture propriété  
  
    -   Membre est le nom de la propriété (attribut) sur lequel les autorisations sont définies  
  
    Pour plus d’informations sur l’utilisation de **Dsacls**, tapez Dsacls sans aucun paramètre à une invite de commandes.  
  
    Si vous avez créé plusieurs comptes de gestion pour le domaine, vous devez exécuter la commande Dsacls pour chaque compte. Lorsque vous avez terminé la configuration de la liste ACL sur l’objet AdminSDHolder, vous devez forcer SDProp pour exécuter ou attendez la fin de son exécution planifiée. Pour plus d’informations sur le forcé SDProp pour exécuter, consultez « En cours d’exécution manuelle d’un SDProp » dans [annexe c : Comptes et groupes dans Active Directory protégés](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    Lorsque SDProp a été exécutée, vous pouvez vérifier que les modifications apportées à l’objet AdminSDHolder ont été appliquées à des groupes protégés dans le domaine. Vous ne pouvez pas le vérifier en affichant la liste ACL sur l’objet AdminSDHolder pour les raisons décrites précédemment, mais vous pouvez vérifier que les autorisations ont été appliquées en affichant les ACL sur les groupes protégés.  
  
5.  Dans **Active Directory Users and Computers**, vérifiez que vous avez activé **fonctionnalités avancées**. Pour ce faire, cliquez sur **vue**, localisez le **Admins du domaine** de groupe, cliquez sur le groupe, cliquez sur **propriétés**.  
  
6.  Cliquez sur le **sécurité** onglet et cliquez sur **avancé** pour ouvrir le **Advanced Security Settings for Admins du domaine** boîte de dialogue.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_140.gif)  
  
7.  Sélectionnez **autoriser un ACE pour le compte d’administration** et cliquez sur **modifier**. Vérifiez que le compte a été accordé uniquement **lecture des membres** et **écriture des membres** autorisations sur le groupe de DA, puis cliquez sur **OK**.  
  
8.  Cliquez sur **OK** dans le **paramètres de sécurité avancés** boîte de dialogue, puis cliquez sur **OK** à nouveau pour fermer la boîte de dialogue de propriété pour le groupe de DA.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_141.gif)  
  
9. Vous pouvez répéter les étapes précédentes pour d’autres groupes protégés dans le domaine ; les autorisations doivent être les mêmes pour tous les groupes protégés. Vous avez maintenant terminé la création et configuration des comptes de gestion pour les groupes protégés dans ce domaine.  
  
    > [!NOTE]  
    > N’importe quel compte qui a l’autorisation d’écrire l’appartenance à un groupe dans Active Directory permettre également ajouter lui-même au groupe. Ce comportement est normal et ne peut pas être désactivé. Pour cette raison, vous devez toujours conserver les comptes de gestion désactivés inutilisés et devez surveiller attentivement les comptes lorsque s’ils sont désactivés et lorsqu’ils sont en cours d’utilisation.  
  
#### <a name="verifying-group-and-account-configuration-settings"></a>Vérification des paramètres de Configuration de compte et de groupe

Maintenant que vous avez créé et configuré des comptes de gestion qui peuvent modifier l’appartenance à des groupes protégés dans le domaine (qui inclut les groupes disposant de privilèges plus élevés de EA, DA et BA), vous devez vérifier que les comptes et leur groupe d’administration ont été créé correctement. Vérification consiste à ces tâches générales :  
  
1.  Tester le groupe qui peut activer et désactiver les comptes de gestion pour vérifier que les membres de groupe peut activent et désactiver les comptes et réinitialisent leurs mots de passe, mais ne peut pas exécuter d’autres activités d’administration sur les comptes de gestion.  
  
2.  Les comptes de gestion pour vérifier qu’ils peuvent ajouter et supprimer des membres protégés des groupes dans le domaine, mais ne peut pas modifier d’autres propriétés des comptes protégés et des groupes de test.  
  
##### <a name="test-the-group-that-will-enable-and-disable-management-accounts"></a>Tester le groupe permettant d’activer et désactiver les comptes de gestion
  
1.  Pour tester l’activation d’un compte de gestion et de réinitialiser son mot de passe, ouvrez une session une station d’administration sécurisée avec un compte qui est membre du groupe créé à le [annexe i Création de comptes de gestion pour protégé des comptes et groupes dans Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_142.gif)  
  
2.  Ouvrez **Active Directory Users and Computers**, cliquez sur le compte d’administration, puis cliquez sur **activer le compte**.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_143.gif)  
  
3.  Une boîte de dialogue à afficher, confirmant que le compte a été activé.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_144.gif)  
  
4.  Ensuite, réinitialiser le mot de passe sur le compte d’administration. Pour ce faire, cliquez à nouveau sur le compte et cliquez sur **réinitialiser le mot de passe**.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_145.gif)  
  
5.  Tapez un nouveau mot de passe pour le compte dans le **nouveau mot de passe** et **confirmer le mot de passe** champs, puis cliquez sur **OK**.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_146.gif)  
  
6.  Une boîte de dialogue doit apparaître, confirmant que le mot de passe pour le compte a été réinitialisé.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_147.gif)  
  
7.  Tenter de modifier d’autres propriétés du compte de gestion. Cliquez sur le compte et cliquez sur **propriétés**, puis cliquez sur le **contrôle à distance** onglet.  
  
8.  Sélectionnez **activer le contrôle à distance** et cliquez sur **appliquer**. L’opération doit échouer et une **accès refusé** message d’erreur doit afficher.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_148.gif)  
  
9. Cliquez sur le **compte** pour le compte de l’onglet et tentent de modifier le nom du compte, les heures d’ouverture de session ou des stations de travail d’ouverture de session. Tout doit échouer et compte les options qui ne sont pas contrôlées par le **userAccountControl** attribut doit être grisé et indisponible pour la modification.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_149.gif)  
  
10. Essayez d’ajouter le groupe d’administration à un groupe protégé, tels que le groupe de DA. Lorsque vous cliquez sur **OK**, un message doit apparaître, vous informant que vous ne disposez pas des autorisations pour modifier le groupe.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_150.gif)  
  
11. Effectuer des tests supplémentaires que nécessaire pour vérifier que vous ne pouvez pas configurer quoi que ce soit sur le compte d’administration, à l’exception **userAccountControl** paramètres et les réinitialisations de mot de passe.  
  
    > [!NOTE]  
    > Le **userAccountControl** attribut contrôle plusieurs options de configuration de compte. Vous ne peut pas accorder l’autorisation de modifier uniquement certaines des options de configuration lorsque vous accordez l’autorisation d’écriture dans l’attribut.  
  
##### <a name="test-the-management-accounts"></a>Les comptes de gestion de test

Maintenant que vous avez activé un ou plusieurs comptes qui peuvent modifier l’appartenance de groupes protégés, vous pouvez tester les comptes pour vous assurer qu’ils peuvent modifier l’appartenance au groupe protégé, mais ne peut pas effectuer les autres modifications sur les comptes protégés et groupes.  
  
1.  Connectez-vous à un hôte d’administration sécurisé en tant que le premier compte de gestion.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_151.gif)  
  
2.  Lancez **Active Directory Users and Computers** et recherchez le **groupe Admins du domaine**.  
  
3.  Cliquez sur le **Admins du domaine** de groupe et cliquez sur **propriétés**.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_152.gif)  
  
4.  Dans le **propriétés Admins du domaine**, cliquez sur le **membres** onglet et **cliquez sur** ajouter. Entrez le nom d’un compte qui aura des privilèges d’administrateurs du domaine temporaires, puis cliquez sur **vérifier les noms**. Lorsque le nom du compte est souligné, cliquez sur **OK** pour revenir à la **membres** onglet.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_153.gif)  
  
5.  Sur le **membres** onglet pour le **propriétés Admins du domaine** boîte de dialogue, cliquez sur **appliquer**. Après avoir cliqué sur **appliquer**, le compte doit être un membre du groupe DA et que vous ne devez recevoir aucun message d’erreur.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_154.gif)  
  
6.  Cliquez sur le **géré par** onglet dans le **propriétés Admins du domaine** boîte de dialogue zone et vérifiez que vous ne pouvez pas entrer le texte dans tous les champs et tous les boutons apparaissent en grisé.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_155.gif)  
  
7.  Cliquez sur le **général** onglet dans le **propriétés Admins du domaine** boîte de dialogue zone et vérifiez que vous ne pouvez pas modifier les informations sur cet onglet.  
  
    ![Création de comptes de gestion](media/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory/SAD_156.gif)  
  
8.  Répétez ces étapes pour les groupes protégés supplémentaires en fonction des besoins. Lorsque vous avez terminé, connectez-vous à un hôte d’administration sécurisé avec un compte qui est membre du groupe que vous avez créé pour activer et désactiver les comptes de gestion. Réinitialisez le mot de passe sur le compte d’administration que vous venez de tester et de désactivez le compte. Vous avez terminé le programme d’installation, les comptes de gestion et du groupe qui sera responsable de l’activation et désactivation de comptes.  
