# 4.4 Injeção de JavaScript

A injeção de JavaScript no Selenium refere-se ao uso de scripts JavaScript dentro de testes automatizados para interagir diretamente com a página da web. Isso é feito através do método `executeScript` da interface `JavascriptExecutor`.

> **Regra de ouro:** Use injeção de JavaScript somente como **último recurso**. Se o Selenium consegue interagir com o elemento via `findElement`, `click`, `sendKeys` etc., prefira sempre essa abordagem — ela é mais legível, mais fácil de manter e testa o comportamento real da UI.

## Quando realmente usar a injeção de JavaScript

Situações em que o JavaScript é a ferramenta certa:

1. **Elementos inacessíveis via Selenium:**
   > Elementos com `display: none` ou `visibility: hidden` que o Selenium se recusa a clicar, mas que precisam ser manipulados de alguma forma no contexto do teste.

2. **Simular eventos JavaScript nativos:**
   > Eventos como `onmouseover`, `onfocus`, ou eventos personalizados disparados via `dispatchEvent` que não têm equivalente direto no WebDriver.

3. **Extrair informações calculadas do DOM:**
   > Valores como estilos CSS aplicados dinamicamente (`getComputedStyle`), propriedades de scroll ou dados expostos apenas via JavaScript.

4. **Scroll programático:**
   > Rolar a página até um elemento específico antes de interagir — especialmente útil em páginas longas ou com lazy loading.

5. **Contornar restrições da aplicação:**
   > Em ambiente de testes/QA, forçar o estado de campos somente-leitura, desabilitar validações ou alterar flags internas para testar casos extremos.

## Cuidados e Melhores Práticas

- **Evite para ações simples:** Não use `jsExecutor.executeScript("arguments[0].click()", el)` se `el.click()` funciona — o JS bypassa a visibilidade e pode mascarar bugs reais de UI.

- **Teste os scripts JavaScript:** Certifique-se de que os scripts são seguros e não alteram o comportamento esperado do site de forma não planejada.

- **Validação de retorno:** Sempre valide o retorno do script (se houver) para garantir que o comportamento foi como esperado.

- **Documentação:** Documente claramente os casos em que o JavaScript foi usado para facilitar a manutenção futura.

## Implementação

A implementação é feita através da interface `JavascriptExecutor`. Aqui está um exemplo com os casos de uso mais comuns:

```java
import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class JavaScriptInjectionExample {
    public static void main(String[] args) {
        // Selenium Manager configura o driver automaticamente (Selenium 4.6+)
        WebDriver driver = new ChromeDriver();

        // Navegar para a página de teste
        driver.get("https://exemplo.com");

        // Obter uma instância do JavascriptExecutor
        JavascriptExecutor jsExecutor = (JavascriptExecutor) driver;

        // Caso de uso 1: Scroll até um elemento (útil em páginas longas)
        WebElement element = driver.findElement(By.id("elementoId"));
        jsExecutor.executeScript("arguments[0].scrollIntoView(true);", element);

        // Caso de uso 2: Alterar o valor de um campo somente-leitura
        // (use apenas quando sendKeys() não funcionar por restrição da aplicação)
        jsExecutor.executeScript("arguments[0].value='Texto alterado via JS';", element);

        // Caso de uso 3: Obter o título da página
        String title = (String) jsExecutor.executeScript("return document.title;");
        System.out.println("Título da página: " + title);

        // Caso de uso 4: Clicar em botão invisível
        // (use apenas quando o elemento estiver intencionalmente oculto no DOM)
        WebElement hiddenButton = driver.findElement(By.id("botaoOculto"));
        jsExecutor.executeScript("arguments[0].click();", hiddenButton);

        // Finalizando o WebDriver
        driver.quit();
    }
}
```

## Explicação do Código

1. **Obter uma instância do JavascriptExecutor:**
   - O driver Selenium implementa essa interface, e você pode convertê-lo diretamente com um cast.

2. **Injetar JavaScript:**
   - Use o método `executeScript` para injetar e executar comandos JavaScript.
   - O método aceita um script JavaScript como `String` e parâmetros opcionais.

3. **Passagem de parâmetros:**
   - `arguments[0]`, `arguments[1]` representam os argumentos passados para o script após a string do script.

4. **Retorno de valores:**
   - Se o script retornar um valor (ex: `return document.title`), faça o cast para o tipo esperado.

Ir para: [4.5 TestNG](5-TestNG.md)
