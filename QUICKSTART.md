# Guia de Início Rápido - Quick Scrapping

Este guia fornece instruções sobre como começar a usar a biblioteca Quick Scrapping.

## Instalação

```bash
pip install quick-scrapping
```

## Exemplos Básicos

### Utilizando a Interface de Linha de Comando

A biblioteca vem com uma interface de linha de comando para uso rápido:

```bash
# Extrair dados de uma URL e salvar como JSON
quick-scrapper https://www.exemplo.com -o dados.json

# Extrair dados de uma URL e salvar como CSV
quick-scrapper https://www.exemplo.com -o dados.csv -f csv

# Executar em modo headless (sem interface gráfica)
quick-scrapper https://www.exemplo.com --headless

# Aumentar o tempo de espera para sites mais lentos
quick-scrapper https://www.exemplo.com --wait 5
```

### Usando o Módulo Selenium

```python
from quick_scrapping.selenium_functions import SeleniumHelper

# Inicializa o Selenium Helper
with SeleniumHelper(browser_type="chrome", headless=False) as selenium:
    # Navega para uma URL
    selenium.navigate.to("https://www.exemplo.com")
    
    # Espera por um elemento e preenche um campo
    selenium.element.wait_for_element("name", "q", timeout=10)
    selenium.interact.type_text("name", "q", "Python web scraping")
    
    # Clica em um botão
    selenium.interact.click("name", "btnK")
    
    # Aguarda o carregamento da página
    selenium.element.wait_for_page_load()
    
    # Tira uma captura de tela
    selenium.utils.take_screenshot("busca.png")
```

### Usando o Módulo BeautifulSoup

```python
from quick_scrapping.beautifulsoup_functions import HTMLParser, DataExtractor

# Inicializa o parser e o extrator
parser = HTMLParser()
extractor = DataExtractor()

# Carrega HTML de uma URL
soup = parser.load_from_url("https://www.exemplo.com")

# Extrai metadados
metadata = extractor.extract_metadata(soup)
print(f"Título da página: {metadata.get('title', 'N/A')}")

# Extrai links
links = extractor.extract_links(soup)
print(f"Encontrados {len(links)} links")

# Extrai tabelas
for i, table in enumerate(soup.find_all("table")):
    headers, rows = extractor.extract_table(table)
    print(f"Tabela {i+1}: {len(rows)} linhas x {len(headers)} colunas")

# Limpa o HTML e extrai texto
cleaned_soup = parser.clean_html(soup, remove_scripts=True, remove_styles=True)
text = parser.extract_text(cleaned_soup)
print(f"Texto extraído: {text[:100]}...")
```

### Usando a Integração entre Selenium e BeautifulSoup

```python
from quick_scrapping.selenium_functions import SeleniumHelper
from quick_scrapping.beautifulsoup_functions import HTMLParser, DataExtractor
from quick_scrapping.common import ScrapingHelper

# Inicializa os componentes
selenium = SeleniumHelper(browser_type="chrome", headless=True)
parser = HTMLParser()
extractor = DataExtractor()

# Cria o helper integrado
scraper = ScrapingHelper(
    selenium_helper=selenium,
    bs_parser=parser,
    bs_extractor=extractor
)

try:
    # Navega para a URL e extrai dados
    soup = scraper.navigate_and_extract(
        url="https://www.exemplo.com",
        wait_for_selector="#conteudo",
        wait_time=3
    )
    
    if soup:
        # Extrai dados
        title = soup.title.text if soup.title else "Sem título"
        links = extractor.extract_links(soup, make_absolute=True)
        
        # Salva resultados
        scraper.save_results(
            data={"title": title, "links": links},
            output_file="resultados.json",
            format="json"
        )
        
finally:
    # Fecha o navegador
    selenium.close()
```

## Próximos Passos

Consulte a documentação completa em [https://quick-scrapping.readthedocs.io/](https://quick-scrapping.readthedocs.io/) para exemplos mais avançados e detalhes sobre todas as funcionalidades disponíveis.