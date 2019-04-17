---
title: "Ajouter des noms de domaine de troisième niveau"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e5b4a362-1881-4024-ae4e-cc3b05e50103
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 64bf24e45155fdd981e2061b3de7ebce1c53b36c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="add-third-level-domain-names"></a>Ajouter des noms de domaine de troisième niveau

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials

Vous pouvez ajouter la capacité des utilisateurs à demander des noms de domaine de troisième niveau dans l’Assistant Configuration de nom de domaine. Pour cela, vous devez en créant et en installant un assembly de code qui est utilisé par le Gestionnaire de domaine dans le système d’exploitation.  
  
## <a name="create-a-provider-of-third-level-domain-names"></a>Créer un fournisseur de noms de domaine de troisième niveau  
 Vous pouvez mettre des noms de domaine de troisième niveau à la disposition en créant et en installant un assembly de code qui fournit les noms de domaine à l’Assistant. Pour ce faire, vous effectuez les tâches suivantes:  
  
-   [Ajout d’une implémentation de l’interface IDomainSignupProvider à l’assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup)  
  
-   [Ajout d’une implémentation de l’interface IDomainMaintenanceProvider à l’assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainMaintenance)  
  
-   [Signer l’assembly avec une signature Authenticode](Add-Third-Level-Domain-Names.md#BKMK_SignAssembly)  
  
-   [Installer l’assembly sur l’ordinateur de référence](Add-Third-Level-Domain-Names.md#BKMK_InstallAssembly)  
  
-   [Redémarrez le service de gestion des noms de domaine Windows Server](Add-Third-Level-Domain-Names.md#BKMK_RestartService)  
  
###  <a name="BKMK_DomainSignup"></a>Ajout d’une implémentation de l’interface IDomainSignupProvider à l’assembly  
 L’interface IDomainSignupProvider sert à ajouter des offres de domaine à l’Assistant.  
  
##### <a name="to-add-the-idomainsignupprovider-code-to-the-assembly"></a>Pour ajouter le code IDomainSignupProvider à l’assembly  
  
1.  Ouvrez Visual Studio2008 en tant qu’administrateur en cliquant sur le programme dans le **Démarrer** menu et en sélectionnant **exécuter en tant qu’administrateur**.  
  
2.  Cliquez sur **fichier**, cliquez sur **New**, puis cliquez sur **projet**.  
  
3.  Dans le **nouveau projet** boîte de dialogue, cliquez sur **Visual c#**, cliquez sur **bibliothèque de classes**, entrez un nom pour la solution, puis cliquez sur **OK**.  
  
4.  Renommez le fichier Class1.cs. Par exemple, MyDomainNameProvider.cs  
  
5.  Ajoutez des références aux fichiers Wssg.Web.DomainManagerObjectModel.dll, CertManaged.dll, WssgCertMgmt.dll et WssgCommon.dll.  
  
6.  Ajoutez le code suivant à l’aide des instructions.  
  
    ```c#  
  
    using System.Collections.ObjectModel;  
    using System.Net;  
    using System.Net.Sockets;  
    using Microsoft.WindowsServerSolutions.Certificates;  
    using Microsoft.WindowsServerSolutions.CertificateManagement;  
    using Microsoft.WindowsServerSolutions.Common;  
    using Microsoft.Win32;  
    ```  
  
7.  Modifier l’espace de noms et l’en-tête de classe pour faire correspondre l’exemple suivant.  
  
    ```c#  
    namespace Microsoft.WindowsServerSolutions.RemoteAccess.Domains  
    {  
        public class MyDomainNameProvider : IDomainSignupProvider  
        {  
        }  
    }  
    ```  
  
8.  Ajoutez la méthode Initialize et les variables requises à la classe, qui définit les offres présentées dans l’Assistant.  
  
    > [!NOTE]
    >  La méthode Initialize définit un identificateur pour le fournisseur de domaine qui doit être unique. Un moyen standard d’effectuer cette opération consiste à définir un GUID comme identificateur. Pour plus d’informations sur la création d’un GUID, voir [créer un Guid (guidgen.exe)](https://go.microsoft.com/fwlink/?LinkId=116098).  
  
     L’exemple de code suivant montre la méthode Initialize.  
  
    ```c#  
  
    static readonly Guid MyID = new Guid("8C999DF5-696A-47af-822D-94F1673D3397");  
    public Guid ID { get { return MyID; } }  
    public string Name { get { return "My Provider"; } }  
    List<Offering> offerings = new List<Offering>();  
  
    public void Initialize(DomainProviderSettings config)  
    {  
       var offer1 = new Offering()  
       {  
          Description = "My Domain Provider",  
          Name = "Offering 1",  
          ProviderID = ID,  
          MoreInfoUrl = new Uri("http://www.contoso.com"),  
          MembershipServiceName = "My Membership",  
          EulaUrl = new Uri("http://www.contoso.com"),  
       };  
  
       this.offerings.Add(offer1);  
       RegistryKey key =   
          Registry.LocalMachine.CreateSubKey(@"Software\Microsoft\Windows Server\Domain Manager\Settings");  
       key.Close();  
    }  
    ```  
  
9. Ajoutez la méthode GetOfferings, qui retourne la liste des offres initialisée à l’étape précédente. L’exemple de code suivant montre la méthode GetOfferings.  
  
    ```c#  
  
    public ReadOnlyCollection<Offering> GetOfferings()  
    {  
       return this.offerings.AsReadOnly();  
    }  
    ```  
  
10. Ajoutez la méthode FindOfferingForDomain, qui retourne l’offre dans la liste. L’exemple de code suivant montre la méthode FindOfferingForDomain.  
  
    ```  
  
    public Offering FindOfferingForDomain(string domain)  
    {  
       // Return the offering that has the domain name.  
       return offerings[0];  
    }  
  
    ```  
  
11. Ajoutez la méthode SetCredentials, qui définit les informations d’identification qui sont nécessaires pour accéder aux offres. L’exemple de code suivant montre la méthode SetCredentials.  
  
    ```c#  
  
    private string currentUser { get; set; }  
    private string currentPassword { get; set; }  
  
    public bool SetCredentials(DomainNameRequest request,   
       DomainProviderCredentials credentials, bool validate)  
    {  
       currentUser = credentials.UserName;  
       currentPassword = credentials.Password;  
       if (validate)  
       {  
          return ValidateCredentials();  
       }  
  
       return true;  
    }  
    ```  
  
12. Ajoutez la méthode ValidateCredentials afin de valider les informations d’identification définies par SetCredentials. L’exemple de code suivant montre la méthode ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (IsUser())  
          return string.Equals(currentPassword, offerPassword);  
       else  
          return false;  
    }   
  
    private bool IsUser()  
    {  
       return string.Equals(currentUser, offerUser, StringComparison.OrdinalIgnoreCase);  
    }  
    ```  
  
13. Ajoutez la méthode GetAvailableDomainRoots, qui retourne la liste des noms de domaine racine qui sont pris en charge par l’offre spécifiée dans la demande. Cette liste de noms de domaine racine ne doit pas être vide. L’exemple de code suivant montre la méthode GetAvailableDomainRoots.  
  
    ```c#  
  
    public ReadOnlyCollection<string> GetAvailableDomainRoots(DomainNameRequest request)  
    {  
       List<string> list = new List<string>();  
       list.Add("domain1.com");  
       list.Add("domain1.org");  
  
       return list.AsReadOnly();  
    }  
    ```  
  
14. Ajoutez la méthode GetUserDomainNames, qui retourne la liste des noms de domaine que l’utilisateur actuel possède déjà par rapport à l’offre en cours. Cette liste peut être vide. L’exemple de code suivant montre la méthode GetUserDomainNames.  
  
    ```c#  
  
    public static readonly string AvailableDomain1 = "available.domain1.com",  
       AvailableDomain2 = "available.domain2.com";  
    public static readonly string OccupiedDomain1 = "occupied.domain1.com",  
       OccupiedDomain2 = "occupied.domain2.com";  
  
    public ReadOnlyCollection<string> GetUserDomainNames(DomainNameRequest request)  
    {  
       var userDomains = new List<string>();  
       userDomains.Add(OccupiedDomain1);  
       userDomains.Add(AvailableDomain1);  
  
       return userDomains.AsReadOnly();  
    }  
    ```  
  
15. Ajoutez la méthode GetUserDomainQuota, qui retourne le nombre maximal de domaines l’offre spécifiée. Si aucun maximum n’est applicable, cette méthode renvoie 0. L’exemple suivant montre la méthode GetUserDomainQuota.  
  
    ```c#  
  
    public int GetUserDomainQuota(DomainNameRequest request)  
    {  
       return 0;  
    }  
    ```  
  
16. Ajoutez la méthode CheckDomainAvailability, qui vérifie la disponibilité du nom de domaine demandé et peut retourner une liste de suggestions. L’exemple de code suivant montre la méthode CheckDomainAvailability.  
  
    ```c#  
  
    public bool CheckDomainAvailability(DomainNameRequest request,   
       out ReadOnlyCollection<string> suggestions)  
    {  
       suggestions = null;  
  
       return true;  
    }  
    ```  
  
17. Ajoutez la méthode CommitDomain qui valide le nom de domaine demandé. Réussite de cette méthode implique que le nom de domaine est associé au compte d’utilisateur, le fournisseur de maintenance est appelé immédiatement pour récupérer le certificat si l’état est opérationnel (fullyoperational), et le fournisseur et l’offre deviennent actives. L’exemple de code suivant montre la méthode CommitDomain.  
  
    ```c#  
  
    public DomainStatus CommitDomain(DomainNameRequest request)  
    {              
       ReadOnlyCollection<string> suggestions;  
       if (!CheckDomainAvailability(request, out suggestions))  
       {  
          throw new DomainException(FailureReason.InvalidDomainName, null, null);  
       }  
  
       return DomainStatus.Ready;  
    }  
    ```  
  
18. Ajoutez la méthode ReleaseDomain, qui informe le fournisseur que l’utilisateur souhaite publier le nom de domaine. L’exemple de code suivant montre la méthode ReleaseDomain.  
  
    ```c#  
  
    public bool ReleaseDomain(DomainNameRequest request)  
    {  
       return true;  
    }  
    ```  
  
19. Ajoutez la méthode GetProviderLandingUrl, qui renvoie l’URL de la page d’accueil dans le flux de travail d’abonnement à un domaine. L’exemple de code suivant montre la méthode GetProviderLandingUrl.  
  
    ```c#  
  
    public Url GetProviderLandingUrl(DomainNameRequest request)  
    {  
       return new Url("www.contoso.com");  
    }  
    ```  
  
20. Ajoutez la méthode GetDomainMaintenanceProvider, qui renvoie une instance de IDomainMaintenanceProvider qui est utilisé pour les tâches de maintenance de domaine. Cette méthode est appelée une fois la méthode commitdomain exécutée, et lorsque le Gestionnaire de domaine démarre. L’exemple de code suivant montre la méthode GetDomainMaintenanceProvider.  
  
    ```c#  
  
    public IDomainMaintenanceProvider GetDomainMaintenanceProvider()  
    {  
       return new MyDomainMaintenanceProvider();  
    }  
    ```  
  
21. Enregistrez le projet et laissez-le ouvert, car vous lui ajouterez à la procédure suivante. Vous ne serez pas en mesure de générer le projet jusqu'à ce que vous effectuez la procédure suivante.  
  
###  <a name="BKMK_DomainMaintenance"></a>Ajout d’une implémentation de l’interface IDomainMaintenanceProvider à l’assembly  
 Le IDomainMaintenanceProvider est utilisé pour gérer le domaine après sa création.  
  
##### <a name="to-add-the-idomainmaintenanceprovider-code-to-the-assembly"></a>Pour ajouter le code IDomainMaintenanceProvider à l’assembly  
  
1.  Ajoutez l’en-tête de classe pour le fournisseur de maintenance du domaine. Assurez-vous que le nom que vous définissez pour le fournisseur correspond au nom de la méthode GetDomainMaintenanceProvider définie précédemment.  
  
    ```c#  
  
    public class MyDomainMaintenanceProvider : IDomainMaintenanceProvider  
    {  
    }  
    ```  
  
2.  Ajoutez la méthode Activate, qui définit le fournisseur actif. L’exemple de code suivant montre la méthode Activate.  
  
    ```c#  
  
    string DomainName { get; set; }  
    protected DomainProviderSettings Settings { get; set; }  
  
    public void Activate(DomainProviderSettings settings,   
       DomainNameConfiguration config, DomainProviderCredentials credentials)  
    {  
       Settings = settings;  
       SetCredentials(credentials);  
       DomainName = config.AutoConfiguredAnywhereAccessFullName.Punycode;  
    }  
    ```  
  
3.  Ajoutez la méthode Deactivate, qui est utilisée pour désactiver toutes les actions. L’exemple de code suivant montre la méthode Deactivate.  
  
    ```  
  
    public void Deactivate()  
    {  
       //Deactivate all actions  
    }  
  
    ```  
  
4.  Ajoutez la méthode SetCredentials, qui met à jour les informations d’identification de l’utilisateur. Par exemple, cette méthode peut être appelée pour mettre à jour les informations d’identification qui ne sont plus valides. L’exemple de code suivant montre la méthode SetCredentials.  
  
    ```c#  
  
    protected DomainProviderCredentials Credentials { get; set; }  
  
    public bool SetCredentials(DomainProviderCredentials credentials)  
    {  
       this.Credentials = credentials;  
  
       return true;  
    }  
    ```  
  
5.  Ajoutez la méthode ValidateCredentials afin de valider les informations d’identification spécifié. L’exemple de code suivant montre la méthode ValidateCredentials.  
  
    ```c#  
  
    public static readonly string offerUser = "user1@contoso.com";  
    public static readonly string offerPassword = "password1";  
  
    public bool ValidateCredentials()  
    {  
       if (string.Equals(this.Credentials.UserName,   
          offerUser,   
          StringComparison.OrdinalIgnoreCase) &&   
             string.Equals(this.Credentials.Password, offerPassword))  
          return true;  
  
       return false;  
    }  
    ```  
  
6.  Ajoutez la méthode GetPublicAddress, qui retourne l’adresse IP externe du serveur. L’exemple de code suivant montre la méthode GetPublicAddress.  
  
    ```c#  
  
    public IPAddress GetPublicIPAddress()  
    {  
       string PublicIP = "0.0.0.0";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          PublicIP = (key == null) ? "0.0.0.0" : key.GetValue("PublicIP", "0.0.0.0").ToString();  
       }  
       IPAddress ip = IPAddress.Parse(PublicIP);  
  
       if (PublicIP == "0.0.0.0")  
       {  
          string strHostName = Dns.GetHostName();  
          IPHostEntry ipEntry = Dns.GetHostEntry(strHostName);  
  
          IPAddress[] addr = ipEntry.AddressList;  
          foreach (IPAddress add in addr)  
          {  
             if (add.AddressFamily == AddressFamily.InterNetwork)  
             {  
                return add;  
             }  
          }  
       }  
       else  
       {  
          return IPAddress.Parse(PublicIP);  
       }  
  
       return null;    
    }  
    ```  
  
7.  Ajoutez la méthode submitcertificaterequest afin de soumettre, qui envoie la demande de certificat pour le nom de domaine actuellement configuré.  
  
    ```c#  
  
    string cert=null;  
  
    public void SubmitCertificateRequest(string certificateRequest)  
    {  
       cert = CertManaged.SubmitRequest(certificateRequest, CertCommon.CAServerFQDN + "\\" +      
          CertCommon.CAName,   
          Microsoft.WindowsServerSolutions.CertificateManagement.CRFlags.Base64Header,   
          CertCommon.CATemplate,   
          EncodingFlags.Base64);  
    }  
    ```  
  
8.  Ajoutez la méthode GetCertificateResponse, qui retourne la réponse de certificat si l’état du domaine soit FullyOperational. Cette méthode est appelée pour les deux nouvelles demandes de certificat et pour les demandes de renouvellement de certificat. L’exemple de code suivant montre la méthode GetCertificateResponse.  
  
    ```c#  
  
    public string GetCertificateResponse(bool renew)  
    {  
       return cert;  
    }  
    ```  
  
9. Ajoutez la méthode submitrenewcertificaterequest afin de traiter le renouvellement du certificat. L’exemple de code suivant montre la méthode SubmitRenewCertificateRequest.  
  
    ```c#  
  
    public void SubmitRenewCertificateRequest()  
    {  
       // Add certificate renewal code   
    }  
    ```  
  
10. Ajoutez la méthode updatednsrecords afin de mettre à jour les enregistrements DNS stockés par le fournisseur. L’exemple de code suivant montre la méthode UpdateDNS.  
  
    ```c#  
  
    public bool UpdateDnsRecords(IList<DnsRecord> records)  
    {  
       string UpdateDNS = "true";  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          UpdateDNS = (key == null) ? "true" : key.GetValue("UpdateDNS", "true").ToString();  
       }  
  
       return UpdateDNS == "true";  
    }  
  
    ```  
  
11. Ajoutez la méthode TestConnection, qui tente d’établir une connexion au service principal. Si cette méthode requiert une authentification, une exception appropriée doit être levée. L’exemple de code suivant montre la méthode TestConnection.  
  
    ```c#  
  
    public bool TestConnection()  
    {  
       // Add code to test connection  
  
       return true;  
    }  
    ```  
  
12. Ajoutez la méthode GetDomainState qui retourne l’état actuel du domaine. L’exemple de code suivant montre la méthode GetDomainState.  
  
    ```c#  
  
    public DomainState GetDomainState()  
    {  
       string domainstatus = "FullyOperational";  
       long expirationDate = 365;  
       using (RegistryKey key = ProductInfo.RegKey.OpenSubKey("Domain Manager\\Settings", true))  
       {  
          domainstatus = (key == null) ? "Ready" : key.GetValue("DomainStatus", "Ready").ToString();  
          expirationDate = Int64.Parse(key.GetValue("ExpirationDate", "365").ToString());  
       }  
  
       switch (domainstatus)  
       {  
          case "Failed":  
             return new DomainState(DomainStatus.Failed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Ready":  
             return new DomainState(DomainStatus.Ready,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewal":  
             return new DomainState(DomainStatus.InRenewal,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "InRenewalCustomerInterventionRequired":  
             return new DomainState(DomainStatus.InRenewalCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "Pending":  
             return new DomainState(DomainStatus.Pending,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "PendingCustomerInterventionRequired":  
             return new DomainState(DomainStatus.PendingCustomerInterventionRequired,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          case "RenewalFailed":  
             return new DomainState(DomainStatus.RenewalFailed,   
                null,   
                DateTime.Now.AddDays(expirationDate));  
          default:  
             return new DomainState(DomainStatus.Unknown,   
                null,   
                DateTime.Now.AddDays(expirationDate));                   
          }  
    }  
    ```  
  
13. Ajoutez la méthode GetCertificateState qui retourne l’état actuel du certificat. L’exemple de code suivant montre la méthode GetCertificateState.  
  
    ```c#  
  
    public CertificateState GetCertificateState(bool renew)  
    {  
       return new CertificateState(CertificateStatus.Ready, string.Empty);  
    }  
    ```  
  
14. Enregistrez et générez la solution.  
  
###  <a name="BKMK_SignAssembly"></a>Signer l’assembly avec une signature Authenticode  
 Vous devez signer l’assembly pour pouvoir être utilisé dans le système d’exploitation. Pour plus d’informations sur la signature de l’assembly, voir [signature et vérification du Code avec Authenticode](https://msdn.microsoft.com/library/ms537364\(VS.85\).aspx#SignCode).  
  
###  <a name="BKMK_InstallAssembly"></a>Installer l’assembly sur l’ordinateur de référence  
 Placez l’assembly dans un dossier sur l’ordinateur de référence. Notez le chemin d’accès du dossier, car vous devrez entrer cette information dans le Registre à l’étape suivante.  
  
### <a name="add-a-key-to-the-registry"></a>Ajouter une clé au Registre  
 Vous ajoutez une entrée de Registre pour définir les caractéristiques et l’emplacement de l’assembly.  
  
##### <a name="to-add-a-key-to-the-registry"></a>Pour ajouter une clé au Registre  
  
1.  Sur l’ordinateur de référence, cliquez sur **Démarrer**, entrez **regedit**, puis appuyez sur **entrée**.  
  
2.  Dans le volet gauche, développez **HKEY_LOCAL_MACHINE**, développez **logiciel**, développez **Microsoft**, développez **Windows Server**, développez **Domain Managers**, puis développez **fournisseurs**.  
  
3.  Avec le bouton droit **fournisseurs**, pointez sur **New**, puis cliquez sur **clé**.  
  
4.  Tapez l’identificateur du fournisseur en tant que le nom de la clé. L’identificateur est le GUID que vous avez défini pour le fournisseur à l’étape8 de [Ajout d’une implémentation de l’interface IDomainSignupProvider à l’assembly](Add-Third-Level-Domain-Names.md#BKMK_DomainSignup).  
  
5.  Cliquez sur la clé que vous venez de créé, puis cliquez sur **valeur de chaîne**.  
  
6.  Type **Assembly** pour le nom de la chaîne, puis appuyez sur **entrée**.  
  
7.  Cliquez sur le nouveau **Assembly** chaîne dans le volet droit, puis cliquez sur **modifier**.  
  
8.  Tapez le chemin d’accès complet au fichier d’assembly que vous avez créé précédemment, puis cliquez sur **OK**.  
  
9. Cliquez à nouveau sur la clé, puis cliquez sur **valeur de chaîne**.  
  
10. Type **activé** pour le nom de la chaîne, puis appuyez sur **entrée**.  
  
11. Cliquez sur le nouveau **activé** chaîne dans le volet droit, puis cliquez sur **modifier**.  
  
12. Type **True**, puis cliquez sur **OK**.  
  
13. Cliquez à nouveau sur la clé, puis cliquez sur **valeur de chaîne**.  
  
14. Type **Type** pour le nom de la chaîne, puis appuyez sur **entrée**.  
  
15. Cliquez sur le nouveau **Type** chaîne dans le volet droit, puis cliquez sur **modifier**.  
  
16. Tapez le nom complet de la classe de votre fournisseur défini dans l’assembly, puis cliquez sur **OK**.  
  
###  <a name="BKMK_RestartService"></a>Redémarrez le service de gestion des noms de domaine Windows Server  
 Vous devez redémarrer le service de gestion de domaine Windows Server pour le fournisseur soit disponible pour le système d’exploitation.  
  
##### <a name="restart-the-service"></a>Redémarrez le service  
  
1.  Cliquez sur **Démarrer**, type **mmc**, puis appuyez sur **entrée**.  
  
2.  Si le composant logiciel enfichable Services n’est pas répertorié dans la console, ajoutez-le en effectuant les étapes suivantes:  
  
    1.  Cliquez sur **fichier**, puis cliquez sur **ajouter/supprimer un composant logiciel enfichable**.  
  
    2.  Dans le **des composants logiciels enfichables disponibles**, cliquez sur **Services**, puis cliquez sur **ajouter**.  
  
    3.  Dans le **Services** boîte de dialogue zone, vérifiez que **ordinateur local** est sélectionné, puis cliquez sur **Terminer**.  
  
    4.  Cliquez sur **OK** pour fermer la **ajouter ou supprimer les composants logiciels enfichables** boîte de dialogue.  
  
3.  Double-cliquez sur **Services**, faites défiler et sélectionnez **gestion de domaine Windows Server**, puis cliquez sur **redémarrer le service**.  
  
## <a name="see-also"></a>Voir aussi  
 [Création et personnalisation de l’Image](Creating-and-Customizing-the-Image.md)   
 [Personnalisations supplémentaires](Additional-Customizations.md)   
 [Préparation de l’Image pour le déploiement](Preparing-the-Image-for-Deployment.md)   
 [Test de l’expérience client](Testing-the-Customer-Experience.md)