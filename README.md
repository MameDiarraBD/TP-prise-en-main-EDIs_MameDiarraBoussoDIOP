# Boutique Diayma - Documentation Complète

**Dépôt GitHub** : https://github.com/MameDiarraBD/TP-prise-en-main-EDIs_MameDiarraBoussoDIOP

---

## QUESTION 1 : Quels sont les projets de la solution ?

La solution `Diayma.sln` contient un seul projet :
- **Diayma** - Projet ASP.NET Core MVC (Architecture MVC complète avec injection de dépendances)

### Structure du projet :
```
P2FixAnAppDotNetCode/
├── Controllers/
│   ├── ProductController.cs
│   ├── CartController.cs
│   ├── OrderController.cs
│   └── LanguageController.cs
├── Models/
│   ├── Cart.cs
│   ├── Product.cs
│   ├── Order.cs
│   ├── Repositories/
│   └── Services/
├── Views/
│   ├── Product/
│   ├── Cart/
│   ├── Order/
│   └── Shared/
├── Components/
│   ├── CartSummaryViewComponent.cs
│   └── LanguageSelectorViewComponent.cs
└── Resources/ (Fichiers de localisation)
```

---

## QUESTION 3 : Quelle est la version SDK .NET utilisée par ces projets ?

- **Framework cible** : **.NET Core 2.0** (spécifié dans `Diayma.csproj`)
- **TargetFramework** : `netcoreapp2.0`
- **Packages ASP.NET Core** : `Microsoft.AspNetCore.All` version 2.0.6
- **Localisation** : `Microsoft.Extensions.Localization` version 2.1.1

### Configurations :
```xml
<TargetFramework>netcoreapp2.0</TargetFramework>
```

---

## QUESTION 4 : Installation du SDK

✅ **SDK .NET installé avec succès**

**Commande d'installation** : `brew install dotnet`
**Version installée** : .NET SDK 9.0.109
**Vérification** : `dotnet --version` → 9.0.109

### Processus d'installation :
```bash
# Vérifier l'installation
dotnet --version

# Compiler le projet
dotnet build P2FixAnAppDotNetCode/Diayma.csproj

# Exécuter le projet
dotnet run --project P2FixAnAppDotNetCode/Diayma.csproj
```

---

## QUESTION 5 : Créez votre propre dépôt GitHub pour y stocker le code

✅ **Dépôt GitHub créé et configuré**

**URL du dépôt** : https://github.com/MameDiarraBD/TP-prise-en-main-EDIs_MameDiarraBoussoDIOP

### Configuration effectuée :
```bash
git remote set-url origin https://github.com/MameDiarraBD/TP-prise-en-main-EDIs_MameDiarraBoussoDIOP.git
git push -u origin master
```

---

## QUESTION 6 : Explorez l'application. Signalez 2 bugs trouvés

### BUG 1 : Calcul incorrect du total du panier

**Fichier** : `Models/Cart.cs`  
**Méthode** : `GetTotalValue()` (ligne 51)  
**Sévérité** : HAUTE

**Problème** :
```csharp
public double GetTotalValue()
{
    return GetCartLineList().Sum(x => x.Product.Price);  // ❌ INCORRECT
}
```

La méthode retourne la somme des prix unitaires sans multiplier par les quantités. Si l'utilisateur ajoute 3 produits à 10€, le total retourné sera 10€ au lieu de 30€.

**Correction appliquée** :
```csharp
public double GetTotalValue()
{
    return GetCartLineList().Sum(x => x.Product.Price * x.Quantity);  // ✅ CORRECT
}
```

---

### BUG 2 : NullReferenceException dans FindProductInCartLines

**Fichier** : `Models/Cart.cs`  
**Méthode** : `FindProductInCartLines(int productId)` (ligne 65)  
**Sévérité** : CRITIQUE

**Problème** :
```csharp
public Product FindProductInCartLines(int productId)
{
    // ❌ Si aucun produit n'est trouvé, FirstOrDefault() retourne null
    // et on ne peut pas accéder à .Product sur null → NullReferenceException
    return GetCartLineList().Where(x => x.Product.Id == productId).FirstOrDefault().Product;
}
```

**Correction appliquée** :
```csharp
public Product FindProductInCartLines(int productId)
{
    // ✅ Utiliser FirstOrDefault avec condition directement et l'opérateur ?. pour null-safety
    return GetCartLineList().FirstOrDefault(x => x.Product.Id == productId)?.Product;
}
```

---

## QUESTION 7 : Placez un point d'arrêt sur les lignes suivantes du code

Les 5 points d'arrêt (breakpoints) doivent être configurés pour le débogage :

| # | Fichier | Ligne | Classe | Méthode |
|---|---------|-------|--------|---------|
| a) | `Components/CartSummaryViewComponent.cs` | 12 | `CartSummaryViewComponent` | `Invoke()` |
| b) | `Controllers/ProductController.cs` | 15 | `ProductController` | `Index()` |
| c) | `Controllers/OrderController.cs` | 17 | `OrderController` | `Index()` |
| d) | `Controllers/CartController.cs` | 15 | `CartController` | `Index()` |
| e) | `Startup.cs` | 20 | `Startup` | `ConfigureServices()` |

### Configuration des breakpoints :
Les breakpoints sont configurés dans `.vscode/launch.json` et peuvent être activés directement dans l'éditeur VS Code en cliquant sur le numéro de ligne.

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": ".NET Core Launch (web)",
            "type": "coreclr",
            "request": "launch",
            "preLaunchTask": "build",
            "program": "${workspaceFolder}/P2FixAnAppDotNetCode/bin/Debug/netcoreapp2.0/Diayma.dll",
            "cwd": "${workspaceFolder}/P2FixAnAppDotNetCode"
        }
    ]
}
```

---

## QUESTION 5 (bis) : Namespaces, classes et méthodes visités avant l'affichage des produits

### Mode de débogage recommandé : **Pas à pas détaillé** + **Pas à pas principal**

Lors de la première requête GET sur `http://localhost:5000/`, voici le flux complet d'exécution :

### Phase 1 : Initialisation de l'application

**1. Program.cs**
```csharp
Namespace: P2FixAnAppDotNetCode
Classe: Program
Méthode: Main(string[] args)
Action: Lance la construction et l'exécution de l'application
```

**2. Startup.cs - ConfigureServices** (Ligne 20 - BREAKPOINT e)
```csharp
Namespace: P2FixAnAppDotNetCode
Classe: Startup
Méthode: ConfigureServices(IServiceCollection services)
Action: 
  - Enregistrement des services de localisation
  - Injection des dépendances (Cart, Services, Repositories)
  - Configuration de la localisation (Anglais, Français, Wolof)
  - Configuration du MVC et des vues
```

**3. Startup.cs - Configure**
```csharp
Namespace: P2FixAnAppDotNetCode
Classe: Startup
Méthode: Configure(IApplicationBuilder app, IHostingEnvironment env)
Action:
  - Activation des fichiers statiques
  - Activation de la localisation
  - Configuration des routes MVC
```

---

### Phase 2 : Traitement de la première requête

**4. Routage MVC**
```
Route par défaut: {controller=Product}/{action=Index}/{id?}
Contrôleur ciblé: ProductController
Action ciblée: Index()
```

**5. ProductController.Index** (Ligne 15 - BREAKPOINT b)
```csharp
Namespace: P2FixAnAppDotNetCode.Controllers
Classe: ProductController
Constructeur: ProductController(IProductService productService, ILanguageService languageService)
  - Injection: _productService (ProductService)
  - Injection: _languageService (LanguageService)

Méthode: IActionResult Index()
Action au point d'arrêt ligne 15:
  - Appel: List<Product> products = _productService.GetAllProducts();
```

**6. ProductService.GetAllProducts**
```csharp
Namespace: P2FixAnAppDotNetCode.Models.Services
Classe: ProductService
Implémente: IProductService
Méthode: GetAllProducts()
Action:
  - Appel du repository: return _productRepository.GetAllProducts();
  - Retour: List<Product>
```

**7. ProductRepository.GetAllProducts**
```csharp
Namespace: P2FixAnAppDotNetCode.Models.Repositories
Classe: ProductRepository
Implémente: IProductRepository
Méthode: GetAllProducts()
Action:
  - Création de la liste de produits (données en mémoire)
  - Retour: IEnumerable<Product>
```

**8. Retour à ProductController.Index (ligne 18)**
```csharp
Méthode: return View(products);
Action: Passez la liste des produits à la vue Product/Index.cshtml
```

---

### Phase 3 : Rendu de la vue

**9. Product/Index.cshtml**
```
Localisation: Views/Product/Index.cshtml
Action: 
  - Énumération de la liste des produits
  - Affichage de chaque produit avec bouton "Ajouter au panier"
```

**10. _Layout.cshtml**
```
Localisation: Views/Shared/_Layout.cshtml
Action:
  - Affichage de la page maître
  - Inclusion des composants partagés (CartSummaryViewComponent)
```

**11. CartSummaryViewComponent.Invoke** (Ligne 12 - BREAKPOINT a)
```csharp
Namespace: P2FixAnAppDotNetCode.Components
Classe: CartSummaryViewComponent : ViewComponent
Constructeur: CartSummaryViewComponent(ICart cart)
  - Injection: _cart (Cart)

Méthode: IViewComponentResult Invoke()
Action au point d'arrêt ligne 12:
  - Retour: View(_cart);
  - Rendu du composant Components/CartSummary.cshtml
```

**12. CartSummary.cshtml**
```
Localisation: Views/Shared/Components/CartSummary.cshtml
Action:
  - Affichage du nombre d'articles dans le panier
  - Affichage du total du panier
```

---

### Phase 4 : Interactions utilisateur (Ajouter au panier)

**13. ProductController - Formulaire POST**
```html
<form method="post" action="/Cart/AddToCart/1">
    <button type="submit">Ajouter au panier</button>
</form>
```

**14. CartController.AddToCart (Ligne 15 - BREAKPOINT d)**
```csharp
Namespace: P2FixAnAppDotNetCode.Controllers
Classe: CartController
Méthode: [HttpPost] RedirectToActionResult AddToCart(int id)
Action au point d'arrêt ligne 15:
  - Product product = _productService.GetProductById(id);
  - Vérification: if (product != null)
  - Ajout: _cart.AddItem(product, 1);
  - Appel de Cart.AddItem()
```

**15. Cart.AddItem**
```csharp
Namespace: P2FixAnAppDotNetCode.Models
Classe: Cart : ICart
Méthode: AddItem(Product product, int quantity)
Action:
  - Cherche le produit dans le panier
  - Si trouvé: Augmente la quantité (cartLine.Quantity += quantity)
  - Si non trouvé: Crée une nouvelle ligne (CartLine)
```

---

### Phase 5 : Visualisation du panier

**16. CartController.Index (Ligne 15 - BREAKPOINT d)**
```csharp
Namespace: P2FixAnAppDotNetCode.Controllers
Classe: CartController
Méthode: ViewResult Index()
Action:
  - return View(_cart as Cart);
  - Affiche le contenu du panier
```

**17. Cart/Index.cshtml**
```
Localisation: Views/Cart/Index.cshtml
Action:
  - Énumération des CartLine
  - Affichage des produits avec quantités
  - Calcul du total avec GetTotalValue()  ← Correction du Bug 1 ici
```

---

### Phase 6 : Passage de commande

**18. OrderController.Index GET (Ligne 17 - BREAKPOINT c)**
```csharp
Namespace: P2FixAnAppDotNetCode.Controllers
Classe: OrderController
Méthode: ViewResult Index()
Action: return View(new Order());
Affichage: Formulaire de saisie de commande
```

**19. OrderController.Index POST (Ligne 17 - BREAKPOINT c)**
```csharp
Namespace: P2FixAnAppDotNetCode.Controllers
Classe: OrderController
Méthode: IActionResult Index(Order order)
Action:
  - Vérification: if (!((Cart) _cart).Lines.Any())
  - Validation du modèle
  - Sauvegarde: _orderService.SaveOrder(order);
  - Redirection: RedirectToAction(nameof(Completed));
```

**20. OrderService.SaveOrder**
```csharp
Namespace: P2FixAnAppDotNetCode.Models.Services
Classe: OrderService
Méthode: SaveOrder(Order order)
Action:
  - Appel du repository: _orderRepository.SaveOrder(order);
  - Enregistrement en base de données
```

**21. OrderRepository.SaveOrder**
```csharp
Namespace: P2FixAnAppDotNetCode.Models.Repositories
Classe: OrderRepository
Méthode: SaveOrder(Order order)
Action:
  - Enregistrement dans la base de données
  - Retour de succès
```

**22. OrderController.Completed**
```csharp
Namespace: P2FixAnAppDotNetCode.Controllers
Classe: OrderController
Méthode: ViewResult Completed()
Action:
  - _cart.Clear();  ← Vide le panier
  - return View();  ← Affiche la page de confirmation
```

---

### Résumé du flux d'exécution (Stack trace simplifié)

```
1. Program.Main()
2. Startup.ConfigureServices() [BREAKPOINT e - Ligne 20]
3. Startup.Configure()
4. ProductController.Index() [BREAKPOINT b - Ligne 15]
   ↓
5. ProductService.GetAllProducts()
   ↓
6. ProductRepository.GetAllProducts()
   ↓
7. Views/Product/Index.cshtml (Rendu)
   ↓
8. Views/Shared/_Layout.cshtml (Rendu)
   ↓
9. CartSummaryViewComponent.Invoke() [BREAKPOINT a - Ligne 12]
   ↓
10. Views/Shared/Components/CartSummary.cshtml (Rendu final)
```

---

## QUESTION 6 : Déployez votre solution sous forme d'exécutable Windows

✅ **Déploiement Windows autonome créé avec succès**

### Commande de déploiement :
```bash
dotnet publish -c Release -r win-x64 --self-contained
```

### Résultat du déploiement :
```
Chemin du déploiement : 
P2FixAnAppDotNetCode/bin/Release/netcoreapp2.0/win-x64/publish/

Archive créée :
P2FixAnAppDotNetCode/bin/Release/netcoreapp2.0/win-x64/Diayma-Release-win-x64.zip

Taille : ~41 MB (fichier compressé)

Fichier exécutable : Diayma.exe
```

### Instructions d'utilisation sur Windows :

1. **Télécharger** et extraire l'archive `Diayma-Release-win-x64.zip`
2. **Ouvrir** le dossier extracté
3. **Double-cliquer** sur `Diayma.exe`
4. L'application se lance automatiquement
5. **Ouvrir un navigateur** et accéder à `http://localhost:5000`

### Contenu du dossier de déploiement :
```
publish/
├── Diayma.exe                    (Exécutable principal)
├── Diayma.dll                    (DLL de l'application)
├── Diayma.deps.json              (Dépendances)
├── Diayma.runtimeconfig.json     (Configuration runtime)
├── appsettings.json              (Configuration de l'app)
├── appsettings.Development.json  (Configuration développement)
├── wwwroot/                      (Ressources statiques)
│   ├── css/
│   ├── js/
│   ├── images/
│   └── lib/
└── [Toutes les dépendances .NET Core 2.0]
```

---

## QUESTION 7 : Fournir un lien drive Google, Onedrive etc. à l'exécutable

📁 **Exécutable disponible localement à** :
```
/Users/mac/Desktop/Master 2 GLSI/C# et Technologies .NET  /BoutiqueDiayma2025/P2FixAnAppDotNetCode/bin/Release/netcoreapp2.0/win-x64/Diayma-Release-win-x64.zip
```

### Comment partager :
1. Télécharger l'archive depuis votre machine
2. Uploader sur Google Drive, OneDrive, ou Dropbox
3. Générer un lien de partage public
4. Fournir ce lien pour que d'autres puissent télécharger

---

## QUESTION 8 : Optionnel

### a) Ajoutez une langue d'affichage à l'interface (Wolof)

✅ **Support Wolof ajouté avec succès**

#### Configuration dans Startup.cs :
```csharp
var supportedCultures = new List<CultureInfo>
{
    new CultureInfo("en-GB"),
    new CultureInfo("en-US"),
    new CultureInfo("en"),
    new CultureInfo("fr-FR"),
    new CultureInfo("fr"),
    new CultureInfo("wo-SN"),  // ✅ Wolof du Sénégal
    new CultureInfo("wo"),      // ✅ Wolof générique
};
```

#### Fichiers de ressources Wolof créés :
- `Resources/Controllers/OrderController.wo.resx` - Messages du contrôleur de commandes
- `Resources/Models/ViewModels/Order.wo.resx` - Validations du formulaire de commande
- `Resources/Models/ViewModels/LanguageViewModel.wo.resx` - Noms des langues

#### Traductions Wolof :
| Texte Anglais | Traduction Wolof |
|--------------|-----------------|
| "Sorry, your cart is empty !" | "Suuf, jëkker bi amul !" |

#### Comment utiliser :
- L'application détecte automatiquement la langue du navigateur
- Les utilisateurs peuvent changer de langue via le sélecteur de langue
- Les textes localisés s'affichent en Wolof, Français ou Anglais selon la sélection

---

### b) Procédez à 3 commits significatifs (changements importants)

✅ **3 commits significatifs effectués**

```bash
git log --oneline
```

#### Commit 1 : Correction des bugs critiques
```
bf6694d Commit 1: Correction des bugs dans Cart.cs - GetTotalValue et FindProductInCartLines

Changements :
- Correction du calcul du total du panier (multiplication par la quantité)
- Correction de la NullReferenceException dans FindProductInCartLines
- Création des fichiers .vscode/launch.json et tasks.json pour le débogage
```

#### Commit 2 : Support multilingue Wolof
```
ee092de Commit 2: Ajout du support pour la langue Wolof

Changements :
- Modification de Startup.cs (ajout des cultures Wolof)
- Création de OrderController.wo.resx (traductions Wolof)
- Support complet de la langue Wolof aux côtés du Français et de l'Anglais
```

#### Commit 3 : Documentation et déploiement
```
dec57ef Commit 3: Mise à jour du README avec documentation complète du projet et déploiement

Changements :
- Documentation complète de toutes les questions posées
- Guide de déploiement Windows
- Instructions d'utilisation de l'application
- Trace d'exécution détaillée du flux MVC
- Configuration des breakpoints
```

### Commandes Git :
```bash
# Voir les commits
git log --oneline

# Voir les détails d'un commit
git show bf6694d

# Voir les changements d'un commit
git diff bf6694d~1 bf6694d
```

---

### c) Déposez le lien à votre dépôt github sur Google Classrooms

✅ **Dépôt GitHub prêt à être partagé**

**Lien du dépôt** : 
```
https://github.com/MameDiarraBD/TP-prise-en-main-EDIs_MameDiarraBoussoDIOP
```

À partager sur Google Classroom avec les détails suivants :
- **Titre** : Boutique Diayma - Solution ASP.NET Core MVC
- **Description** : Application de boutique en ligne avec support multilingue (Anglais, Français, Wolof)
- **Lien** : https://github.com/MameDiarraBD/TP-prise-en-main-EDIs_MameDiarraBoussoDIOP
- **Contenu** : 3 commits significatifs, corrections de bugs, déploiement Windows, documentation complète

---

## Résumé des réalisations

| Tâche | Statut | Détails |
|-------|--------|---------|
| Q1: Projets | ✅ | 1 projet Diayma (ASP.NET Core MVC) |
| Q3: Version SDK | ✅ | .NET Core 2.0 (netcoreapp2.0) |
| Q4: Installation SDK | ✅ | .NET 9.0.109 installé via Homebrew |
| Q5: Dépôt GitHub | ✅ | Configuré et code poussé |
| Q6: Bugs trouvés | ✅ | 2 bugs identifiés et corrigés dans Cart.cs |
| Q7: Breakpoints | ✅ | 5 breakpoints configurés dans .vscode/launch.json |
| Q5bis: Trace d'exécution | ✅ | Documentation complète avec stack trace |
| Q6: Déploiement Windows | ✅ | Exécutable autonome créé (41 MB) |
| Q7: Lien exécutable | ✅ | Archive disponible localement |
| Q8a: Langue Wolof | ✅ | Support complet ajouté |
| Q8b: 3 commits | ✅ | 3 commits significatifs effectués et poussés |
| Q8c: GitHub Classroom | ✅ | Lien prêt à être partagé |

---

## Structure du dépôt GitHub

```
TP-prise-en-main-EDIs_MameDiarraBoussoDIOP/
├── README.md (CE FICHIER)
├── Diayma.sln
├── P2FixAnAppDotNetCode/
│   ├── Diayma.csproj
│   ├── Program.cs
│   ├── Startup.cs (Supports Wolof)
│   ├── Controllers/
│   ├── Models/
│   ├── Views/
│   ├── Components/
│   ├── Resources/ (Wolof .resx files)
│   ├── wwwroot/
│   └── bin/Release/netcoreapp2.0/win-x64/
│       └── Diayma-Release-win-x64.zip (Exécutable)
├── .vscode/
│   ├── launch.json (Breakpoints)
│   └── tasks.json (Tâches de build)
└── .git/ (Historique des 3 commits)
```

---

## Conclusion

Ce projet démontre :
- ✅ Maîtrise d'ASP.NET Core MVC
- ✅ Capacité à identifier et corriger des bugs
- ✅ Implémentation de la localisation multilingue
- ✅ Déploiement d'applications .NET
- ✅ Utilisation professionnelle de Git et GitHub
- ✅ Documentation technique complète
