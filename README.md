# Virtual Memory Simulator - Page Replacement Algorithms

## Autor:
** Victor Hugo Faria - 5962 ** 
[Perfil no Github](https://github.com/victorh1590)

 ## Repositório:
 [Github](https://github.com/victorh1590/VirtualMemoryAlgorithms)

## Descrição:

Trabalho proposto pelo professor Rodrigo na disciplina de Sistemas Operacionais (SIN351) na Universidade Federal de Viçosa.

O trabalho consistiu na implementação de quatro algoritmos de substituição de páginas à partir de um simulador de memória virtual.

## Algoritmos:
- FIFO (_First-in First-out_),
- Second Chance,
- NRU (_Not Recently Used_),
- Aging (NFU - _Not Frequently Used_)

## Compilando o código:
> Pré-requisito: Um compilador C (recomendado: gcc).

Abra um shell no diretório raíz e digite o comando:
```shell
gcc -Wall vmm.c -o vmm
```


## Instruções e detalhes de implementação:
### FIFO
O algoritmo FIFO (_First-in First-out_) sempre retorna a página mapeada a mais tempo. O algoritmo funciona como uma fila simples, onde o primeiro a chegar é sempre o primeiro a sair.

A implementação consiste num laço while que percorre a lista de páginas procurando por uma página que mapeia a moldura que foi acessada a mais tempo (identificada pela variável fifo_frm).

** Execução **
Abra um shell no diretório raíz e digite o comando:
```shell
./vmm fifo 10 < anomaly.dat
```

### Second Chance
O algoritmo Second Chance funciona de maneira similar ao FIFO, porém antes de remover a página mais antiga ele verifica se aquela página foi referenciada recentemente, através de um bit R. Caso tenha sido referenciada, a página é movida para o fim da fila e a próxima página é então testada.

A implementação do algoritmo foi feita através da implementação de uma fila estática à partir de uma array, onde uma variável índice é utilizada para marcar o início da fila. Sempre que ocorre uma _page fault_ e o algoritmo é chamado, a fila estática é montada numa array, utilizando-se a variável q_start (que é igualada a fifo_frm, moldura acessada a mais tempo) para garantir que a ordem das páginas na fila está correta. O elemento de índice 0 representa o elemento a mais tempo na memória e os elementos estão ordenados em ordem de chegada. Se a página do início da fila foi referenciada recentemente, o índice que aponta para o início da fila é incrementado. Este teste é repetido para cada elemento da fila até que um elemento com o bit R = 0 é encontrado. Se o índice chegar no fim da array que representa a fila, ele retorna para o início e o elemento de índice 0 é removido (comportamento FIFO). Isso significa que todas as páginas da fila foram referenciadas recentemente e portanto a primeira a chegar será removida.

** Execução **
Abra um shell no diretório raíz e digite o comando:
```shell
./vmm second_chance 10 < anomaly.dat
```

### NRU (Not Recently Used)
O algoritmo NRU substitui a página com base na atividade recente da página. Esta atividade é verificada testando-se os bits R e M, referenciada e modificada respectivamente. Os critérios estão descritos a seguir: 
- Se a página não foi referenciada ou modificada recentemente, ela é substituída. 
-  Caso nenhuma corresponda ao critério anterior, então o próximo critério é substituir uma página que foi modificada, mas não referenciada recentemente. 
- Caso nenhuma corresponda ao critério anterior, então o próximo critério é substituir a uma página que foi referenciada, mas não modificada recentemente.
- Caso nenhuma corresponda ao critério anterior, substitui-se arbitrariamente qualquer uma das páginas mapeadas em memória.

A implementação consiste em quatro laços while em sequência, cada laço corresponde a um critério, na sequência descrita anteriormente. O laço percorre a lista de páginas procurando por uma página que se encaixe no seu critério, que é então substituída. Se não for encontrada, segue o próximo laço.

** Execução **
Abra um shell no diretório raíz e digite o comando:
```shell
./vmm nru 10 < anomaly.dat
```

### Aging (NFU)
O algoritmo Aging (NFU) substitui a página com base num contador. Este contador inicia em 0 e é incrementando a cada interrupção do relógio. O incremento corresponde ao estado do bit R (de referência) atual para cada página. A ideia é que as páginas que tenham o bit de referência 1 são mais utilizadas, e portanto terão valores de contador altos em relação a páginas não referenciadas. A página que possuir o menor valor de contador é substituída a cada iteração do algoritmo.

A implementação pode ser dividida em duas partes: incremento e substituição, ambas implementadas por meio de um laço de repetição. O incremento é feito quando a variável _clock_ é testada e tem valor _true_, isso significa que a última instrução gerou um ciclo de _clock_ e portanto o contador deve ser atualizado. A lista de páginas é percorrida e o contador é atualizado para cada página. Em seguida vem a substituição. O valor do contador de cada página é testado e a página mapeada que tiver o menor valor no contador é substituída. 
Se todas as páginas tiverem o mesmo valor de contador, uma página é substituída arbitrariamente.

** Execução **
Abra um shell no diretório raíz e digite o comando:
```shell
./vmm aging 10 < anomaly.dat
```

## Comparação de Resultados e Discussão.


| Execução 	| Random 	| FIFO 	| Second Chance 	| NRU 	| Aging 	|
|:---------:|:---------:|:-----:|:-----------------:|:-----:|:---------:|
|     1    	|        	|      	|               	|     	|       	|
|     2    	|        	|      	|               	|     	|       	|
|     3    	|        	|      	|               	|     	|       	|
|     4    	|        	|      	|               	|     	|       	|
|     5    	|        	|      	|               	|     	|       	|
|     6    	|        	|      	|               	|     	|       	|
|     7    	|        	|      	|               	|     	|       	|
|     8    	|        	|      	|               	|     	|       	|
|     9    	|        	|      	|               	|     	|       	|
|    10    	|        	|      	|               	|     	|       	|

