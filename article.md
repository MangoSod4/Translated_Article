# Azure Open AI na VNet - DEV Community

## Visão Geral

Os modelos GPT estão atualmente hospedados em múltiplos provedores de serviços, e a Microsoft Azure é um deles. 
Embora os modelos em si sejam os mesmos, há várias diferenças, incluindo:

- Custo
- Funcionalidades
- Tipo e versão dos modelos
- Localização geográfica
- Segurança
- Suporte
- Entre outros.

Um dos aspectos mais importantes ao usá-lo em um ambiente empresarial é, claro, a segurança. Utilizando recursos de 
segurança de rede da Azure com o Azure Open AI, os clientes podem consumir o serviço Open AI a partir de e dentro 
da VNet (Rede Virtual), garantindo que nenhuma informação seja transmitida publicamente.

## Exemplo de Implantação

O repositório de exemplos da Azure fornece arquivos Bicep de exemplo para implantar o Azure Open AI em um ambiente VNet.

Repositório GitHub: [openai-enterprise-iac](https://github.com/Azure-Samples/openai-enterprise-iac)

### Principais recursos que o Bicep usa:

- **VNet**
- **Integração da VNet para Web App**
- **Ponto de Extremidade Privado para Azure Open AI**
- **Ponto de Extremidade Privado para Cognitive Search**
- **Zona de DNS Privada**

Utilizando esses recursos, todo o tráfego de saída do aplicativo web é roteado apenas dentro da VNet, e todos os nomes 
são resolvidos em endereços IP privados. O Open AI e o Cognitive Search desativam o endereço IP público, eliminando a 
disponibilidade de uma interface pública.

## Implantação

O arquivo Bicep irá implantar os seguintes recursos da Azure.

Vamos implantar e confirmar como funciona. Criei um grupo de recursos na região leste dos EUA para o meu teste.

```bash
git clone https://github.com/Azure-Samples/openai-enterprise-iac
cd openai-enterprise-iac
az group create -n openaitest -l eastus
az deployment group create -g openaitest -f ./infra/main.bicep
```

Depois de executar o comando acima, o processo de implantação é iniciado. Aguarde até a conclusão da implantação.

## Teste

Vamos verificar se a implantação foi bem-sucedida.

### Azure Open AI

Primeiro, vamos tentar o acesso público. Consegui criar uma implantação sem problemas. No entanto, ao tentar a partir 
do playground de Chat no meu portal Azure, vejo o seguinte erro.

### Como acessar via API Web?

De uma ferramenta avançada do App Service, faço login na sessão Bash e, primeiramente, faço ping no URL do serviço. 
Vejo que o endereço IP privado atribuído ao Ponto de Extremidade Privado é retornado. Em seguida, uso o comando `curl` 
para enviar uma solicitação ao endpoint.

---

## Conclusão

Essa configuração permite que todo o tráfego seja restrito à VNet, melhorando a segurança ao usar o Azure Open AI em ambientes empresariais.