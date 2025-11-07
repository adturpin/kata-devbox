# Kata DevBox - C# / .NET

Environnement pour katas en C# avec .NET et NUnit.

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

# 2. Créer le projet
dotnet new sln -n $PROJECT_NAME
dotnet new classlib -n $PROJECT_NAME.Core
dotnet new nunit -n $PROJECT_NAME.Tests
dotnet sln add $PROJECT_NAME.Core $PROJECT_NAME.Tests
cd $PROJECT_NAME.Tests && dotnet add reference ../$PROJECT_NAME.Core && cd ..

# 3. Ajouter les packages de test
cd $PROJECT_NAME.Tests
dotnet add package Moq
dotnet add package FluentAssertions
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

**Kata.Tests/CalculatorTests.cs**
```csharp
using NUnit.Framework;
using FluentAssertions;
using Kata.Core;

namespace Kata.Tests;

[TestFixture]
public class CalculatorTests
{
    [Test]
    public void Add_TwoNumbers_ReturnsSum()
    {
        var calculator = new Calculator();
        var result = calculator.Add(2, 3);
        result.Should().Be(5);
    }
}
```

**Kata.Core/Calculator.cs**
```csharp
namespace Kata.Core;

public class Calculator
{
    public int Add(int a, int b) => a + b;
}
```

---

## 🧪 Guide des Outils

### 1️⃣ NUnit - Framework de test

```csharp
[Test]
public void SimpleTest() { }

[TestCase(2, 3, 5)]
[TestCase(0, 0, 0)]
public void ParameterizedTest(int a, int b, int expected) { }

[SetUp]
public void BeforeEachTest() { }

[Category("Fast")]
public void CategorizedTest() { }
```

### 2️⃣ FluentAssertions - Assertions lisibles

```csharp
// Valeurs
result.Should().Be(5);
result.Should().BeGreaterThan(0);

// Chaînes
name.Should().Be("Alice");
name.Should().Contain("lic");
name.Should().StartWith("Al");

// Collections
list.Should().HaveCount(3);
list.Should().Contain(x => x.Id == 1);
list.Should().BeEmpty();

// Exceptions
Action act = () => throw new Exception();
act.Should().Throw<Exception>();

// Objets
user.Should().BeEquivalentTo(new { Name = "Alice", Age = 30 });
```

### 3️⃣ Moq - Mocks et stubs

```csharp
// Créer un mock
var mock = new Mock<IUserRepository>();

// Setup : configurer le comportement
mock.Setup(r => r.GetById(1))
    .Returns(new User { Id = 1, Name = "Alice" });

// Utiliser
var service = new UserService(mock.Object);
var user = service.GetUser(1);

// Verify : vérifier les appels
mock.Verify(r => r.GetById(1), Times.Once());
```

**Matchers utiles :**
```csharp
It.IsAny<int>()                    // N'importe quelle valeur
It.Is<int>(x => x > 0)             // Condition
It.IsInRange(1, 100, Range.Inclusive)
```

### 4️⃣ SpecFlow - Tests BDD (optionnel)

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

**StepDefinitions/CalculatorSteps.cs**
```csharp
[Binding]
public class CalculatorSteps
{
    private int _result;

    [Given(@"les nombres (.*) et (.*)")]
    public void GivenNombres(int a, int b)
    {
        // Setup
    }

    [When(@"je les additionne")]
    public void WhenAddition()
    {
        _result = // calcul
    }

    [Then(@"le résultat est (.*)")]
    public void ThenResultat(int expected)
    {
        _result.Should().Be(expected);
    }
}
```

### 5️⃣ ApprovalTests - Tests de snapshot (optionnel)

**Installation :**
```bash
dotnet add package ApprovalTests
```

**Utilisation :**
```csharp
[Test]
public void GenerateReport_ProducesCorrectOutput()
{
    var report = GenerateReport();
    Approvals.Verify(report);
}
```

Au premier run, crée un fichier `.received.txt`. Si OK, renommer en `.approved.txt`.
Les runs suivants comparent avec `.approved.txt`.

### 6️⃣ Coverlet - Couverture de code

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