# Sistema PDV para Loja de Acessórios

Aplicação de frente de caixa para uma loja de acessórios, desenvolvida com Google Apps Script e Google Sheets. O sistema reúne vendas, controle de estoque, clientes, despesas e relatórios em uma interface web.

## Funcionalidades

- Login de funcionários com os perfis `Gerente` e `Caixa`.
- Abertura e fechamento de caixa, com conferência do dinheiro contado e totais por forma de pagamento.
- Vendas com busca de produtos por código de barras/SKU ou nome, controle de estoque, seleção de cliente e cálculo de troco.
- Emissão de comprovante de venda para impressoras térmicas de 58 mm.
- Cadastro de produtos, clientes e funcionários.
- Registro de despesas e relatórios financeiros por período, com vendas, despesas, total líquido e formas de pagamento.
- Interface responsiva em português brasileiro.

## Tecnologias

- Google Apps Script para lógica do servidor e publicação como Web App.
- Google Sheets como banco de dados.
- HTML, CSS e JavaScript no navegador.
- `google.script.run` para comunicação entre as páginas e o Apps Script.

Não há dependências externas de JavaScript configuradas neste projeto.

## Arquivos

| Arquivo | Finalidade |
| --- | --- |
| `Código.gs` | Rotas do Web App, acesso à planilha e regras de negócio. |
| `Login.html` | Autenticação de funcionários. |
| `Pdv.html` | Frente de caixa, vendas e fechamento de caixa. |
| `CadastroProdutos.html` | Cadastro de produtos. |
| `Cliente.html` | Cadastro e consulta de clientes. |
| `Funcionarios.html` | Cadastro de funcionários e níveis de acesso. |
| `Despesas.html` | Registro de despesas. |
| `Relatorios.html` | Relatórios financeiros e impressão. |
| `LogoData.html` | Imagem da marca incluída na tela de login. |
| `logo.png` | Arquivo da logo no repositório. |

## Configuração e implantação

1. Crie ou escolha uma planilha Google para os dados do sistema e copie o ID da planilha, que fica entre `/d/` e `/edit` no endereço.
2. No arquivo `Código.gs`, substitua o valor de `ID_PLANILHA` pelo ID da sua planilha.
3. Crie um projeto no [Google Apps Script](https://script.google.com/) e adicione o conteúdo de `Código.gs` como arquivo de script. Crie também os arquivos HTML com os nomes usados pelo projeto: `Login`, `Pdv`, `CadastroProdutos`, `Cliente`, `Despesas`, `Relatorios`, `Funcionarios` e `LogoData`; cole em cada um o conteúdo do arquivo correspondente deste repositório.
4. Na planilha, crie a aba `USUARIOS` com estes cabeçalhos na primeira linha:

   | `id_usuario` | `username` | `senha` | `nivel_acesso` |
   | --- | --- | --- | --- |

   Cadastre manualmente o primeiro usuário gerente em uma linha abaixo do cabeçalho. Use `Gerente` em `nivel_acesso`; depois, os demais funcionários poderão ser cadastrados pela tela do sistema.

5. Na aba `PRODUTOS`, mantenha os campos nas colunas A a G nesta ordem: `id_produto`, `codigo_barras`, `nome`, `categoria`, `preco_custo`, `preco_venda`, `estoque_atual`. O PDV usa a coluna H como lista de busca por nome; preencha-a com os nomes dos produtos (por exemplo, com uma fórmula que replique a coluna C).
6. No Apps Script, selecione **Implantar > Nova implantação > App da Web**. Execute como o proprietário do projeto e limite o acesso às pessoas ou à organização que devem usar o sistema. Autorize o acesso à planilha e implante.
7. Abra a URL do Web App publicada para entrar na tela de login.

As abas de clientes, vendas, itens de venda, despesas e controle de caixa são criadas ou preparadas pelo código conforme necessário. A aba `USUARIOS` e o primeiro usuário gerente precisam ser configurados antes do primeiro login.

## Uso

1. Entre com o usuário gerente inicial.
2. Cadastre produtos e funcionários. Para vender, abra o caixa informando o fundo inicial.
3. Adicione produtos ao carrinho, selecione a forma de pagamento e finalize a venda. Em pagamentos em dinheiro, informe o valor recebido.
4. Use as telas de clientes e despesas conforme necessário; os relatórios permitem selecionar um período e imprimir o resultado.
5. Feche o caixa informando o dinheiro contado fisicamente. O sistema apresenta a diferença em relação ao valor esperado.

## Segurança

**Antes de publicar este projeto ou usá-lo com dados reais, revise a segurança.** O código atual contém um ID de planilha em `Código.gs` e grava senhas em texto simples na aba `USUARIOS`. Além disso, algumas operações não validam a sessão e o perfil no servidor. Restrinja o acesso à implantação, não publique IDs ou dados reais e implemente armazenamento seguro de senhas e autorização no servidor antes de utilizar o sistema em produção.

## Observações

- O projeto depende de uma planilha Google acessível pela conta usada para executar o Web App.
- A impressão do comprovante abre uma janela do navegador; pop-ups precisam estar permitidos para o site.
- O fechamento considera o fundo inicial mais as vendas em dinheiro como valor esperado na gaveta. Pix e cartões são apresentados separadamente.
