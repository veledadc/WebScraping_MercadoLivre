# WebScraping MercadoLivre
Nesse estudo percorro mais de 200 paginas e faço a extração de mais de 10.000 itens com nome, preço e links todos em oferta no site Mercado Livre, isso tudo usando Python

```#  install
pip install scrapy 

# project
scrapy startproject mercadolivre

# diretorio
cd mercadolivre
# no diretorio gerar spider
scrapy genspider ml mercadolivre.com
```
# Exemplo

```
import scrapy


class MlSpider(scrapy.Spider):
    name = 'ml'

    start_urls = ['http://mercadolivre.com.br/ofertas']

    def parse(self, response, **kwargs):
        for i in response.xpath('//li[@class="promotion-item"]'):
            price = i.xpath('.//span[@class="promotion-item__price"]//text()').getall()
            title = i.xpath('.//p[@class="promotion-item__title"]/text()').get()
            link = i.xpath('.//a/href').get()

            yield{
                'price' : price, 
            'title' : title, 
            'link' : link
            }

            next_page = response.xpath('//a[contains(@title, "Próxima")]/@href').get()
            if next_page:
                yield scrapy.Request(url=next_page, callback=self.parse)
``` 
# Use as configurações 
```
BOT_NAME = 'mercadolivre'

SPIDER_MODULES = ['mercadolivre.spiders']
NEWSPIDER_MODULE = 'mercadolivre.spiders'

USER_AGENT = 'USER_AGENT'

ROBOTSTXT_OBEY = False

AUTOTHROTTLE_ENABLED = True

```

# Para salvar os resultados  
```
scrapy crawl ml -o nome_do_arquivo.csv

ou

scrapy crawl ml -o nome_do_arquivo.json

```
