<properties
	pageTitle="Version préliminaire d’Azure Active Directory B2C | Microsoft Azure"
	description="Comment créer une application web avec inscription, connexion et modification du mot de passe à l’aide d’Azure Active Directory B2C."
	services="active-directory-b2c"
	documentationCenter=".net"
	authors="dstrockis"
	manager="msmbaldwin"
	editor=""/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="04/07/2016"
	ms.author="dastrock"/>

# Version préliminaire d’Azure AD B2C : inscription et connexion dans une application web ASP.NET

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

Avec Azure Active Directory (Azure AD) B2C, vous pouvez ajouter à vos applications web en quelques étapes seulement des fonctionnalités de gestion des identités en libre-service performantes. Cet article explique comment créer une application web ASP.NET qui inclut l’inscription d’utilisateur, la connexion et la réinitialisation du mot de passe. L’application prendra en charge l’inscription et la connexion à l’aide d’un nom d’utilisateur, d’une adresse e-mail ou d’un compte de réseau social comme Facebook et Google.

Ce didacticiel diffère de [nos autres didacticiel web .NET](active-directory-b2c-devquickstarts-web-dotnet.md), car il utilise une [stratégie d’inscription ou de connexion](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy) pour assurer l’inscription et la connexion de l’utilisateur à l’aide d’un seul bouton au lieu de deux (un pour inscription et l’autre pour la connexion). En bref, une stratégie d’inscription ou de connexion permet aux utilisateurs de se connecter avec un compte existant s’ils en ont un, ou d’en créer un s’il s’agit de leur première utilisation de l’application.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Obtention d'un répertoire Azure AD B2C

Avant de pouvoir utiliser Azure AD B2C, vous devez créer un répertoire ou un client. Un répertoire est un conteneur destiné à recevoir tous vos utilisateurs, applications, groupes, etc. Si vous n’en avez pas encore, [créez un répertoire B2C](active-directory-b2c-get-started.md) avant d’aller plus loin dans ce guide.

## Création d'une application

Vous devez maintenant créer dans votre répertoire B2C une application fournissant à Azure AD certaines informations nécessaires pour communiquer de manière sécurisée avec votre application. Pour créer une application, suivez [ces instructions](active-directory-b2c-app-registration.md). Veillez à effectuer les opérations suivantes :

- Inclure une **application web/API web** dans l’application.
- Spécifier `https://localhost:44316/` comme **URI de redirection**. Il s’agit de l’URL par défaut pour cet exemple de code.
- Notez également l'**ID d'application** affecté à votre application. Vous en aurez besoin ultérieurement.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Création de vos stratégies

Dans Azure AD B2C, chaque expérience utilisateur est définie par une [stratégie](active-directory-b2c-reference-policies.md). Cet exemple de code contient deux expériences liées à l’identité : **l’inscription et la connexion**, et la **réinitialisation du mot de passe**. Vous devez créer une stratégie de chaque type, comme décrit dans l’[article de référence de stratégie](active-directory-b2c-reference-policies.md). Lors de la création des deux stratégies, assurez-vous de :

- Choisir **Inscription par le biais d’un ID utilisateur** ou **Inscription par le biais d’une adresse e-mail** dans le panneau des fournisseurs d’identité.
- Choisir le **Nom d’affichage** et d’autres attributs d’inscription dans votre stratégie d’inscription et de connexion.
- Choisir la revendication **Nom d’affichage** comme revendication d’application pour chaque stratégie. Vous pouvez aussi choisir d'autres revendications.
- Copier le **Nom** de chaque stratégie après l’avoir créée. Vous aurez besoin des noms de ces stratégies ultérieurement.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

Une fois vos deux stratégies créées, vous pouvez générer votre application.

## Télécharger le code et configurer l’authentification

Le code associé à cet exemple [est stocké sur GitHub](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI). Pour générer l’exemple à mesure que vous avancez, vous pouvez [télécharger la structure de projet sous la forme d’un fichier zip](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip). Vous pouvez également cloner la structure :

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

L’exemple complet est également [disponible en tant que fichier .zip](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip) ou sur la branche `complete` du même dépôt.

Une fois l’exemple de code téléchargé, ouvrez le fichier .sln Visual Studio pour commencer.

Votre application communique avec Azure AD B2C en envoyant des requêtes d’authentification HTTP qui indiquent la stratégie à exécuter dans le cadre de la requête. Pour les applications web .NET, vous pouvez utiliser la bibliothèque OWIN de Microsoft pour envoyer des requêtes d’authentification OpenID Connect, exécuter les stratégies, gérer les sessions utilisateur, etc.

Pour commencer, ajoutez au projet les packages NuGet du middleware OWIN à l’aide de la console du gestionnaire de package Visual Studio.

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Intsall-Package System.IdentityModel.Tokens.Jwt
```

Ensuite, ouvrez le fichier `web.config` à la racine du projet, puis entrez les valeurs de configuration de votre application dans la section `<appSettings>`, en remplaçant les valeurs ci-dessous par vos propres valeurs. Vous pouvez également laisser les valeurs `ida:RedirectUri` et `ida:AadInstance` inchangées.

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

Ajoutez ensuite une classe de démarrage OWIN au projet appelé `Startup.cs`. Cliquez avec le bouton droit sur le projet, sélectionnez **Ajouter** et **Nouvel élément**, puis recherchez « OWIN ». Remplacez la déclaration de classe par `public partial class Startup`. Nous avons déjà mis en œuvre une partie de cette classe pour vous dans un autre fichier. L’intergiciel OWIN appelle la méthode `Configuration(...)` lorsque votre application démarre. Dans cette méthode, appelez `ConfigureAuth(...)` pour configurer l’authentification de votre application.

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

Ouvrez le fichier `App_Start\Startup.Auth.cs` et implémentez la méthode `ConfigureAuth(...)`. Les paramètres que vous fournissez dans `OpenIdConnectAuthenticationOptions` servent de coordonnées pour que votre application puisse communiquer avec Azure AD. Vous devez également configurer l’authentification des cookies. Le middleware OpenID Connect utilise des cookies pour gérer les sessions utilisateur, entre autres.

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // The ACR claim is used to indicate which policy was executed
    public const string AcrClaimType = "http://schemas.microsoft.com/claims/authnclassreference";
    public const string PolicyKey = "b2cpolicy";
    public const string OIDCMetadataSuffix = "/.well-known/openid-configuration";

    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        OpenIdConnectAuthenticationOptions options = new OpenIdConnectAuthenticationOptions
        {
            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            { 
                AuthenticationFailed = AuthenticationFailed,
                RedirectToIdentityProvider = OnRedirectToIdentityProvider,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // The PolicyConfigurationManager takes care of getting the correct Azure AD authentication
            // endpoints from the OpenID Connect metadata endpoint.  It is included in the PolicyAuthHelpers folder.
            ConfigurationManager = new PolicyConfigurationManager(
                String.Format(CultureInfo.InvariantCulture, aadInstance, tenant, "/v2.0", OIDCMetadataSuffix),
                new string[] { SusiPolicyId, PasswordResetPolicyId }),

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {  
                NameClaimType = "name",
            },
        };

        app.UseOpenIdConnectAuthentication(options);
            
    }
    
...
```

## Envoyer des demandes d’authentification à Azure AD
Votre application est maintenant correctement configurée pour communiquer avec Azure AD B2C en utilisant le protocole d’authentification OpenID Connect. OWIN a pris en charge tous les détails de la création de messages d’authentification, de la validation des jetons d’Azure AD et de la gestion des sessions utilisateur. Il ne reste plus qu’à initier le flux de chaque utilisateur.

Quand un utilisateur sélectionne **Se connecter** ou **J’ai oublié mon mot de passe** dans l’application web, l’action associée est appelée dans `Controllers\AccountController.cs`. Dans tous les cas, vous pouvez utiliser les méthodes OWIN intégrées pour déclencher la bonne stratégie :

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by adding it to the AuthenticationProperties using the PolicyKey provided.

        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties (
                new Dictionary<string, string> 
                { 
                    {Startup.PolicyKey, Startup.SusiPolicyId}
                })
            { 
                RedirectUri = "/", 
            }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
    }
}

public void ResetPassword()
{
    HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties(
            new Dictionary<string, string>
            {
                {Startup.PolicyKey, Startup.PasswordResetPolicyId}
            })
        {
            RedirectUri = "/",
        }, OpenIdConnectAuthenticationDefaults.AuthenticationType);
}
```

Vous pouvez également utiliser une balise `PolicyAuthorize` personnalisée dans vos contrôleurs pour exiger l’exécution d’une stratégie spécifique si l’utilisateur n’est pas connecté. Ouvrez `Controllers\HomeController.cs` et ajoutez la balise `[PolicyAuthorize]` au contrôleur des revendications. Remplacez l’exemple de stratégie par votre propre stratégie d’inscription ou de connexion.

```C#
// Controllers\HomeController.cs

// You can use the PolicyAuthorize decorator to execute a certain policy if the user is not already signed into the app.
[PolicyAuthorize(Policy = "b2c_1_susi")]
public ActionResult Claims()
{
  ...
```

Vous pouvez également utiliser OWIN pour déconnecter l’utilisateur de l’application. Dans `Controllers\AccountController.cs` :

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request using the last policy that the user executed.
    // This is as easy as looking up the current value of the ACR claim, adding it to the AuthenticationProperties, and making an OWIN SignOut call.

    HttpContext.GetOwinContext().Authentication.SignOut(
        new AuthenticationProperties(
            new Dictionary<string, string> 
            { 
                {Startup.PolicyKey, ClaimsPrincipal.Current.FindFirst(Startup.AcrClaimType).Value}
            }), OpenIdConnectAuthenticationDefaults.AuthenticationType, CookieAuthenticationDefaults.AuthenticationType);
}
```

Par défaut, OWIN n’envoie pas les stratégies que vous spécifiez dans `AuthenticationProperties` à Azure AD. Toutefois, vous pouvez modifier les demandes qu'OWIN génère dans la notification `RedirectToIdentityProvider`. Utilisez cette notification dans `App_Start\Startup.Auth.cs` pour extraire le point de terminaison approprié pour chaque stratégie à partir des métadonnées de la stratégie. Cela garantit que la requête appropriée est envoyée vers Azure AD pour chaque stratégie que votre application souhaite exécuter.

```C#
// App_Start\Startup.Auth.cs

private async Task OnRedirectToIdentityProvider(RedirectToIdentityProviderNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    PolicyConfigurationManager mgr = notification.Options.ConfigurationManager as PolicyConfigurationManager;
    if (notification.ProtocolMessage.RequestType == OpenIdConnectRequestType.LogoutRequest)
    {
        OpenIdConnectConfiguration config = await mgr.GetConfigurationByPolicyAsync(CancellationToken.None, notification.OwinContext.Authentication.AuthenticationResponseRevoke.Properties.Dictionary[Startup.PolicyKey]);
        notification.ProtocolMessage.IssuerAddress = config.EndSessionEndpoint;
    }
    else
    {
        OpenIdConnectConfiguration config = await mgr.GetConfigurationByPolicyAsync(CancellationToken.None, notification.OwinContext.Authentication.AuthenticationResponseChallenge.Properties.Dictionary[Startup.PolicyKey]);
        notification.ProtocolMessage.IssuerAddress = config.AuthorizationEndpoint;
    }
}
```

## Afficher les informations utilisateur
Lors de l’authentification des utilisateurs avec OpenID Connect, Azure AD retourne un jeton d’ID à l’application qui contient des **revendications**. Il s’agit d’assertions concernant l’utilisateur. Vous pouvez utiliser des revendications pour personnaliser votre application.

Ouvrez le fichier `Controllers\HomeController.cs`. Vous pouvez accéder aux revendications de l’utilisateur dans vos contrôleurs par le biais du principal de sécurité `ClaimsPrincipal.Current`.

```C#
// Controllers\HomeController.cs

[PolicyAuthorize(Policy = "b2c_1_susi")]
public ActionResult Claims()
{
	Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
	ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

Vous pouvez accéder à toutes les revendications que votre application reçoit de la même façon. Une liste de toutes les revendications reçues par l’application est disponible dans la page **Revendications**.

## Exécution de l'exemple d'application

Enfin, vous pouvez générer et exécuter votre application. Inscrivez-vous à l’application à l’aide d’une adresse de messagerie ou d’un nom d’utilisateur. Déconnectez-vous, puis reconnectez-vous en tant que même utilisateur. Modifiez le profil de cet utilisateur. Déconnectez-vous et inscrivez-vous en tant qu’autre utilisateur. Notez que les informations affichées sous l’onglet **Revendications** correspondent aux informations configurées dans vos stratégies.

## Ajout d’IDP sociaux

Actuellement, l’application prend uniquement en charge l’inscription et la connexion d’utilisateurs avec des **comptes locaux**. Il s’agit de comptes stockés dans votre répertoire B2C qui utilisent un nom d’utilisateur et un mot de passe. Avec Azure AD B2C, vous pouvez ajouter la prise en charge d’autres **fournisseurs d’identité** sans modifier votre code.

Pour ajouter des fournisseurs d’identité sociaux à votre application, commencez par suivre les instructions détaillées figurant dans ces articles. Pour chaque fournisseur d’identité que vous souhaitez prendre en charge, vous devez inscrire une application dans ce système et obtenir un ID client.

- [Définir Facebook en tant qu’IDP](active-directory-b2c-setup-fb-app.md)
- [Définir Google en tant qu’IDP](active-directory-b2c-setup-goog-app.md)
- [Définir Amazon en tant qu’IDP](active-directory-b2c-setup-amzn-app.md)
- [Définir LinkedIn en tant qu’IDP](active-directory-b2c-setup-li-app.md)

Après avoir ajouté les fournisseurs d’identité à votre répertoire B2C, vous devez modifier chacune de vos trois stratégies pour inclure ces nouveaux fournisseurs, comme décrit dans l’[article de référence de stratégie](active-directory-b2c-reference-policies.md). Après avoir enregistré vos stratégies, exécutez à nouveau l’application. Les nouveaux fournisseurs d’identité doivent être ajoutés comme options d’inscription et de connexion dans chacune de vos expériences relatives à l’identité.

Vous pouvez tester vos stratégies et observer le résultat sur votre exemple d’application. Ajoutez ou supprimez des fournisseurs d’identité, manipulez des revendications d’application ou modifiez des attributs d’inscription. Faites des essais jusqu’à ce que vous compreniez la façon dont les stratégies, les requêtes d’authentification et la bibliothèque OWIN sont liées.

Pour référence, l’exemple complet (sans vos valeurs de configuration) [est fourni en tant que fichier .zip](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip). Vous pouvez également le cloner à partir de GitHub :

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->

<!---HONumber=AcomDC_0525_2016-->