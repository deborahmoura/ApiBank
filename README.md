<h1 align="center"> API de consulta de bancos por estado de atuação e juros </h1>





## Conceitos de Testes de API
* API é um conjunto de rotinas e padrões de programação para acesso a um aplicativo de software ou plataforma baseado na Web;
* A sigla API refere-se ao termo em inglês “Application Programming Interface”, que significa em tradução para o português “Interface de Programação de Aplicativos”.
* Estão entre a camada de testes de UI e Unitários;
* Podem ser automatizados em paralelo com o desenvolvimento da API;
* Facilitam a validação de múltiplos cenários;
* Garantem que a estrutura do JSON de retorno esteja correta.


## Tipos de Requisições utilizadas 

* GET - Obter os dados de um recurso
* POST - Criar um novo recurso
* PUT - Substituir _completamente_ os dados de um determinado recurso
* PATCH - Atualiza _parcialmente_ um determinado recurso 
* DELET - Excluir um determinado recurso

## Status Code

* 100 (*) - Informal 
* 200 (*) - Sucess
* 300 (*) - Redirection 
* 400 (*) - Client Erros
* 500 (*) - Server Error 


## Variável de Ambiente

* URL Token https://8dac9f4e-fcb2-4e8f-857a-e4ed3497a0d8.mock.pstmn.io/auth

* URL Endpoint https://8dac9f4e-fcb2-4e8f-857a-e4ed3497a0d8.mock.pstmn.io/bank

## Tipos de teste 

## Testes de Schema
Tem como objetivo garantir que o conteúdo fornecido não tenha sido modificado, poderá mostrar que tem permissão para validar se o contrato acordado foi ou não foi quebrado; deve validar se o esquema permanecerá o mesmo, assim como a integridade dos dados na comunicação entre cliente / servidor; É possível validar se os dados continuam iguais, como os valores limites, uma restrição de valores recebidos, se uma estrutura foi ou não modificada etc.

Em poucas palavras, validação do tipo primitivo dos atributos. Ex: RESPONSE

``` [
    {
        "id": 1,
        "banco": "Banco do Brasil",
        "estadoAtuacao": "SC",
        "juros": "11,25%"
    },
```

TESTE 

```
var responseSchema = {
              "type": "array",
                "items": {
                "type": "object",
                "properties": {
                    "id": {
                        "type": "number"
                    },
                    "banco": {
                        "type": "string"
                    },
                    "estadoAtuacao": {
                        "type": "string"
                    },
                    "juros": {
                        "type": "string"
                    }
                    
                }
                }       
}
```
## Testes positivos- negativos

Para assegurar que a API contempla os fluxos positivos e negativos (erros)


 ## Testes da estrutura de dados de resposta 

O teste é bem simples, estou utilizando a biblioteca tv4 para fazer a validação. Basicamente o que fazemos é salvar a resposta que recebemos em uma variável e depois utilizar a biblioteca para verificar se a resposta obedece o esquema que esperamos.

``` [
    {
        "id": 1,
        "banco": "Banco do Brasil",
        "estadoAtuacao": "SC",
        "juros": "11,25%"
    },
```
```
pm.test('Validação da estrutura do JSON', function () {
    var validation = tv4.validate(jsonData, responseSchema);
    pm.expect(validation).to.be.true;
});
```

## Teste de Conteúdo 

RESPONSE 
```
{
    "error": {
        "name": "URIError",
        "message": "The provided request parameter is invalid. Please sanitize the parameter before trying again.",
        "details": {
            "param": "11,25%"
        }
    }
}
```
TESTES
```
pm.test("O tipo de conteúdo deve ser JSON", function () {
    pm.response.to.have.header("Content-Type", "application/json; charset=utf-8");
});
```


## Instruções para rodar a automação em sua máquina
Necessário instalar o NodeJs e abrir o cmd, executar o seguinte comando para instalar o newman.
> npm install -g newman

## Relatório HTML
Instalar a dependência
> npm install -g newman-reporter-htmlextra

Rodando testes com newman:
obs: poderá usar qualquer uma das collections.
>newman run ApiBank.json -rhtmlextra

Dentro da pasta do arquivo no qual está o projeto será criado uma pasta chamada newman e a evidência estará contida nela 



![Image](https://user-images.githubusercontent.com/75025389/226958083-e4a43ae5-c4e3-4406-be4a-d5b81b250601.png)



![Image](https://user-images.githubusercontent.com/75025389/226958125-ed7bb4fe-a4a5-43d3-b65a-da8839992115.png)

## Executar um fluxo de trabalho manualmente 

* No GitHub.com, navegue até a página principal do repositório.  Abaixo do nome do repositório, clique em  Actions. Guia Actions no menu de navegação do repositório principal



![Image](https://user-images.githubusercontent.com/75025389/227211364-3389f2ec-8dbb-4c79-a1c7-025cf4243ed9.png)


* Na barra lateral esquerda, clique no nome do fluxo de trabalho que deseja executar.  
* Acima da lista de execuções de fluxo de trabalho, selecione Executar fluxo de trabalho
* Selecione o menu suspenso Branch e clique em um branch para executar o fluxo de trabalho.
* Se o fluxo de trabalho exigir entradas, preencha os campos.
* Clique em Executar fluxo de trabalho.

![Image](https://user-images.githubusercontent.com/75025389/227211496-b4858a81-4fdd-4f36-bcb4-c6db72aa36f6.png)

* Consultando o relatório do newman 
* Clique no teste quando o mesmo estiver verde

![Image](https://user-images.githubusercontent.com/75025389/227212085-765c2b7e-abab-484d-8426-793c2e4541f2.png)

* no Summary vá até o final da página e encontre o report 

![Image](https://user-images.githubusercontent.com/75025389/227212352-90a5bdf0-5b0a-419d-af8b-8198167b5617.png)

* Um download será feito no seu computador com os status do teste.

OBS: O status report do newman pode ser personalizado, mas o padrão que foi colocado neste relatório traz as seguintes informações:

* Visualizações separadas para resumo e todas as solicitações. Há também uma guia separada para visualizar apenas as solicitações com falha.

* Muitas personalizações estão disponíveis com o repórter, como Mostrar logs do console, Não mostrar testes ignorados, Mostrar apenas falhas nos relatórios, etc. Todos os detalhes estão disponíveis na página do GitHub [aqui](https://github.com/DannyDainton/newman-reporter-htmlextra) para este módulo.

* Muitos códigos de cores para distinguir falhas e sucessos apenas olhando para o relatório.

* Mais interativo com exibições com guias, bem como opções de expandir/recolher para obter detalhes da solicitação.

* Todas as respostas de solicitação são capturadas totalmente, incluindo as informações do cabeçalho.

## Tecnologias utilizadas
* Postman - é uma ferramenta que tem como objetivo testar serviços RESTful (Web APIs) por meio do envio de requisições HTTP e da análise do seu retorno. Com ele é possível consumir facilmente serviços locais e na internet, enviando dados e efetuando testes sobre as respostas das requisições. O uso de Web APIs vem crescendo e o Postman auxilia nos testes desse tipo de projeto, bem como permite aos desenvolvedores analisarem o funcionamento de serviços externos, a fim de saber como consumi-los.

* Newman - é uma ferramenta de linha de comando que podemos utilizar para automatizar a execução dos scripts criados no postman. Isso é bem interessante pois dessa forma podemos junto da publicação de uma aplicação que expõe uma API Rest, por exemplo, já termos um teste de API automatizado.

* JSON -  é um formato que armazena informações estruturadas e é principalmente usado para transferir dados entre um servidor e um cliente. O arquivo é basicamente uma alternativa simples e mais leve ao XML (Extensive Markup Language), que tem funções similares.

* Git Action - É uma plataforma de integração contínua e entrega contínua (CI/CD) que permitiu automatizar a compilação e testar a pipeline de implantação. 

* GitHub - Possibilitando que vários membros do mesmo time trabalhem juntos em um projeto, cada um fazendo a sua versão.

* Javascrip - O Postman tem um runtime de testes baseado no node. js então é simples construir os casos de testes.


## extra
Segue uma collection que encontrou erros. [link](https://github.com/deborahmoura/ApiBank/blob/main/ApiBankError.json)

