![valid XHTML][checkmark]
[checkmark]: https://raw.githubusercontent.com/mozgbrasil/mozgbrasil.github.io/master/assets/images/logos/Red_star_32_32.png "MOZG"
[url-method]: https://www.loggi.com/contas/criar/GH5THM/
[requerimentos]: http://mozgbrasil.github.io/requerimentos/
[contact-loggi]: http://api.docs.dev.loggi.com/intro.html
[tickets]: https://cerebrum.freshdesk.com/support/tickets/new
[preco]: http://www.cerebrum.com.br/preco/
[github-boxpacker]: https://github.com/mozgbrasil/magento-boxpacker-php55#mozgboxpacker
[getcomposer]: https://getcomposer.org/
[uninstall-mods]: https://getcomposer.org/doc/03-cli.md#remove

# Mozg\Loggi

## Sinopse

Integração a [Loggi][url-method]

## Motivação

Atender o mercado de módulos para Magento oferecendo melhorias e um excelente suporte

## Suporte / Dúvidas

Para obter o devido suporte [Clique aqui][tickets], relatando o motivo da ocorrência o mais detalhado possível e anexe o print da tela para nosso entendimento

## Preço

[Clique aqui][preco]

## Recursos

- Definir dimensões para os produtos

- Definir dimensões, peso e valor da Embalagem/Caixa

- Opção para embalar os produtos separadamente ou combinar na mesma Embalagem/Caixa

- Embalagem do produto inteligente, exibe como os produtos serão agrupados e calcula o peso dimensional fazendo o envio fracionado se necessário

- Armazenamento das requisições em Cache

~~- Definir como diferentes combinações de produtos são embalados em conjunto # TODO~~

~~- Atribuir produtos a determinadas caixas (produtos múltiplos podem ser atribuídos à mesma caixa) # TODO~~

## Característica técnica

Atualmente diversos módulos de terceiros relativo a métodos de entrega sempre soma o peso e dimensões dos produtos gerando falha na requisição a transportadora devido não terem um sistema que separa os produtos em sua devida embalagem distribuindo seu peso.

O nosso módulo foi desenvolvido visando total transparência dos processos executados, para efeito de análise visualize os processos armazenado em log.

A extensão permite você definir as dimensões de seus produtos, as dimensões de suas embalagens e regras de como empacotar diferentes combinações de produtos em conjunto.

A extensão escolhe qual embalagem será utilizado para embalar os produtos para o pedido.

A extensão pode distribuir os produtos em diversas embalagens até o peso máximo suportado para a embalagem.

Como será cadastrado a embalagem com as dimensões e peso suportado pelas transportadoras não deve ocorrer falha relativa as dimensões ou peso.

A primeira coisa a se levar em consideração no uso do módulo é o [Gerenciamento de Embalagem/Caixa][github-boxpacker], como já vem alguns registros pré inseridos certifique se de atualizar os registros conforme sua necessidade.

Certifique se ter cadastrado as devidas dimensões para os produtos.

Para cada embalagem é feito uma requisição a transportadora onde é passado os devidos parâmetros

O módulo possui armazenamento de cache

Na finalização do pedido é armazenado no histórico do pedido um comentário contendo um identificador único que poderá ser usado para consulta no arquivo de log a discriminação dos pacotes seus itens e a visualização de cada pacote com seus itens em 3D

Sempre confira as informações de frete antes de processar cada pedido, caso algo esteja inconsistente será necessário cancelar o pedido até a correção da ocorrência

Para o rastreamento do pacote é feito acesso ao WebService onde é passado os devidos parâmetros e exibido o devido retorno

## Instalação - Atualização - Desinstalação - Desativação

Este módulo destina-se a ser instalado usando o [Composer][getcomposer]

Antes de executar os processos, [clique aqui][requerimentos] e leia os pré-requisitos e sugestões

--

Certique se da existencia do arquivo composer.json na raiz do projeto Magento e que o mesmo tenha os trechos "minimum-stability", "prefer-stable", "repositories" e '"magento-root-dir":"./"', conforme https://gist.github.com/mozgbrasil/0c9bb8792ea6273ea24aed30b0fbcfba

Caso não exista o arquivo composer.json na raiz do projeto Magento, efetue o download

	wget https://gist.githubusercontent.com/mozgbrasil/0c9bb8792ea6273ea24aed30b0fbcfba/raw/9b514bc896171b6d75833b6f165065356f62ca59/composer.json

--

Para qualquer atualização no Magento sempre mantenha o Compiler e o Cache desativado

--

### Para instalar o módulo execute o comando a seguir no terminal do seu servidor

	composer require mozgbrasil/magento-loggi-php55:dev-master

Você pode verificar se o módulo está instalado, indo ao backend em:

	STORES -> Configuration -> ADVANCED/Advanced -> Disable Modules Output

--

### Para atualizar o módulo execute o comando a seguir no terminal do seu servidor

	composer clear-cache && composer update

Na ocorrência de erro, renomeie a pasta /vendor/mozgbrasil e execute novamente

Para checar a data do módulo execute o seguinte comando

	grep -ri --include=*.json 'time": "' ./vendor/mozgbrasil

--

### Para [desinstalar][uninstall-mods] o módulo execute o comando a seguir no terminal do seu servidor

	composer remove mozgbrasil/magento-loggi-php55 && composer clear-cache && composer update

--

### Para desativar o módulo

Renomeie a pasta do módulo

Cada módulo tem a sua pasta no diretório /vendor/mozgbrasil/

--

## Como configurar o método de entrega

Antes de configurar o módulo você deve cadastrar o CEP de origem, indo ao backend em:

	STORES -> Configuration -> Sales/Shipping Settings -> Origin

Para configurar o método de entrega, acesse no backend em:

	STORES -> Configuration -> Sales/Shipping Methods -> Loggi (powered by MOZG)

Você terá os campos a seguir

### • **Ativar**

Para "ativar" ou "desativar" o uso do método

### • **Ordem de exibição**

É a ordem apresentada em métodos de entrega no passo de fechamento de pedido

### • **Título**

Nome do método que deve ser exibido

### • **Serviços**

Selecione os serviços desejado, para selecionar mais de um, segure a tecla "Ctrl" e clique nos serviços

### • **Serviço Para Entrega Gratuita**

Quando houver um desconto de frete grátis, esse serviço terá o valor zero

### • **Calcular taxa de manuseio**

Podendo ser fixo ou percentual

### • **Taxa de Manuseio**

Será adicionado o valor ao frete

### • **Mostrar método se não aplicável**

Quando configurado como "Não", caso seja retornado algum serviço com erro, não será exibido o método de entrega

### • **Debug**

Deve ser armazenado os processos do módulo em var/log/

O arquivo 

DATE_mozg.log

se trata de log do módulo sendo um log mais detalhado contendo todos os processos inclusive das execuções realizadas pelas bibliotecas externas do módulo

O arquivo

shipping_METHOD.log

se trata de log nativo do magento relativo ao método de entrega

### • **Identificador do atributo largura dos produtos**

Permite definir o nome do atributo de largura dos produtos usado no projeto

### • **Identificador do atributo comprimento dos produtos**

Permite definir o nome do atributo de comprimento dos produtos usado no projeto

### • **Identificador do atributo altura dos produtos**

Permite definir o nome do atributo de altura dos produtos usado no projeto

### • **Unidade de medida**

Sendo o padrão do peso do produto como kilo

Caso esteja usando a unidade de massa em gramas, tanto os produtos como as embalagens devem respeitar o mesmo padrão

Ao informar na configuração do método o uso da unidade de massa em gramas é feito a conversão do peso de grama para kilo

1 Kg no formato "Kilo" será "1.000", já em "Gramas" será "1000.000"

### • **Mostrar serviço com retorno de erro**

Quando configurado como "Não", caso seja retornado algum serviço com erro, o mesmo não deve ser exibido no método de entrega

### • **Modo teste/produção**

Informe o ambiente a ser usado

### • **e-mail Ambiente de teste**

Informe o e-mail

### • **API Key Ambiente de teste**

Informe o API Key

Para obter acesse

https://staging.loggi.com/contas/haxor/

### • **e-mail Ambiente de produção**

Informe o e-mail

### • **API Key Ambiente de produção**

Informe o API Key

Para obter acesse

https://loggi.com/contas/haxor/

### • **Método de Pagamento**

Selecione o método previamente cadastrado no sistema da Loggi em:

Configurações -> Empresa -> Meios de Pagamentos 

## Quais os recursos do módulo

- [✓] Cálculo do frete
- [✓] Solicitação de coleta

## Perguntas mais frequentes "FAQ"

### Como aplicar o Frete Grátis

Na configuração do módulo para o método de entrega é possível definir o "Serviço Para Entrega Gratuita" recurso que deve ser aplicado quando definido a ação de "Frete Grátis" nas "Regras da Promoção"

No Backend do Magento, acesse o menu: Promoções -> Regras de Promoção -> Criar regra -> Crie uma regra e defina na aba "Ações" o uso do Frete Grátis

Dessa forma na exibição do cálculo do frete será exibido para o serviço escolhido o valor zerado

Esse recurso se trata de regra nativa do Magento caso tenha algum problema sugiro desativar todas as regras de promoção e ativar uma de cada vez até encontrar o motivo da ocorrência

### Dados de contato - Loggi

Para entrar em contato com a [Loggi][contact-loggi]

## Manual

http://api.docs.dev.loggi.com/

## Contribuintes

Equipe Mozg

## License

[Comercial License] (LICENSE.txt)

## Badges

[![Join the chat at https://gitter.im/mozgbrasil](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/mozgbrasil/)
[![Latest Stable Version](https://poser.pugx.org/mozgbrasil/magento-loggi-php55/v/stable)](https://packagist.org/packages/mozgbrasil/magento-loggi-php55)
[![Total Downloads](https://poser.pugx.org/mozgbrasil/magento-loggi-php55/downloads)](https://packagist.org/packages/mozgbrasil/magento-loggi-php55)
[![Latest Unstable Version](https://poser.pugx.org/mozgbrasil/magento-loggi-php55/v/unstable)](https://packagist.org/packages/mozgbrasil/magento-loggi-php55)
[![License](https://poser.pugx.org/mozgbrasil/magento-loggi-php55/license)](https://packagist.org/packages/mozgbrasil/magento-loggi-php55)
[![Monthly Downloads](https://poser.pugx.org/mozgbrasil/magento-loggi-php55/d/monthly)](https://packagist.org/packages/mozgbrasil/magento-loggi-php55)
[![Daily Downloads](https://poser.pugx.org/mozgbrasil/magento-loggi-php55/d/daily)](https://packagist.org/packages/mozgbrasil/magento-loggi-php55)
[![Reference Status](https://www.versioneye.com/php/mozgbrasil:magento-loggi-php55/reference_badge.svg?style=flat-square)](https://www.versioneye.com/php/mozgbrasil:magento-loggi-php55/references)
[![Dependency Status](https://www.versioneye.com/php/mozgbrasil:magento-loggi-php55/1.0.0/badge?style=flat-square)](https://www.versioneye.com/php/mozgbrasil:magento-loggi-php55/1.0.0)

:cat2: