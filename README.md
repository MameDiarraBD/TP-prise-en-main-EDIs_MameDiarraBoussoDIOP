# Boutique Diayma

## 1. Projets de la Solution

La solution `Diayma.sln` contient un seul projet :
- **Diayma** (Projet ASP.NET Core MVC)

## 2. Version SDK .NET

- **Version cible** : .NET Core 2.0 (`netcoreapp2.0`)
- **Packages principaux** :
  - `Microsoft.AspNetCore.All` version 2.0.6
  - `Microsoft.Extensions.Localization` version 2.1.1

## 3. Installation du SDK

Le SDK .NET a été installé via Homebrew :
- **Version installée** : .NET SDK 9.0.109 (version récente compatible)
- **Commande** : `brew install dotnet`

## 4. Bugs Identifiés

### Bug 1 : Calcul incorrect du total du panier
**Fichier** : `Models/Cart.cs`
**Méthode** : `GetTotalValue()` (ligne 51)
**Description** : La méthode retourne la somme des prix unitaires au lieu du total multiplié par les quantités.
**Ligne problématique** : `return GetCartLineList().Sum(x => x.Product.Price);`
**Correctif appliqué** : `return GetCartLineList().Sum(x => x.Product.Price * x.Quantity);`

### Bug 2 : NullReferenceException dans FindProductInCartLines
**Fichier** : `Models/Cart.cs`
**Méthode** : `FindProductInCartLines()` (ligne 65)
**Description** : Si le produit n'existe pas dans le panier, la méthode lance une `NullReferenceException` car elle tente d'accéder à `.Product` sur un objet null.
**Ligne problématique** : `return GetCartLineList().Where(x => x.Product.Id == productId).FirstOrDefault().Product;`
**Correctif appliqué** : `return GetCartLineList().FirstOrDefault(x => x.Product.Id == productId)?.Product;`

## 5. Points d'Arrêt (Breakpoints)

Les points d'arrêt suivants doivent être placés pour le débogage :

| Fichier | Ligne | Méthode |
|---------|-------|---------|
| `Components/CartSummaryViewComponent.cs` | 12 | `Invoke()` |
| `Controllers/ProductController.cs` | 15 | `Index()` |
| `Controllers/OrderController.cs` | 17 | `Index()` POST |
| `Controllers/CartController.cs` | 15 | `Index()` |
| `Startup.cs` | 20 | `ConfigureServices()` |

## 6. Trace d'Exécution - Affichage des Produits sur l'Écran d'Accueil

### Mode de débogage recommandé : "Pas à pas détaillé"

Lors du chargement de la page d'accueil (http://localhost:5000/), la trace d'exécution suit ce chemin :

1. **Startup.cs (ligne 20 - ConfigureServices)**
   - Classe : `Startup`
   - Méthode : `ConfigureServices(IServiceCollection services)`
   - Action : Configuration des services d'injection de dépendances

2. **ProductController.cs (ligne 15 - Index)**
   - Namespace : `P2FixAnAppDotNetCode.Controllers`
   - Classe : `ProductController`
   - Méthode : `IActionResult Index()`
   - Action : Invocation du contrôleur par défaut (route par défaut = Product/Index)

3. **ProductService.cs (GetAllProducts)**
   - Namespace : `P2FixAnAppDotNetCode.Models.Services`
   - Classe : `ProductService`
   - Méthode : `GetAllProducts()`
   - Action : Récupération de tous les produits depuis le référentiel

4. **ProductRepository.cs (GetAllProducts)**
   - Namespace : `P2FixAnAppDotNetCode.Models.Repositories`
   - Classe : `ProductRepository`
   - Méthode : `GetAllProducts()`
   - Action : Accès aux données des produits

5. **_Layout.cshtml**
   - Affichage du composant CartSummaryViewComponent

6. **CartSummaryViewComponent.cs (ligne 12 - Invoke)**
   - Namespace : `P2FixAnAppDotNetCode.Components`
   - Classe : `CartSummaryViewComponent`
   - Méthode : `IViewComponentResult Invoke()`
   - Action : Rendu du composant de résumé du panier

7. **Product/Index.cshtml**
   - Affichage final de la vue avec la liste des produits

## 7. Architecture et Flux de Données

L'application suit le pattern MVC avec injection de dépendances :

```
HttpRequest
    ↓
Startup (Configuration)
    ↓
ProductController (Contrôleur par défaut)
    ↓
ProductService (Logique métier)
    ↓
ProductRepository (Accès aux données)
    ↓
CartSummaryViewComponent (Composant UI)
    ↓
Views (Rendu HTML)
```

## 8. Déploiement

Le projet peut être déployé en tant qu'exécutable Windows autonome :

```bash
dotnet publish -c Release -r win-x64 --self-contained
```

L'exécutable généré sera disponible dans : `P2FixAnAppDotNetCode/bin/Release/netcoreapp2.0/win-x64/publish/`

## 9. Localisation

L'application supporte actuellement :
- Anglais (`en`, `en-US`, `en-GB`)
- Français (`fr`, `fr-FR`)
- **Wolof** (`wo`, `wo-SN`) - Nouveau support ajouté

Configuration dans `Startup.cs` :
```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-GB"),
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("wo-SN"),
    new CultureInfo("wo"),
};
```

### Fichiers de ressources Wolof créés :
- `Resources/Controllers/OrderController.wo.resx` - Textes du contrôleur de commandes
- `Resources/Models/ViewModels/Order.wo.resx` - Validations des modèles
- `Resources/Models/ViewModels/LanguageViewModel.wo.resx` - Noms des langues

## 10. Modification et Commits

### Commits effectués :
1. **Commit 1** : Correction des bugs dans Cart.cs (GetTotalValue et FindProductInCartLines)
2. **Commit 2** : Ajout du support pour la langue Wolof
3. **Commit 3** : Mise à jour de la documentation et configuration de déploiement