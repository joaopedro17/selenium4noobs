# 5.4 - Execução Headless

O modo **headless** executa o navegador sem interface gráfica — sem janela visível na tela. É essencial para rodar testes em ambientes sem display, como servidores e pipelines de CI/CD, e também acelera a execução em máquinas locais.

## Quando usar headless?

| Situação | Headless? |
|----------|-----------|
| Desenvolvimento local (debugging) | Não — é mais fácil ver o que acontece |
| CI/CD (GitHub Actions, Jenkins) | Sim — não há display disponível |
| Execução rápida em lote | Sim — sem renderização, roda mais rápido |
| Teste de comportamento visual / screenshots | Depende — pode haver diferenças de renderização |

## Chrome Headless

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;

public class HeadlessExample {
    public static void main(String[] args) {
        ChromeOptions options = new ChromeOptions();
        options.addArguments("--headless=new");   // Modo headless moderno (Chrome 112+)
        options.addArguments("--window-size=1920,1080"); // Define resolução virtual

        // Necessário em Linux sem root (containers, CI)
        options.addArguments("--no-sandbox");
        options.addArguments("--disable-dev-shm-usage");

        WebDriver driver = new ChromeDriver(options);

        driver.get("https://exemplo.com");
        System.out.println("Título: " + driver.getTitle());

        driver.quit();
    }
}
```

> **`--headless=new`** é a flag recomendada a partir do Chrome 112. A flag antiga `--headless` ainda funciona mas usa uma implementação legada.

## Firefox Headless

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.firefox.FirefoxOptions;

public class FirefoxHeadlessExample {
    public static void main(String[] args) {
        FirefoxOptions options = new FirefoxOptions();
        options.addArguments("-headless");

        WebDriver driver = new FirefoxDriver(options);

        driver.get("https://exemplo.com");
        System.out.println("Título: " + driver.getTitle());

        driver.quit();
    }
}
```

## Integrando headless ao Driver Factory

Uma boa prática é controlar o modo headless via variável de ambiente ou propriedade do sistema, para não precisar modificar o código entre ambientes:

```java
private static WebDriver createDriver() {
    String browser = System.getProperty("browser", "chrome");
    boolean headless = Boolean.parseBoolean(System.getProperty("headless", "false"));

    switch (browser.toLowerCase()) {
        case "firefox": {
            FirefoxOptions options = new FirefoxOptions();
            if (headless) options.addArguments("-headless");
            return new FirefoxDriver(options);
        }
        case "chrome":
        default: {
            ChromeOptions options = new ChromeOptions();
            if (headless) {
                options.addArguments("--headless=new");
                options.addArguments("--no-sandbox");
                options.addArguments("--disable-dev-shm-usage");
                options.addArguments("--window-size=1920,1080");
            }
            return new ChromeDriver(options);
        }
    }
}
```

Para rodar headless via Maven:

```bash
mvn test -Dheadless=true
```

Para rodar com browser visível (padrão):

```bash
mvn test
```

## Tirando screenshots em headless

Mesmo sem interface, você pode capturar screenshots — útil para depurar falhas no CI:

```java
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import java.io.File;
import org.apache.commons.io.FileUtils;

// Dentro do teste, ao capturar uma falha:
File screenshot = ((TakesScreenshot) driver).getScreenshotAs(OutputType.FILE);
FileUtils.copyFile(screenshot, new File("target/screenshot-falha.png"));
```

---

Parabéns por chegar até aqui! Você percorreu todo o conteúdo do **selenium4noobs**, do básico ao avançado. Agora você tem as ferramentas para escrever, organizar e escalar testes de automação com Selenium.

Voltar para: [README](../README.md)
