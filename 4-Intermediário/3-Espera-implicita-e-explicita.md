# 4.3 Espera implícita e explícita

No Selenium, as esperas são usadas para sincronizar a execução do script de automação com o comportamento do navegador. Isso é útil para lidar com elementos que podem demorar a ser carregados ou se tornarem interativos. As duas principais formas de espera são Implicit Wait e Explicit Wait.

## Comparação Entre Implicit Wait e Explicit Wait
| **Característica**     | **Implicit Wait**    | **Explicit Wait**             |
| ---------------------- | -------------------- | ----------------------------- |
| **Escopo**             | Global               | Elemento específico           |
| **Condição**           | Nenhuma              | Condição explícita necessária |
| **Configuração**       | Única vez            | Configurado para cada uso     |
| **Código mais claro?** | Sim                  | Não necessariamente           |
| **Uso recomendado**    | Para esperas simples | Para condições complexas      |

## Práticas Recomendadas
1. Evitar misturar implicit e explicit wait: Isso pode levar a comportamento imprevisível.

2. Prefira explicit wait: Especialmente em aplicações complexas com diferentes tempos de carregamento de elementos.

3. Defina tempos razoáveis: Evite tempos longos desnecessários para melhorar a eficiência do script.

## Implicit Wait (Espera Implícita)

- **Definição:** Configura um tempo padrão de espera para que os elementos se tornem visíveis ou disponíveis no DOM antes de lançar uma exceção NoSuchElementException.

- **Escopo:** Aplica-se globalmente a todas as buscas de elementos feitas pelo WebDriver.

- **Uso:** É útil quando todos os elementos em sua automação têm tempos semelhantes de carregamento.

```Java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

import java.util.concurrent.TimeUnit;

public class ImplicitWaitExample {
    public static void main(String[] args) {
        // Configuração do WebDriver
        System.setProperty("webdriver.chrome.driver", "caminho/para/chromedriver");
        WebDriver driver = new ChromeDriver();

        // Definindo espera implícita
        driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);

        // Acessando uma página
        driver.get("https://exemplo.com");

        // Tentando encontrar um elemento
        driver.findElement(By.id("elementoId")).click();

        // Finalizando o WebDriver
        driver.quit();
    }
}

```

> **Limitação:** Pode aumentar o tempo de execução se o elemento já estiver disponível rapidamente, pois o Selenium ainda tentará esperar pelo tempo especificado.

## Explicit Wait (Espera Explícita)

- **Definição:** Permite definir uma condição explícita para esperar por um elemento ou estado específico. Por exemplo, esperar até que um elemento seja clicável, visível ou presente no DOM.

- **Escopo:** Aplica-se somente ao elemento ou condição especificada.

- **Uso:** É mais flexível e geralmente preferido em cenários onde diferentes elementos têm diferentes tempos de carregamento.

```Java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.ExpectedConditions;
import org.openqa.selenium.support.ui.WebDriverWait;

import java.time.Duration;

public class ExplicitWaitExample {
    public static void main(String[] args) {
        // Configuração do WebDriver
        System.setProperty("webdriver.chrome.driver", "caminho/para/chromedriver");
        WebDriver driver = new ChromeDriver();

        // Acessando uma página
        driver.get("https://exemplo.com");

        // Definindo espera explícita
        WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(10));

        // Esperando até que o elemento esteja clicável
        WebElement elemento = wait.until(ExpectedConditions.elementToBeClickable(By.id("elementoId")));
        elemento.click();

        // Finalizando o WebDriver
        driver.quit();
    }
}

```

> **Vantagens:** Permite configurar diferentes condições e tempos para elementos específicos, o que é útil em situações complexas.

Ir para: [4.4 Injeção de JavaScript](4-Injecao-de-javascript.md)
