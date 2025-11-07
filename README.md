# Kata DevBox - F# / .NET

Environnement pour katas en F# avec .NET et NUnit.

---

## 📦 Initialiser DevBox (première fois uniquement)

```bash
# Initialiser devbox
devbox init

# Ajouter .NET SDK
devbox add dotnet-sdk_8

# Entrer dans l'environnement
devbox shell

# Vérifier l'installation
dotnet --version
```

---

## 🚀 Setup du Projet

```bash
# 0. Définir le nom du projet
PROJECT_NAME="Kata"  # Changer ici pour votre kata (ex: "FizzBuzz", "StringCalculator")

# 1. Lancer DevBox (si pas déjà dans le shell)
devbox shell

# 2. Créer le projet F#
dotnet new sln -n $PROJECT_NAME
dotnet new classlib -lang F# -n $PROJECT_NAME.Core
dotnet new nunit -lang F# -n $PROJECT_NAME.Tests
dotnet sln add $PROJECT_NAME.Core $PROJECT_NAME.Tests
cd $PROJECT_NAME.Tests && dotnet add reference ../$PROJECT_NAME.Core && cd ..

# 3. Ajouter les packages de test
cd $PROJECT_NAME.Tests
dotnet add package FsUnit
dotnet add package Unquote
dotnet add package coverlet.collector
cd ..

# 4. Lancer les tests
dotnet test

# 5. Mode watch (auto-reload)
dotnet watch test --project $PROJECT_NAME.Tests
```

**Structure créée :**
```
$PROJECT_NAME/
├── $PROJECT_NAME.Core/      → Code de production
└── $PROJECT_NAME.Tests/     → Tests
```

---

## ✏️ Exemple

**Kata.Tests/CalculatorTests.fs**
```fsharp
module Kata.Tests.CalculatorTests

open NUnit.Framework
open FsUnit
open Kata.Core

[<Test>]
let ``Add two numbers returns sum`` () =
    let result = Calculator.add 2 3
    result |> should equal 5
```

**Kata.Core/Calculator.fs**
```fsharp
module Kata.Core.Calculator

let add a b = a + b
```

---

## 🧪 Guide des Outils

### 1️⃣ NUnit + FsUnit - Framework de test

```fsharp
open NUnit.Framework
open FsUnit

[<Test>]
let ``simple test`` () =
    1 + 1 |> should equal 2

[<TestCase(2, 3, 5)>]
[<TestCase(0, 0, 0)>]
let ``parameterized test`` a b expected =
    a + b |> should equal expected

[<SetUp>]
let setup () =
    // Avant chaque test
    ()

[<Category("Fast")>]
let ``categorized test`` () =
    true |> should be True
```

### 2️⃣ FsUnit - Assertions idiomatiques F#

```fsharp
// Valeurs
result |> should equal 5
result |> should not' (equal 0)
result |> should be (greaterThan 0)
result |> should be (lessThan 10)

// Chaînes
name |> should equal "Alice"
name |> should contain "lic"
name |> should startWith "Al"
name |> should endWith "ce"

// Collections
list |> should haveLength 3
list |> should contain 42
list |> should be Empty

// Exceptions
(fun () -> failwith "boom") |> should throw typeof<System.Exception>

// Booléens
result |> should be True
result |> should be False

// Null
value |> should be Null
value |> should not' (be Null)
```

### 3️⃣ Unquote - Assertions quotées

```fsharp
open Swensen.Unquote

[<Test>]
let ``unquote test`` () =
    test <@ 2 + 2 = 4 @>
    test <@ "hello".Length = 5 @>

// Affiche les valeurs intermédiaires en cas d'échec
[<Test>]
let ``detailed failure`` () =
    let x = 5
    let y = 3
    test <@ x + y = 10 @>  // Montre: x = 5, y = 3, x + y = 8
```

### 4️⃣ Moq - Mocks (utilisable depuis F#)

```fsharp
open Moq

[<Test>]
let ``mock example`` () =
    // Créer un mock
    let mock = Mock<IUserRepository>()

    // Setup
    mock.Setup(fun m -> m.GetById(1))
        .Returns({ Id = 1; Name = "Alice" })
        |> ignore

    // Utiliser
    let user = mock.Object.GetById(1)
    user.Name |> should equal "Alice"

    // Verify
    mock.Verify((fun m -> m.GetById(1)), Times.Once())
```

### 5️⃣ SpecFlow - Tests BDD (optionnel)

**Installation :**
```bash
dotnet add package SpecFlow.NUnit
dotnet add package SpecFlow.Tools.MsBuild.Generation
```

**Features/Calculator.feature**
```gherkin
# language: fr
Fonctionnalité: Calculatrice

Scénario: Addition de deux nombres
  Étant donné les nombres 2 et 3
  Quand je les additionne
  Alors le résultat est 5
```

**StepDefinitions/CalculatorSteps.fs**
```fsharp
module Kata.Tests.CalculatorSteps

open TechTalk.SpecFlow
open FsUnit
open Kata.Core

[<Binding>]
type CalculatorSteps() =
    let mutable result = 0

    [<Given(@"les nombres (.*) et (.*)")>]
    member _.GivenNombres(a: int, b: int) =
        result <- a + b

    [<When(@"je les additionne")>]
    member _.WhenAddition() =
        ()  // Déjà fait dans Given

    [<Then(@"le résultat est (.*)")>]
    member _.ThenResultat(expected: int) =
        result |> should equal expected
```

### 6️⃣ ApprovalTests - Tests de snapshot (optionnel)

**Installation :**
```bash
dotnet add package ApprovalTests
```

**Utilisation :**
```fsharp
open ApprovalTests

[<Test>]
let ``generate report produces correct output`` () =
    let report = generateReport()
    Approvals.Verify(report)
```

Au premier run, crée un fichier `.received.txt`. Si OK, renommer en `.approved.txt`.
Les runs suivants comparent avec `.approved.txt`.

### 7️⃣ Coverlet - Couverture de code

```bash
# Couverture simple
dotnet test /p:CollectCoverage=true

# Rapport HTML
dotnet test /p:CollectCoverage=true /p:CoverletOutputFormat=opencover
dotnet tool install -g dotnet-reportgenerator-globaltool
reportgenerator -reports:coverage.opencover.xml -targetdir:coverage-report
open coverage-report/index.html
```

---

## 🛠️ Commandes Essentielles

```bash
# Tests
dotnet test                                          # Lancer les tests
dotnet watch test --project $PROJECT_NAME.Tests      # Mode watch
dotnet test --filter "Category=Fast"                 # Filtrer par catégorie
dotnet test --logger "console;verbosity=detailed"    # Sortie détaillée

# Build
dotnet build                                         # Compiler
dotnet clean                                         # Nettoyer

# DevBox
devbox shell                                         # Entrer dans l'env
exit                                                 # Sortir
devbox info                                          # Voir les packages
```

---

**Bon kata ! 🥋**