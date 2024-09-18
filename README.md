#include <stdio.h>
#include <string.h>

#define MAX_PRODUTOS 50
#define MAX_CARRINHO 50

// Estrutura para armazenar os produtos
typedef struct {
    int codigo;
    char nome[30];
    float preco;
} Produto;

// Estrutura para armazenar os produtos no carrinho
typedef struct {
    Produto produto;
    int quantidade;
} Carrinho;

// Variáveis globais para os produtos e o carrinho
Produto produtos[MAX_PRODUTOS];
Carrinho carrinho[MAX_CARRINHO];
int numProdutos = 0;
int numCarrinho = 0;

// Função para exibir o menu
void menu() {
    int opcao;
    do {
        printf("\nMenu:\n");
        printf("1. Cadastrar Produto\n");
        printf("2. Listar Produtos\n");
        printf("3. Comprar Produto\n");
        printf("4. Visualizar Carrinho\n");
        printf("5. Fechar Pedido\n");
        printf("6. Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);

        switch(opcao) {
            case 1:
                cadastrarProduto();
                break;
            case 2:
                listarProdutos();
                break;
            case 3:
                comprarProduto();
                break;
            case 4:
                visualizarCarrinho();
                break;
            case 5:
                fecharPedido();
                break;
            case 6:
                printf("Saindo...\n");
                break;
            default:
                printf("Opcao invalida! Tente novamente.\n");
        }
    } while(opcao != 6);
}

// Função para cadastrar novos produtos
void cadastrarProduto() {
    if(numProdutos < MAX_PRODUTOS) {
        Produto novoProduto;
        printf("Codigo do produto: ");
        scanf("%d", &novoProduto.codigo);
        printf("Nome do produto: ");
        scanf("%s", novoProduto.nome);
        printf("Preco do produto: ");
        scanf("%f", &novoProduto.preco);

        produtos[numProdutos] = novoProduto;
        numProdutos++;

        printf("Produto cadastrado com sucesso!\n");
    } else {
        printf("Limite de produtos atingido!\n");
    }
}

// Função para listar todos os produtos cadastrados
void listarProdutos() {
    if(numProdutos > 0) {
        printf("\nLista de Produtos:\n");
        for(int i = 0; i < numProdutos; i++) {
            printf("Codigo: %d, Nome: %s, Preco: %.2f\n", produtos[i].codigo, produtos[i].nome, produtos[i].preco);
        }
    } else {
        printf("Nenhum produto cadastrado.\n");
    }
}

// Função para pegar o índice de um produto pelo código
int pegarProdutoPorCodigo(int codigo) {
    for(int i = 0; i < numProdutos; i++) {
        if(produtos[i].codigo == codigo) {
            return i;
        }
    }
    return -1;  // Retorna -1 se o produto não for encontrado
}

// Função para verificar se um produto já está no carrinho
int temNoCarrinho(int codigo) {
    for(int i = 0; i < numCarrinho; i++) {
        if(carrinho[i].produto.codigo == codigo) {
            return i;
        }
    }
    return -1;  // Retorna -1 se o produto não estiver no carrinho
}

// Função para adicionar um produto ao carrinho
void comprarProduto() {
    int codigo, quantidade;
    printf("Digite o codigo do produto que deseja comprar: ");
    scanf("%d", &codigo);

    int indiceProduto = pegarProdutoPorCodigo(codigo);
    if(indiceProduto == -1) {
        printf("Produto nao encontrado.\n");
        return;
    }

    printf("Digite a quantidade: ");
    scanf("%d", &quantidade);

    int indiceCarrinho = temNoCarrinho(codigo);
    if(indiceCarrinho != -1) {
        // Se o produto já estiver no carrinho, aumenta a quantidade
        carrinho[indiceCarrinho].quantidade += quantidade;
    } else {
        // Se não estiver, adiciona ao carrinho
        carrinho[numCarrinho].produto = produtos[indiceProduto];
        carrinho[numCarrinho].quantidade = quantidade;
        numCarrinho++;
    }

    printf("Produto adicionado ao carrinho.\n");
}

// Função para visualizar o carrinho
void visualizarCarrinho() {
    if(numCarrinho > 0) {
        printf("\nCarrinho:\n");
        for(int i = 0; i < numCarrinho; i++) {
            printf("Codigo: %d, Nome: %s, Quantidade: %d, Preco Unitario: %.2f\n", 
                    carrinho[i].produto.codigo, carrinho[i].produto.nome, 
                    carrinho[i].quantidade, carrinho[i].produto.preco);
        }
    } else {
        printf("Carrinho vazio.\n");
    }
}

// Função para fechar o pedido e esvaziar o carrinho
void fecharPedido() {
    if(numCarrinho > 0) {
        float total = 0;
        printf("\nFatura:\n");
        for(int i = 0; i < numCarrinho; i++) {
            float subtotal = carrinho[i].quantidade * carrinho[i].produto.preco;
            total += subtotal;
            printf("Produto: %s, Quantidade: %d, Subtotal: %.2f\n", 
                   carrinho[i].produto.nome, carrinho[i].quantidade, subtotal);
        }
        printf("Valor total: %.2f\n", total);
        numCarrinho = 0;  // Limpa o carrinho
        printf("Pedido fechado com sucesso!\n");
    } else {
        printf("Carrinho vazio. Nao ha pedido para fechar.\n");
    }
}

// Função principal
int main() {
    menu();
    return 0;
}
