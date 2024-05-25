# Estrutura-de-dados 
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct {
    int codigo;
    char nome[50];
    int quantidade;
    float preco;
} Fruta;

typedef struct No {
    Fruta fruta;
    struct No* prox;
} No;

No* lista = NULL;

void cadastrarFruta() {
    Fruta novaFruta;
    printf("Digite o codigo da fruta: ");
    scanf("%d", &novaFruta.codigo);
    
    // Verifica se o código já existe
    No* atual = lista;
    while (atual != NULL) {
        if (atual->fruta.codigo == novaFruta.codigo) {
            printf("Codigo ja existente.\n");
            return;
        }
        atual = atual->prox;
    }

    printf("Digite o nome da fruta: ");
    scanf("%s", novaFruta.nome);
    printf("Digite a quantidade da fruta: ");
    scanf("%d", &novaFruta.quantidade);
    printf("Digite o preco da fruta: ");
    scanf("%f", &novaFruta.preco);

    No* novoNo = (No*)malloc(sizeof(No));
    novoNo->fruta = novaFruta;
    novoNo->prox = lista;
    lista = novoNo;

    printf("Fruta cadastrada com sucesso!\n");
}

void listarFrutas() {
    No* atual = lista;
    while (atual != NULL) {
        printf("Codigo: %d, Nome: %s, Quantidade: %d, Preco: %.2f\n", 
                atual->fruta.codigo, atual->fruta.nome, atual->fruta.quantidade, atual->fruta.preco);
        atual = atual->prox;
    }
}

void buscarFruta() {
    int codigo;
    printf("Digite o codigo da fruta a ser buscada: ");
    scanf("%d", &codigo);
    No* atual = lista;
    while (atual != NULL) {
        if (atual->fruta.codigo == codigo) {
            printf("Codigo: %d, Nome: %s, Quantidade: %d, Preco: %.2f\n", 
                    atual->fruta.codigo, atual->fruta.nome, atual->fruta.quantidade, atual->fruta.preco);
            return;
        }
        atual = atual->prox;
    }
    printf("Fruta nao encontrada.\n");
}

void alterarFruta() {
    int codigo;
    printf("Digite o codigo da fruta a ser alterada: ");
    scanf("%d", &codigo);
    No* atual = lista;
    while (atual != NULL) {
        if (atual->fruta.codigo == codigo) {
            printf("Digite o novo nome da fruta: ");
            scanf("%s", atual->fruta.nome);
            printf("Digite a nova quantidade da fruta: ");
            scanf("%d", &atual->fruta.quantidade);
            printf("Digite o novo preco da fruta: ");
            scanf("%f", &atual->fruta.preco);
            printf("Fruta alterada com sucesso!\n");
            return;
        }
        atual = atual->prox;
    }
    printf("Fruta nao encontrada.\n");
}

void excluirFruta() {
    int codigo;
    printf("Digite o codigo da fruta a ser excluida: ");
    scanf("%d", &codigo);
    No* atual = lista;
    No* anterior = NULL;
    while (atual != NULL) {
        if (atual->fruta.codigo == codigo) {
            if (anterior == NULL) {
                lista = atual->prox;
            } else {
                anterior->prox = atual->prox;
            }
            free(atual);
            printf("Fruta excluida com sucesso!\n");
            return;
        }
        anterior = atual;
        atual = atual->prox;
    }
    printf("Fruta nao encontrada.\n");
}

void venderFruta() {
    int codigo, quantidade;
    printf("Digite o codigo da fruta a ser vendida: ");
    scanf("%d", &codigo);
    printf("Digite a quantidade a ser vendida: ");
    scanf("%d", &quantidade);

    No* atual = lista;
    while (atual != NULL) {
        if (atual->fruta.codigo == codigo) {
            if (atual->fruta.quantidade >= quantidade) {
                atual->fruta.quantidade -= quantidade;
                
                // Registrar a venda
                FILE* file = fopen("vendas.txt", "a");
                if (file != NULL) {
                    fprintf(file, "Codigo: %d, Nome: %s, Quantidade vendida: %d, Preco unitario: %.2f, Total: %.2f\n",
                            atual->fruta.codigo, atual->fruta.nome, quantidade, atual->fruta.preco, quantidade * atual->fruta.preco);
                    fclose(file);
                }

                printf("Venda realizada com sucesso!\n");
            } else {
                printf("Quantidade em estoque insuficiente.\n");
            }
            return;
        }
        atual = atual->prox;
    }
    printf("Fruta nao encontrada.\n");
}

void sair() {
    // Libera a memória alocada
    No* atual = lista;
    while (atual != NULL) {
        No* temp = atual;
        atual = atual->prox;
        free(temp);
    }
    printf("Saindo do programa...\n");
}

int main() {
    int opcao;
    do {
        printf("\nMenu:\n");
        printf("1. Cadastrar fruta\n");
        printf("2. Listar frutas\n");
        printf("3. Buscar fruta\n");
        printf("4. Alterar fruta\n");
        printf("5. Excluir fruta\n");
        printf("6. Vender fruta\n");
        printf("7. Sair\n");
        printf("Escolha uma opcao: ");
        scanf("%d", &opcao);
        switch (opcao) {
            case 1:
                cadastrarFruta();
                break;
            case 2:
                listarFrutas();
                break;
            case 3:
                buscarFruta();
                break;
            case 4:
                alterarFruta();
                break;
            case 5:
                excluirFruta();
                break;
            case 6:
                venderFruta();
                break;
            case 7:
                sair();
                break;
            default:
                printf("Opcao invalida.\n");
                break;
        }
    } while (opcao != 7);
    return 0;
}
