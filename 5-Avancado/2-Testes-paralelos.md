# 5.2 - Testes Paralelos

Testes paralelos permitem executar múltiplos testes ao mesmo tempo, reduzindo significativamente o tempo total de execução da suíte. Com o padrão **Driver Factory** que vimos no módulo intermediário, a paralelização fica segura porque cada thread tem sua própria instância do WebDriver.

## Pré-requisito: ThreadLocal no Driver Factory

O `ThreadLocal<WebDriver>` garante que cada thread de teste receba um driver isolado, sem conflito com outras threads. Já vimos isso em [4.2 Driver Factory](../4-Intermediário/2-Driver-factory.md).

## Configurando paralelismo com JUnit + Maven Surefire

O plugin **Maven Surefire** controla como os testes são executados. Para habilitar paralelismo, configure o `pom.xml`:

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.2.5</version>
    <configuration>
        <!-- Executa classes de teste em paralelo -->
        <parallel>classes</parallel>
        <!-- Número de threads simultâneas -->
        <threadCount>4</threadCount>
        <forkCount>1</forkCount>
    </configuration>
</plugin>
```

Opções para `<parallel>`:
- `methods` — paraleliza métodos dentro de uma mesma classe
- `classes` — paraleliza classes de teste inteiras (recomendado)
- `both` — paraleliza classes e métodos simultaneamente

## Exemplo de testes paralelos

```java
import org.junit.After;
import org.junit.Before;
import org.junit.Test;
import org.openqa.selenium.WebDriver;

public class ParallelTest {

    private WebDriver driver;

    @Before
    public void setUp() {
        // Cada thread cria seu próprio driver via DriverFactory
        driver = DriverFactory.getDriver();
        driver.manage().window().maximize();
    }

    @Test
    public void testPaginaInicial() {
        driver.get("https://exemplo.com");
        System.out.println("[Thread " + Thread.currentThread().getId() + "] Título: " + driver.getTitle());
    }

    @Test
    public void testPaginaLogin() {
        driver.get("https://exemplo.com/login");
        System.out.println("[Thread " + Thread.currentThread().getId() + "] Título: " + driver.getTitle());
    }

    @After
    public void tearDown() {
        // IMPORTANTE: sempre chame quitDriver() para liberar o ThreadLocal
        DriverFactory.quitDriver();
    }
}
```

## Cuidados importantes

- **Sempre chame `DriverFactory.quitDriver()`** no `@After`. Sem isso, o `ThreadLocal` vaza memória entre testes.
- **Dados de teste não podem ser compartilhados** entre threads — cada teste deve ser independente.
- **Verifique o número de drivers** — rodar muitas threads ao mesmo tempo pode sobrecarregar a máquina. Ajuste `threadCount` conforme os recursos disponíveis.
- Para escalar além de uma máquina, combine testes paralelos com o **Selenium Grid** (veja o capítulo anterior).

Ir para: [5.3 Integração com CI/CD](3-CI-CD.md)
