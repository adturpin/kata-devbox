# Kata DevBox - Java

Environnement pour katas en Java avec Maven et JUnit 5.

---

## 📦 Initialiser DevBox (première fois uniquement)

```bash
# Initialiser devbox
devbox init

# Ajouter Java et Maven
devbox add jdk maven

# Entrer dans l'environnement
devbox shell

# Vérifier l'installation
java -version
mvn -version
```

---

## 🚀 Setup du Projet

```bash
# 1. Lancer DevBox (si pas déjà dans le shell)
devbox shell

# 2. Créer le projet Maven
mvn archetype:generate \
  -DgroupId=com.kata \
  -DartifactId=kata \
  -DarchetypeArtifactId=maven-archetype-quickstart \
  -DarchetypeVersion=1.4 \
  -DinteractiveMode=false

cd kata

# 3. Remplacer le pom.xml avec les dépendances nécessaires
cat > pom.xml << 'EOF'
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.kata</groupId>
    <artifactId>kata</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <junit.version>5.10.1</junit.version>
    </properties>

    <dependencies>
        <!-- JUnit 5 -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>

        <!-- AssertJ (assertions fluides) -->
        <dependency>
            <groupId>org.assertj</groupId>
            <artifactId>assertj-core</artifactId>
            <version>3.24.2</version>
            <scope>test</scope>
        </dependency>

        <!-- Mockito (mocks) -->
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>5.8.0</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-junit-jupiter</artifactId>
            <version>5.8.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.2.3</version>
            </plugin>
        </plugins>
    </build>
</project>
EOF

# 4. Lancer les tests
mvn test

# 5. Mode watch (auto-reload avec maven wrapper)
mvn compile test -Dtest.watch=true
```

**Structure créée :**
```
kata/
├── src/
│   ├── main/java/com/kata/     → Code de production
│   └── test/java/com/kata/     → Tests
└── pom.xml                     → Configuration Maven
```

---

## ✏️ Exemple

**src/test/java/com/kata/CalculatorTest.java**
```java
package com.kata;

import org.junit.jupiter.api.Test;
import static org.assertj.core.api.Assertions.assertThat;

class CalculatorTest {

    @Test
    void add_twoNumbers_returnsSum() {
        Calculator calculator = new Calculator();
        int result = calculator.add(2, 3);
        assertThat(result).isEqualTo(5);
    }
}
```

**src/main/java/com/kata/Calculator.java**
```java
package com.kata;

public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

---

## 🧪 Guide des Outils

### 1️⃣ JUnit 5 - Framework de test

```java
@Test
void simpleTest() { }

@ParameterizedTest
@CsvSource({"2, 3, 5", "0, 0, 0"})
void parameterizedTest(int a, int b, int expected) { }

@BeforeEach
void beforeEachTest() { }

@Tag("Fast")
@Test
void categorizedTest() { }

@DisplayName("Addition de deux nombres")
@Test
void testWithDisplayName() { }
```

### 2️⃣ AssertJ - Assertions lisibles

```java
// Valeurs
assertThat(result).isEqualTo(5);
assertThat(result).isGreaterThan(0);

// Chaînes
assertThat(name).isEqualTo("Alice");
assertThat(name).contains("lic");
assertThat(name).startsWith("Al");

// Collections
assertThat(list).hasSize(3);
assertThat(list).anyMatch(x -> x.getId() == 1);
assertThat(list).isEmpty();

// Exceptions
assertThatThrownBy(() -> { throw new Exception(); })
    .isInstanceOf(Exception.class);

// Objets
assertThat(user)
    .hasFieldOrPropertyWithValue("name", "Alice")
    .hasFieldOrPropertyWithValue("age", 30);
```

### 3️⃣ Mockito - Mocks et stubs

```java
// Créer un mock
UserRepository mock = mock(UserRepository.class);

// Setup : configurer le comportement
when(mock.getById(1))
    .thenReturn(new User(1, "Alice"));

// Utiliser
UserService service = new UserService(mock);
User user = service.getUser(1);

// Verify : vérifier les appels
verify(mock, times(1)).getById(1);
```

**Matchers utiles :**
```java
any()                              // N'importe quelle valeur
anyInt()                           // N'importe quel int
argThat(x -> x > 0)                // Condition personnalisée
eq(5)                              // Égalité exacte
```

**Avec annotations :**
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    @Mock
    UserRepository repository;

    @InjectMocks
    UserService service;

    @Test
    void test() {
        when(repository.getById(1)).thenReturn(new User(1, "Alice"));
        // test...
    }
}
```

### 4️⃣ Cucumber - Tests BDD (optionnel)

**Installation (ajouter au pom.xml) :**
```xml
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-java</artifactId>
    <version>7.15.0</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>io.cucumber</groupId>
    <artifactId>cucumber-junit-platform-engine</artifactId>
    <version>7.15.0</version>
    <scope>test</scope>
</dependency>
```

**src/test/resources/features/calculator.feature**
```gherkin
# language: fr
Fonctionnalité: Calculatrice

Scénario: Addition de deux nombres
  Étant donné les nombres 2 et 3
  Quand je les additionne
  Alors le résultat est 5
```

**src/test/java/com/kata/steps/CalculatorSteps.java**
```java
package com.kata.steps;

import io.cucumber.java.fr.*;
import static org.assertj.core.api.Assertions.assertThat;

public class CalculatorSteps {
    private int result;

    @Etantdonné("les nombres {int} et {int}")
    public void lesNombres(int a, int b) {
        // Setup
    }

    @Quand("je les additionne")
    public void jeAdditionne() {
        result = // calcul
    }

    @Alors("le résultat est {int}")
    public void leResultatEst(int expected) {
        assertThat(result).isEqualTo(expected);
    }
}
```

### 5️⃣ ApprovalTests - Tests de snapshot (optionnel)

**Installation (ajouter au pom.xml) :**
```xml
<dependency>
    <groupId>com.approvaltests</groupId>
    <artifactId>approvaltests</artifactId>
    <version>22.3.3</version>
    <scope>test</scope>
</dependency>
```

**Utilisation :**
```java
@Test
void generateReport_producesCorrectOutput() {
    String report = generateReport();
    Approvals.verify(report);
}
```

Au premier run, crée un fichier `.received.txt`. Si OK, renommer en `.approved.txt`.
Les runs suivants comparent avec `.approved.txt`.

### 6️⃣ JaCoCo - Couverture de code

**Configuration (ajouter au pom.xml) :**
```xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.11</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

**Utilisation :**
```bash
# Lancer les tests avec couverture
mvn clean test

# Voir le rapport HTML
open target/site/jacoco/index.html
```

---

## 🛠️ Commandes Essentielles

```bash
# Tests
mvn test                                             # Lancer les tests
mvn test -Dtest=CalculatorTest                       # Lancer un test spécifique
mvn test -Dgroups=Fast                               # Filtrer par tag
mvn test -X                                          # Sortie détaillée (debug)

# Build
mvn compile                                          # Compiler
mvn clean                                            # Nettoyer
mvn package                                          # Créer le JAR

# Autres
mvn dependency:tree                                  # Voir les dépendances
mvn help:effective-pom                               # Voir le POM effectif

# DevBox
devbox shell                                         # Entrer dans l'env
exit                                                 # Sortir
devbox info                                          # Voir les packages
```

---

**Bon kata ! 🥋**