# Guide de Déploiement - Boutique Diayma

**Date** : 26 novembre 2025
**Application** : Boutique Diayma (ASP.NET Core MVC)
**Dépôt GitHub** : https://github.com/MameDiarraBD/TP-prise-en-main-EDIs_MameDiarraBoussoDIOP

---

## 📦 Fichier Exécutable Disponible

### Localisation
```
P2FixAnAppDotNetCode/bin/Release/netcoreapp2.0/win-x64/Diayma-Release-win-x64.zip
```

### Caractéristiques
- **Type** : Archive ZIP (compressée)
- **Taille** : 41 MB
- **Plateforme** : Windows x64 (64-bit)
- **Format** : Self-contained (autonome - pas besoin d'installer .NET)

---

## 🚀 Installation et Utilisation

### Étape 1 : Télécharger l'archive
Télécharger le fichier `Diayma-Release-win-x64.zip` depuis votre machine locale.

### Étape 2 : Extraire l'archive
1. Clic droit sur l'archive
2. Sélectionner "Extraire tout..." (ou similaire selon votre système)
3. Choisir une destination (ex: `C:\Applications\Diayma`)
4. Cliquer sur "Extraire"

### Étape 3 : Lancer l'application
1. Ouvrir le dossier extrait
2. Double-cliquer sur **`Diayma.exe`**
3. L'application se lance automatiquement
4. Une fenêtre de terminal s'affiche (peut être ignorée)

### Étape 4 : Accéder à l'application
1. Ouvrir un navigateur web (Chrome, Firefox, Edge, etc.)
2. Accéder à : **http://localhost:5000**
3. L'interface de la boutique s'affiche

---

## 🌐 Fonctionnalités

### Boutique
- Affichage des produits disponibles
- Ajout de produits au panier
- Retrait de produits du panier
- Calcul automatique du total (bug corrigé ✅)

### Localisation
L'application supporte **3 langues** :
- 🇬🇧 Anglais (English)
- 🇫🇷 Français
- 🇸🇳 Wolof (Nouveau - support complet)

Utilisez le sélecteur de langue en haut de la page pour changer la langue.

### Commande
- Visualiser le panier
- Passer une commande
- Voir la page de confirmation

---

## 🔧 Arrêter l'application

### Méthode 1 (Recommandée)
1. Appuyer sur `Ctrl + C` dans la fenêtre de terminal
2. L'application s'arrête gracieusement

### Méthode 2
Fermer simplement la fenêtre de terminal

---

## 📋 Contenu de l'archive

```
Diayma-Release-win-x64/
├── Diayma.exe                      (Exécutable principal)
├── Diayma.dll                      (DLL de l'application)
├── Diayma.deps.json                (Dépendances)
├── Diayma.runtimeconfig.json       (Configuration runtime)
├── Diayma.pdb                      (Debug symbols)
├── appsettings.json                (Configuration générale)
├── appsettings.Development.json    (Configuration développement)
├── wwwroot/                        (Ressources statiques)
│   ├── css/                        (Feuilles de style)
│   │   ├── site.css
│   │   └── site.min.css
│   ├── js/                         (JavaScript)
│   │   ├── site.js
│   │   └── site.min.js
│   ├── images/                     (Images)
│   ├── lib/                        (Bibliothèques tierces)
│   │   ├── bootstrap/
│   │   ├── jquery/
│   │   └── fontawesome/
├── Resources/                      (Fichiers de localisation)
│   ├── Controllers/
│   │   ├── OrderController.en.resx
│   │   ├── OrderController.fr.resx
│   │   └── OrderController.wo.resx
│   ├── Models/
│   │   └── ViewModels/
│       └── Order.resx files
└── [Dépendances .NET Core 2.0 (assemblies)]
```

---

## ⚙️ Configuration

### Port par défaut
- **URL** : `http://localhost:5000`
- **Port** : 5000

### Fichiers de configuration
- `appsettings.json` - Configuration générale
- `appsettings.Development.json` - Configuration développement (debugging)

### Modification du port
Pour changer le port, modifier `Program.cs` et recompiler :
```csharp
.UseUrls("http://localhost:5001")  // Utiliser le port 5001
```

---

## 🐛 Bugs Corrigés

✅ **Bug 1** : Calcul du total du panier
- Le total était calculé sans multiplier par les quantités
- **Correction** : Multiplication par la quantité appliquée

✅ **Bug 2** : NullReferenceException dans FindProductInCartLines
- Risque d'exception si le produit n'existe pas
- **Correction** : Utilisation de l'opérateur `?.` pour null-safety

---

## 📱 Navigateurs supportés

- ✅ Chrome/Chromium
- ✅ Firefox
- ✅ Microsoft Edge
- ✅ Safari
- ✅ Internet Explorer 11+

---

## ⚠️ Prérequis

### Windows
- Windows 7 SP1 ou supérieur
- Architecture 64-bit (x64)
- **Pas besoin d'installer .NET** (inclus dans l'archive)

### Disque dur
- ~150 MB pour l'extraction
- ~50 MB d'espace libre supplémentaire

### Réseau
- Port 5000 disponible (configurable)
- Connexion localhost (pas de connexion internet requise)

---

## 🆘 Dépannage

### L'application ne démarre pas
1. Vérifier que le port 5000 n'est pas utilisé
2. Exécuter en tant qu'administrateur
3. Vérifier la présence du fichier `Diayma.exe`

### Erreur "Port déjà utilisé"
1. Arrêter l'application existante sur le port 5000
2. Ou modifier le port dans la configuration

### Problème d'accès sur le navigateur
1. Vérifier que l'URL est correcte : `http://localhost:5000`
2. Attendre quelques secondes pour que l'application démarre
3. Actualiser la page (F5)

### L'application se ferme immédiatement
Généralement un problème de configuration :
1. Vérifier le fichier `appsettings.json`
2. Vérifier les droits d'accès sur le dossier

---

## 📊 Informations Techniques

- **Framework** : ASP.NET Core 2.0
- **Architecture** : MVC (Model-View-Controller)
- **Injection de dépendances** : Intégrée
- **Localisation** : Microsoft.Extensions.Localization
- **Stockage** : En mémoire (données de démonstration)

---

## 📞 Support

Pour plus d'informations ou signaler des problèmes :
- **Dépôt GitHub** : https://github.com/MameDiarraBD/TP-prise-en-main-EDIs_MameDiarraBoussoDIOP
- **README** : Consultez le fichier README.md dans le dépôt

---

## ✅ Checklist Avant Utilisation

- [ ] Archive téléchargée
- [ ] Archive extraite
- [ ] Dossier contient `Diayma.exe`
- [ ] Port 5000 disponible
- [ ] Navigateur prêt à l'emploi
- [ ] Fenêtre de terminal visible (bon signe !)

**Vous êtes prêt à utiliser Boutique Diayma ! 🎉**
