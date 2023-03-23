# 3.2 - WebDriver

O WebDriver é um recurso que se acopla à API do Selenium, permitindo a execução direta de testes via código fonte no servidor.<br>
Ele faz uso de uma API JavaScript extensiva que, aliada ao motor JavaScript do próprio navegador, permite acesso ao documento DOM completo da página HTML, assim como a manipulação de suas propriedades e funções.<br> Em outras palavras, ele permite traduzir o código de servidor para o código de cliente e executar nossos testes automatizados no universo front-end dos diversos browsers suportados.

## Drivers de browsers

O Selenium WebDriver usa o próprio driver do navegador para a automação. É a forma mais moderna de interação atualmente, pois cada browser possui o seu respectivo driver, permitindo a interação entre o script de teste e o respectivo browser.<br>

Dito isso, pra cada navegador que você for rodar a automação é necessário fazer o download do driver correspondente.

Exemplo:

- Chrome => <a href="https://chromedriver.chromium.org/downloads">ChromeDriver</a><br>
- Firefox => <a href="https://github.com/mozilla/geckodriver/releases">geckodriver</a><br>

## 

Ir para: [3.3 Abrindo navegador](3-Abrindo-navegador.md)
