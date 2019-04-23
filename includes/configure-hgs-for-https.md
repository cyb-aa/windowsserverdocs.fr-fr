Tout d’abord, obtenir un certificat SSL pour le service HGS à partir de votre autorité de certification. Chaque ordinateur hôte sera doivent approuver le certificat SSL, il est donc recommandé que vous émettez le certificat SSL à partir de votre entreprise publique clé infrastructure ou un tiers autorité de certification. N’importe quel certificat SSL pris en charge par IIS est pris en charge par SGH, toutefois **le nom du sujet du certificat doit correspondre le nom qualifié complet du service HGS** (nom de réseau distribué de cluster). Par exemple, si le domaine SGH est « bastion.local » et le nom de votre service SGH est « sgh », votre certificat SSL doit être émis pour « hgs.bastion.local ». Vous pouvez ajouter des noms DNS supplémentaires pour le champ d’autre nom de sujet du certificat si nécessaire.

Une fois que vous avez le protocole SSL de certificat, ouvrez une session PowerShell avec élévation de privilèges et fournissez le chemin d’accès de certificat lorsque vous exécutez [Set-HgsServer](https://technet.microsoft.com/itpro/powershell/windows/host-guardian-service/server/set-hgsserver):


```powershell
$sslPassword = Read-Host -AsSecureString -Prompt "SSL Certificate Password"
Set-HgsServer -Http -Https -HttpsCertificatePath 'C:\temp\HgsSSLCertificate.pfx' -HttpsCertificatePassword $sslPassword
```

Ou, si vous avez déjà installé le certificat dans le magasin de certificats local, vous pouvez le référencer par empreinte numérique :

```powershell
Set-HgsServer -Http -Https -HttpsCertificateThumbprint 'A1B2C3D4E5F6...'
```

> [!IMPORTANT]
> Configuration du service HGS avec un certificat SSL ne désactive pas le point de terminaison HTTP.
> Si vous souhaitez uniquement autoriser l’utilisation du point de terminaison HTTPS, configurez le pare-feu Windows pour bloquer les connexions entrantes vers le port 80.
> **Ne modifiez pas les liaisons IIS** pour les sites Web SGH supprimer le point de terminaison HTTP ; il est non pris en charge pour ce faire.
