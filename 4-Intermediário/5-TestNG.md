# 4.5 - TestNG

O **TestNG** é um framework de testes para Java inspirado no JUnit, mas com funcionalidades muito mais ricas para automação de testes. É amplamente adotado em projetos Selenium por suportar paralelismo nativo, agrupamento de testes, providers de dados e configuração declarativa via XML — sem depender do Maven Surefire para isso.

## TestNG vs JUnit 4

| Funcionalidade | JUnit 4 | TestNG |
|----------------|---------|--------|
| Anotações de ciclo de vida | `@Before`, `@After` | `@BeforeMethod`, `@AfterMethod`, `@BeforeClass`, `@AfterClass`, `@BeforeSuite`, `@AfterSuite` |
| Testes parametrizados | Limitado (com Parameterized runner) | Nativo com `@DataProvider` |
| Paralelismo | Via Maven Surefire | Nativo via `testng.xml` |
| Agrupamento de testes | Não | Nativo com `groups` |
| Dependência entre testes | Não | `dependsOnMethods` |
| Relatórios | Básico | HTML detalhado por padrão |

## Adicionando TestNG ao projeto

No `pom.xml`, substitua a dependência do JUnit pelo TestNG:

```xml
<dependency>
    <groupId>org.testng</groupId>
    <artifactId>testng</artifactId>
    <version>7.9.0</version>
    <scope>test</scope>
</dependency>
```

## Anotações principais

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.AfterSuite;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.BeforeSuite;
import org.testng.annotations.Test;

public class ExemploTestNG {

    private WebDriver driver;

    @BeforeSuite
    public void configurarSuite() {
        // Executado uma vez antes de todos os testes da suíte
        System.out.println("Iniciando suíte de testes");
    }

    @BeforeMethod
    public void setUp() {
        // Executado antes de cada método @Test
        driver = new ChromeDriver();
        driver.manage().window().maximize();
    }

    @Test
    public void testAbrirPaginaInicial() {
        driver.get("https://exemplo.com");
        System.out.println("Título: " + driver.getTitle());
    }

    @AfterMethod
    public void tearDown() {
        // Executado após cada método @Test
        if (driver != null) {
            driver.quit();
        }
    }

    @AfterSuite
    public void finalizarSuite() {
        // Executado uma vez após todos os testes da suíte
        System.out.println("Suíte finalizada");
    }
}
```

## Asserções com TestNG

O TestNG tem sua própria classe de asserções:

```java
import org.testng.Assert;

// Verifica igualdade
Assert.assertEquals(driver.getTitle(), "Página Inicial");

// Verifica que a condição é verdadeira
Assert.assertTrue(driver.getCurrentUrl().contains("exemplo.com"));

// Verifica que o elemento não é nulo
Assert.assertNotNull(driver.findElement(By.id("botao")));
```

## Data Provider — testes parametrizados

O `@DataProvider` permite rodar o mesmo teste com diferentes conjuntos de dados:

```java
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class LoginTest {

    @DataProvider(name = "credenciais")
    public Object[][] fornecerCredenciais() {
        return new Object[][] {
            { "usuario1", "senha123" },
            { "usuario2", "outraSenha" },
            { "admin",    "admin123"  }
        };
    }

    @Test(dataProvider = "credenciais")
    public void testLogin(String usuario, String senha) {
        driver.get("https://exemplo.com/login");
        driver.findElement(By.id("username")).sendKeys(usuario);
        driver.findElement(By.id("password")).sendKeys(senha);
        driver.findElement(By.id("loginBtn")).click();

        // Valida que o login foi realizado
        Assert.assertTrue(driver.getCurrentUrl().contains("dashboard"),
            "Login falhou para o usuário: " + usuario);
    }
}
```

## Agrupamento de testes

Você pode marcar testes com grupos e decidir quais executar:

```java
@Test(groups = { "smoke" })
public void testAbrirSite() { ... }

@Test(groups = { "regressao" })
public void testFluxoCompleto() { ... }

@Test(groups = { "smoke", "regressao" })
public void testLogin() { ... }
```

## testng.xml — configurando a suíte

O arquivo `testng.xml` (na raiz do projeto) controla quais testes rodam, em que ordem e com qual paralelismo:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE suite SYSTEM "https://testng.org/testng-1.0.dtd">
<suite name="Suíte Selenium" verbose="1">

    <!-- Paralelismo a nível de classes: cada classe roda em uma thread própria -->
    <suite-files/>

    <test name="Testes de Smoke" parallel="methods" thread-count="3">
        <groups>
            <run>
                <include name="smoke"/>
            </run>
        </groups>
        <classes>
            <class name="LoginTest"/>
            <class name="ExemploTestNG"/>
        </classes>
    </test>

</suite>
```

Para rodar via Maven usando o `testng.xml`:

```xml
<!-- No pom.xml, dentro do plugin maven-surefire-plugin -->
<configuration>
    <suiteXmlFiles>
        <suiteXmlFile>testng.xml</suiteXmlFile>
    </suiteXmlFiles>
</configuration>
```

Ou diretamente pelo terminal:

```bash
mvn test -DsuiteXmlFile=testng.xml
```

## Relatórios

O TestNG gera automaticamente relatórios HTML em `target/surefire-reports/index.html` após cada execução. Para relatórios mais ricos, considere o [Allure](https://allurereport.org) — que integra nativamente com TestNG.

Ir para: [5.1 Selenium Grid](../5-Avancado/1-Selenium-grid.md)
