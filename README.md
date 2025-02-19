# Automated API Tests using Postman CLI

Este repositório contém um conjunto de testes automatizados para a API ServeRest, utilizando Postman CLI e Newman. O objetivo é garantir a qualidade e a funcionalidade da API através de testes automatizados que são executados em um workflow do GitHub Actions.

## Descrição do Projeto

O projeto consiste em uma coleção de testes automatizados para a API ServeRest, que cobre diversas funcionalidades, como:

- **Usuários**: Cadastro, busca, alteração e exclusão de usuários.
- **Login**: Autenticação de usuários e geração de tokens.
- **Produtos**: Criação, busca, alteração e exclusão de produtos.
- **Carrinho**: Adição de produtos ao carrinho, busca e exclusão de carrinhos.
- **Deleção**: Exclusão de usuários, produtos e carrinhos.

Os testes são escritos em Postman e executados via Newman, que é a CLI do Postman, permitindo a integração contínua com o GitHub Actions.

## Workflow do GitHub Actions

O workflow é configurado para ser executado automaticamente a cada push no repositório. Ele realiza os seguintes passos:

1. **Checkout do código**: O código do repositório é clonado para o ambiente de execução.
2. **Instalação do Postman CLI**: O Postman CLI é instalado no ambiente Windows.
3. **Login no Postman CLI**: O login é realizado utilizando uma chave de API do Postman armazenada como segredo no GitHub.
4. **Execução dos testes**: A coleção de testes é executada utilizando o Postman CLI.

O arquivo de configuração do workflow (`main.yml`) está localizado na pasta `.github/workflows/`.

### Estrutura do Workflow

```yaml
name: Automated API Tests using Postman CLI

on: push

jobs:
  automated-api-tests:
    runs-on: windows-latest

    steps:
      - name: Checkout do código
        uses: actions/checkout@v4

      - name: Instalar Postman CLI
        run: |
          powershell.exe -NoProfile -InputFormat None -ExecutionPolicy AllSigned -Command "[System.Net.ServicePointManager]::SecurityProtocol = 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://dl-cli.pstmn.io/install/win64.ps1'))"

      - name: Login no Postman CLI
        run: |
          $env:POSTMAN_API_KEY="${{ secrets.POSTMAN_API_KEY }}"
          postman login --with-api-key $env:POSTMAN_API_KEY

      - name: Executar testes de API
        run: |
          postman collection run "32887155-ab8c059c-a445-4b93-bf41-06531bf45221" -e "32887155-62dbaf6a-6ee2-4a7f-82e6-fc5b3dd71dc5"


```
## Como Executar Localmente
Para executar os testes localmente, siga os passos abaixo:

1º ***instalar o newman** 
Certifique-se de ter o Node.js instalado e execute o seguinte comando para instalar o Newman globalmente:
```
npm install -g newman
```

2º ***Clone o repositório**
```
git clone https://github.com/seu-usuario/seu-repositorio.git
cd seu-repositorio
```
3º***Execute os testes:**

```
newman run "caminho/para/sua/colecao.json" -e "caminho/para/seu/ambiente.json"
```
