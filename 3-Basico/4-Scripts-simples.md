# 3.4 - Script simples

Agora que já vimos o básico para abrirmos o navegador, vamos fazer um script bem simples para acessar o site da He4rt e colocar no modo escuro, bora?

- Para acessar o site desejado temos que usar a função `get()`.
- Vamos usar o `findElement` com o `By` para identificar o tipo de locator.
- Depois podemos passar o `click()`.

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class ScriptSimples {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://heartdevs.com");

        // Botão de mudança de tema
        driver.findElement(By.xpath("//*[@id='inicio']/div[1]/div[1]/svg[2]")).click();

        driver.quit();
    }
}
```

## Pesquisa no google

Agora vamos fazer uma pesquisa no google.

- Nesse caso usamos o `sendKeys()` onde passo a string que desejo.

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class PesquisaGoogle {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://google.com");

        // Campo de input de texto
        driver.findElement(By.cssSelector("body > div.L3eUgb > div.o3j99.ikrT4e.om7nvf > form > div:nth-child(1) > div.A8SBwf > div.RNNXgb > div > div.a4bIc > input")).sendKeys("He4rt");

        // Botão de pesquisa/lupa
        driver.findElement(By.xpath("/html/body/div[1]/div[3]/form/div[1]/div[1]/div[1]/div/div[1]/div/span")).click();

        driver.quit();
    }
}
```

## Armazenando texto do elemento

Podemos também armazenar o texto de um elemento para alguma validação. Por exemplo, você está efetuando uma compra e pode verificar se os preços no final batem.

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;

public class ArmazenandoTexto {
    public static void main(String[] args) {
        WebDriver driver = new ChromeDriver();

        driver.manage().window().maximize();

        driver.get("https://google.com");

        // Botão de pesquisa
        String googleSearch = driver.findElement(By.cssSelector("body > div.L3eUgb > div.o3j99.ikrT4e.om7nvf > form > div:nth-child(1) > div.A8SBwf > div.FPdoLc.lJ9FBc > center > input.gNO89b")).getText();

        // Botão estou com sorte
        String feelingLucky = driver.findElement(By.xpath("/html/body/div[1]/div[3]/form/div[1]/div[1]/div[4]/center/input[2]")).getText();

        System.out.println("Pesquisa: " + googleSearch);
        System.out.println("Estou com sorte: " + feelingLucky);

        driver.quit();
    }
}
```

Ir para: [4.1 Page object](../4-Intermediário/1-Page-object.md)
