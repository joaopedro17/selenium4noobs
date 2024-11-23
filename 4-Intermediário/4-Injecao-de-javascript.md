# 4.4 Injeção de JavaScript

A injeção de JavaScript no Selenium refere-se ao uso de scripts JavaScript dentro de testes automatizados para interagir diretamente com a página da web. Isso é feito através do método `executeScript` da interface `JavascriptExecutor`. Essa prática pode ser útil em diversos cenários em que as interações convencionais do Selenium (como cliques ou envio de textos) falham ou são limitadas.

## Motivos para usar a injeção de JavaScript

1. **Interagir com elementos não acessíveis via Selenium:**

       Alguns elementos podem estar ocultos ou fora da área visível do DOM, dificultando a interação direta. O JavaScript pode manipular esses elementos diretamente.

2. **Simular eventos ou interações complexas:**

       Eventos personalizados (como onmouseover, onfocus, ou outros eventos JavaScript complexos) podem ser acionados diretamente.

3. **Obter informações do DOM:**

       Extração de informações específicas que não estão acessíveis diretamente via APIs do Selenium, como valores calculados pelo navegador (ex.: estilos CSS aplicados dinamicamente).

4. **Desempenho:**

       Em alguns casos, executar scripts JavaScript é mais rápido do que usar as interações padrão do Selenium.

5. **Manipular tempo de execução:**

       Forçar o carregamento de elementos ou ignorar restrições temporais configuradas via JavaScript na página.


## Cuidados e Melhores Práticas

- **Evite dependência excessiva:** A injeção de JavaScript deve ser usada como último recurso. Prefira interações padrão do Selenium sempre que possível.

- **Teste scripts JavaScript:** Certifique-se de que os scripts são seguros e não alteram o comportamento esperado do site de forma não planejada.

- **Validação de retorno:** Sempre valide o retorno do script (se houver) para garantir que o comportamento foi como esperado.

- **Documentação:** Documente claramente os casos em que o JavaScript foi usado para facilitar a manutenção.

## Implementação

A implementação da injeção de JavaScript no Selenium é feita através da interface ``JavascriptExecutor``. Aqui está um exemplo de como usar a injeção:

```Java
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class JavaScriptInjectionExample {
    public static void main(String[] args) {
        // Configuração do WebDriver
        System.setProperty("webdriver.chrome.driver", "caminho/para/chromedriver");
        WebDriver driver = new ChromeDriver();

            // Navegar para a página de teste
            driver.get("https://exemplo.com");

            // Obter uma instância do JavascriptExecutor
            JavascriptExecutor jsExecutor = (JavascriptExecutor) driver;

            // Exemplo 1: Scroll até um elemento
            WebElement element = driver.findElement(By.id("elementoId"));
            jsExecutor.executeScript("arguments[0].scrollIntoView(true);", element);

            // Exemplo 2: Alterar o valor de um campo de texto
            jsExecutor.executeScript("arguments[0].value='Texto alterado via JS';", element);

            // Exemplo 3: Obter o título da página
            String title = (String) jsExecutor.executeScript("return document.title;");
            System.out.println("Título da página: " + title);

            // Exemplo 4: Clicar em um botão invisível
            WebElement hiddenButton = driver.findElement(By.id("botaoOculto"));
            jsExecutor.executeScript("arguments[0].click();", hiddenButton);

        // Finalizando o WebDriver
        driver.quit();
    }
}

```

## Explicação do Código

1. Obter uma instância do JavascriptExecutor:

- O driver Selenium implementa essa interface, e você pode convertê-lo diretamente.

2. Injetar JavaScript:

   - Use o método executeScript para injetar e executar comandos JavaScript.
O método aceita um script JavaScript como string e parâmetros opcionais.
Passagem de parâmetros:

   - `arguments[0], arguments[1]` representam os argumentos passados para o script.

3. Manipulações comuns:

   - Você pode executar comandos como alterar valores, rolar a página, interagir com elementos ou recuperar dados.
