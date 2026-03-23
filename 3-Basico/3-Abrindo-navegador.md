# 3.3 - Abrindo navegador

Após configurar o projeto, podemos criar um script simples para abrir o navegador.

Então vamo lá!

## Abordagem moderna (Selenium 4 + Selenium Manager)

A partir do **Selenium 4.6+**, o **Selenium Manager** configura o driver do navegador automaticamente. Você não precisa baixar o ChromeDriver, geckodriver nem usar `System.setProperty`. Basta instanciar o driver diretamente:

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class AbrindoNavegador {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();
    }
}
```

### Manipulando a dimensão do browser

Após o script rodar, você vai perceber que a janela do browser não está maximizada. Para maximizar:

```java
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class AbrindoNavegador {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        // Sempre encerre o driver ao final
        driver.quit();
    }
}
```

## Abordagem legada (antes do Selenium 4.6)

Caso esteja usando uma versão mais antiga do Selenium, é necessário indicar manualmente o caminho do driver com `System.setProperty`:

```java
System.setProperty("webdriver.chrome.driver", "C://chromedriver.exe");
WebDriver driver = new ChromeDriver();
```

> **Recomendação:** Use sempre a versão mais recente do Selenium para aproveitar o Selenium Manager e evitar a manutenção manual dos drivers.

Ir para: [3.4 Scripts simples](4-Scripts-simples.md)
