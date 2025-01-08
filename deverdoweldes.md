<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sistema de Loja</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .container {
            background: #fff;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            width: 400px;
            padding: 20px;
        }
        h1 {
            text-align: center;
            color: #333;
        }
        button {
            display: block;
            width: 100%;
            margin: 10px 0;
            padding: 10px;
            font-size: 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            background: #007BFF;
            color: #fff;
            transition: background 0.3s ease;
        }
        button:hover {
            background: #0056b3;
        }
        .output {
            margin-top: 20px;
            padding: 10px;
            border: 1px solid #ddd;
            background: #fafafa;
            border-radius: 5px;
        }
        .output ul {
            list-style-type: none;
            padding: 0;
        }
        .output li {
            padding: 5px 0;
            border-bottom: 1px solid #ddd;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Sistema de Loja</h1>
        <button onclick="cadastrarProduto()">Cadastrar Produto</button>
        <button onclick="listarProdutos()">Listar Produtos</button>
        <button onclick="comprarProduto()">Comprar Produto</button>
        <button onclick="visualizarCarrinho()">Visualizar Carrinho</button>
        <button onclick="fecharPedido()">Fechar Pedido</button>
        <button onclick="sair()">Sair</button>
        <div id="output" class="output"></div>
    </div>

    <script>
        let produtos = [];
        let carrinho = [];

        function cadastrarProduto() {
            let codigo = prompt("Digite o código do produto:");
            let nome = prompt("Digite o nome do produto:");
            let preco = parseFloat(prompt("Digite o preço do produto:"));

            if (codigo && nome && preco) {
                produtos.push({ codigo, nome, preco });
                exibirMensagem("Produto cadastrado com sucesso!");
            } else {
                exibirMensagem("Erro ao cadastrar o produto. Verifique os dados.");
            }
        }

        function listarProdutos() {
            if (produtos.length > 0) {
                let lista = "<ul>";
                produtos.forEach(produto => {
                    lista += `<li>Código: ${produto.codigo}, Nome: ${produto.nome}, Preço: R$ ${produto.preco.toFixed(2)}</li>`;
                });
                lista += "</ul>";
                exibirMensagem(lista);
            } else {
                exibirMensagem("Nenhum produto cadastrado.");
            }
        }

        function comprarProduto() {
            let codigo = prompt("Digite o código do produto que deseja comprar:");
            let produto = produtos.find(p => p.codigo === codigo);

            if (produto) {
                let quantidade = parseInt(prompt("Digite a quantidade:"));
                if (quantidade > 0) {
                    let itemCarrinho = carrinho.find(c => c.codigo === codigo);
                    if (itemCarrinho) {
                        itemCarrinho.quantidade += quantidade;
                    } else {
                        carrinho.push({ ...produto, quantidade });
                    }
                    exibirMensagem("Produto adicionado ao carrinho.");
                } else {
                    exibirMensagem("Quantidade inválida.");
                }
            } else {
                exibirMensagem("Produto não encontrado.");
            }
        }

        function visualizarCarrinho() {
            if (carrinho.length > 0) {
                let lista = "<ul>";
                carrinho.forEach(item => {
                    lista += `<li>Código: ${item.codigo}, Nome: ${item.nome}, Quantidade: ${item.quantidade}, Preço Unitário: R$ ${item.preco.toFixed(2)}</li>`;
                });
                lista += "</ul>";
                exibirMensagem(lista);
            } else {
                exibirMensagem("Carrinho vazio.");
            }
        }

        function fecharPedido() {
            if (carrinho.length > 0) {
                let total = 0;
                let fatura = "<ul>";
                carrinho.forEach(item => {
                    let subtotal = item.quantidade * item.preco;
                    total += subtotal;
                    fatura += `<li>Produto: ${item.nome}, Quantidade: ${item.quantidade}, Subtotal: R$ ${subtotal.toFixed(2)}</li>`;
                });
                fatura += `</ul><strong>Total: R$ ${total.toFixed(2)}</strong>`;
                exibirMensagem(fatura);
                carrinho = [];
            } else {
                exibirMensagem("Carrinho vazio. Não há pedido para fechar.");
            }
        }

        function sair() {
            exibirMensagem("Saindo do sistema...");
        }

        function exibirMensagem(mensagem) {
            document.getElementById("output").innerHTML = mensagem;
        }
    </script>
</body>
</html>
