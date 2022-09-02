# Vetor de Ponteiros (Array of pointers)

1. Escreva a função **insereOrdCrescentemente**, que recebe um vetor de ponteiros para Empregado, ORDENADO CRESCENTEMENTE por salário, o número de empregados e os dados de um novo empregado - insere um novo empregado com esses dados, preservando a ordenação do vetor. Se a nova inclusão for feita sem problemas, a função retorna 1. Caso já exista empregado com essa identificação, o salario deve ser atualizado com o valor de salario recebido, e a função retorna -1. Caso não seja possível alocar espaço em memória para o novo empregado, a função retorna 0.

**Solução**

>Lembrando que um vetor de ponteiro é diferente de um vetor de estruturas (vide ilustração abaixo).

<img width="855" alt="image" src="https://user-images.githubusercontent.com/1479925/180856383-e1ca8d23-6c5f-46cb-9c84-369861388e13.png">

<img width="838" alt="image" src="https://user-images.githubusercontent.com/1479925/180856443-4c58bf96-a2e3-44da-9a2d-0df782c902a6.png">


```c
int insereOrdCrescentemente(Empregado **vet, int n, int id, char *nome, float salario)
{
  int i, j, posnovo = 0;
  Empregado *novo;
  Empregado **vet2[n + 1];
  for (i = 0; i<n; i++)
  {
    if (vet[i]->identificação == id)
    {
      vet[i]->salario = salario;
      return -1;
    }
    
    else
    {
      while (salario < vet[i]->salario)
        posnovo++;
    }
  }
  
  novo = (Empregado*)malloc(sizeof(Empregado));

  if (novo == NULL)
    return 0;

  novo->identificação = id;
  strcpy(novo->nome, nome);
  novo->salario = salario;

  j = 0;
  
  for (i = 0; i < posnovo; i++)
  {
    vet2[j] = vet[i];
    j++; 
  }
  
  j++;
  
  for (i = posnovo; i < n; i++)
  {
    vet2[j] = vet[i];
    j++;
  }
     
  vet2[posnovo] = novo;
  
  return 1;
}    
```

# Lista Encadeada (Linked List)
2. Considere a existência de duas listas simplesmente encadeadas, ORDENADAS crescentemente por código, em que cada nó encadeado é representado pelo tipo NoLista abaixo.

```c
typedef struct noLista NoLista;
struct noLista
{     int codigo;
      char nome[51];
      NoLista *prox;
};
```

Escreva a função **testaContida** que recebe duas listas simplesmente encadeadas (ou seja, os endereços dos primeiros nós das duas listas) e ordenadas crescentemente por "codigo" e retorna 1 se a primeira lista está totalmente contida na segunda lista. Retorna 0 em caso contrário. NÃO USE FUNÇÕES AUXILIARES (exceto as disponibilizadas pelas bibliotecas padrão de C) e LEVE EM CONSIDERAÇÃO A ORDENAÇÃO DAS LISTAS para tornar sua função mais eficiente.

Por exemplo, se os elementos da lista 1 fossem:
[222,”leo”], [333, “rui”], [777,”rosa”], [888,”lia”]

E os elementos da lista 2 fossem:
[111, “edu”], [222,”leo”], [333, “rui”], [446, “cris”], [558, “lara”], [777,”rosa”] , [888,”lia”], [999,”bia”]
A função retorna 1.

Por exemplo, se os elementos da lista 1 fossem:
[222,”leo”], [333, “rui”], [554, “vera”], [777,”rosa”], [888,”lia”]

E os elementos da lista 2 fossem:
[111, “edu”], [222,”leo”], [333, “rui”], [446, “cris”], [666,”cadu”], [777,”rosa”] , [888,”lia”], [999,”bia”]

A função retorna 0.

Considere que nas listas recebidas, nem todos os valores da lista 2 estão na lista 1 e vice-versa. Considere também que não há repetições de "codigo" numa mesma lista e que um determinado "codigo" corresponderá sempre a um mesmo nome. Observação: não é para percorrer as listas várias vezes.

**Solução**
```c
int testaContida(NoLista *lst1, NoLista *lst2)
{
        NoLista *p1, *p2;
        p1 = lst1;
        p2 = lst2;
        while (p1 != NULL && p2 != NULL)
        {
               while (p2->código < p1->código)
                      p2 = p2->prox;
               if (p2->código == p1->código)
               {
                      p1 = p1->prox;
                      p2 = p2->prox;
}
               else
                      return 0;
        }
        if (p1 == NULL)
               return 1;
        return 0;
}
```

# Pilha (Stack)
3. Escreva uma implementação do TAD Pilha de caracteres usando um vetor de caracteres, alocado dinamicamente com n (recebido como parâmetro e armazenado na estrutura da pilha) elementos e uma variável inteira topo, que guarda a posição do elemento mais recentemente inserido na pilha (o último dentre os que estão na pilha). O nome do arquivo deve ser Pilha.c. Faça todos os includes necessários, defina a struct para a pilha conforme a descrição anterior e implemente as funções publicadas no arquivo de interface Pilha.h.

```c
/* TAD Pilha de caracteres */

/* interface: arquivo Pilha.h */
typedef struct pilha Pilha;

/* função que cria, inicializa e retorna o endereço de (o ponteiro para) uma nova pilha de caracteres, Esta pilha não pode conter mais de n elementos */
Pilha *CriaPilha(int n);

/* função que recebe um ponteiro para (o endereço de) uma pilha de inteiros e um novo elemento (no caso, um inteiro), fazendo a inserção desse elemento. Caso a inclusão não possa ser realizada esta função finaliza a execução. */
void Empilha(Pilha *pi, char ninf);

/* função que recebe um ponteiro para (o endereço de ) uma pilha de inteiros e faz a retirada de um elemento da pilha(no caso, um caracter), retornando-o. Caso a exclusão não possa ser realizada esta função finaliza a execução. */
char Desempilha(Pilha *pi);

/* função que recebe um ponteiro para (o endereço de ) uma pilha de inteiros e retorna 1, se a pilha é vazia, ou 0, caso contrário */
int PilhaVazia(Pilha *pi);

/* função que recebe um ponteiro para (o endereço de ) uma pilha de inteiros e libera o espaço ocupado pela pilha */
void LiberaPilha(Pilha *pi);

/* função que recebe um ponteiro para (o endereço de ) uma pilha de inteiros e retorna 1, se a pilha está cheia, ou 0, caso contrário */
int PilhaCheia(Pilha *pi);
```

**Solução**

```c
#include "Pilha.h"
#include <stdio.h>
#include <stdlib.h>

struct pilha
  {
    int max;
    char *vet;
    int topo;
  };
  
Pilha *CriaPilha(int n)
{
  char *v;
  Pilha* p;
  p = (Pilha*)malloc(sizeof(Pilha));
  
  p->topo = 0;
  p->max = n;
  
  v = (char*)malloc(p->max * sizeof(char));
  p->vet = v;

  return p;
}

void Empilha(Pilha *pi, char ninf)
{
  if (PilhaCheia(pi) == 1)
  {
    printf("Não foi possível realizar a inclusão desejada.\n");
    exit(1);
  }

  pi->vet[pi->topo] = ninf;
  pi->topo++;
}

char Desempilha(Pilha *pi)
{
  char c;
  if (PilhaVazia(pi) == 1)
  {
    printf("A pilha está vazia!\n");
    exit(1);
  }

  c = pi->vet[pi->topo];
  pi->topo--;
  
  return c; 
}

int PilhaVazia(Pilha *pi)
{
  if (pi->topo == -1) /* Pilha é vazia */
    return 1;
  else
    return 0;
}

void LiberaPilha(Pilha *pi)
{
  free(pi->vet);
  free(pi);
}

int PilhaCheia(Pilha *pi)
{
  if (pi->topo == pi->max)
    return 1;
  else
    return 0;
}

```

4. Considere a interface do TAD Pilha (de caracteres) definida acima 

Escreva uma implementação do TAD Pilha de caracteres usando uma estrutura encadeada com quantidade máxima de elementos determinada em sua criação e que aponta para o nó topo. O nome do arquivo deve ser Pilha.c. Faça todos os includes necessários, defina a struct para a pilha conforme a descrição anterior e implemente as funções publicadas no arquivo de interface Pilha.h.

Substitua a implementação do TAD no exemplo anterior e execute novamente o programa. Observe que NENHUMA linha do código é modificada!!!!!

**Solução**

```c
/* Pilha.c */
#include "Pilha.h"
#include <stdio.h>
#include <stdlib.h>

struct elemento
{
	char c;
	struct elemento *prox;
};

struct pilha
{
	Elemento *prim;
	int max; 
};

Pilha *CriaPilha(int n)
{
	Pilha* p = (Pilha*)malloc(sizeof(Pilha));
	
	p->prim = NULL;
	p->max = n;
	
	return p;
}

void Empilha(Pilha *pi, char ninf)
{
	if (PilhaVazia(pi) == 1)
	{
		printf("A pilha está vazia!\n");
		exit(1); 
	}
	
	Elemento* n = (Elemento*)malloc(sizeof(Elemento));
	
	n->c = ninf;
	n->prox = p->prim;
	p->prim = n;
}


char Desempilha(Pilha *pi)
{
	Elemento* t;
	char c;
	if (PilhaVazia(pi) == 1)
	{
		printf("A pilha está vazia!\n");
		exit(1); 
	}
	
	t = p->prim;
	c = t->c;
	p->prim = t->prox;
	free(t);
	
	return c; 
}

int PilhaVazia(Pilha *pi)
{
	if (p->prim == NULL)
		return 1;
	return 0; 
}

int PilhaCheia(Pilha *pi)
{
	int i = 0;
	Elemento *p = prim;
	while (p != NULL)
	{
		p = p->prox;
		i++; 
	}
	if (pi->max = i)
		return 1;
	else
       return 0;
}

void LiberaPilha(Pilha *pi)
{
	Elemento *t, *q = p->prim;
	while (q != NULL)
	{
		t = q->prox;
		free(q);
		q = t;
	}
	free(p); 
}
```

5. Uma mensagem foi armazenada em uma cadeia de caracteres de forma codificada. A codificação foi feita invertendo-se as subsequências de não vogais.

Assim, a mensagem que originalmente era no exemplo 1:
“PROGRAMA DA PROVA” , foi armazenada da seguinte maneira: “RPORGAMAD ARP OVA”

E a mensagem que originalmente era no exemplo 2:
“ATRASOS PREVISTOS NOS PROJETOS”, foi armazenada da seguinte maneira: “ARTASORP SEVITSON SORP SOJETOS”.

Usando obrigatoriamente uma pilha como estrutura auxiliar, escreva a função decifra , que recebe (o ponteiro para) uma cadeia contendo uma mensagem codificada e exibe a mensagem original. (É só para exibir.)

Assuma que todo caracter correspondente a uma letra do alfabeto está em maiúsculo. (Observe que branco/espaço é uma "não-vogal") .

Para isso deve ser utilizada uma pilha de caracteres. Considere a existência de um TAD Pilha de caracteres
(Pilha de char), que tem o seguinte arquivo de interface:

```c
/* arquivo PilhaDeChar.h */

/* tipo e funções básicas */
typedef struct pilhaChar PilhaChar;

/* função que cria, inicializa e retorna (o endereço de) uma pilha de caracteres */
PilhaChar *pilha_cria(void);

/* função que recebe (o endereço de) uma pilha de caracteres e um caracter, inserindo-o na pilha */
void pilha_push (PilhaChar *p, char novo);

/* função que recebe (o endereço de) uma pilha de caracteres e faz uma retirada da pilha, retornando o caracter do topo da pilha, ou seja o mais recentemente inserido */
char pilha_pop(PilhaChar *p);

/* função que recebe (o endereço de) uma pilha de caracteres e retorna 1, se a pilha está vazia, ou 0, em caso contrario */
int pilha_vazia(PilhaChar *p);
/* função que recebe (o endereço de) uma pilha de caracteres e libera o espaço ocupado pela pilha */
void pilha_libera(PilhaChar *p);
```

Obs: Não esqueça de liberar o espaço ocupado pela pilha quando ela não for mais necessária.

**Solução**

```c
int eh_vogal(char letra){
	return (letra == 'A' || letra == 'E' || letra == 'I' || letra == 'O' || letra == 'U');
}
void decifra(char *msgcod) {
	int i;
	int len = strlen(msgcod);
    PilhaChar *aux = pilha_cria();
    for(i=0; i<len; i++){
		if(eh_vogal(msgcod[i])){
			while(!pilha_vazia(aux)){
				printf("%c", pilha_pop(aux));
			}
				printf("%c", msgcod[i]);
        } else {
                pilha_push(aux, msgcod[i]);
		} 
	}
    
	while(!pilha_vazia(aux)){
        printf("%c", pilha_pop(aux));
	}
    
	pilha_libera(aux);
}
```
