# Portfólio 2 - Inteligência Artificial

## **Agente de Soluções de Problemas**

Um agente de solução de problemas é uma entidade autônoma projetada para atuar em um ambiente com o objetivo de atingir metas definidas. Ele segue um processo sistemático para identificar, analisar e resolver problemas. Esses agentes são fundamentais na Inteligência Artificial (IA), sendo aplicados em áreas como tomada de decisão, planejamento e automação.

### **1. Formulação de Objetivos**

A formulação de objetivos é a etapa inicial onde as metas são claramente definidas. Objetivos bem delineados orientam todas as fases seguintes, garantindo que os esforços do agente sejam direcionados eficientemente. Segundo **Russell e Norvig (2010)**, a qualidade dessa formulação afeta diretamente a eficiência do processo de solução de problemas.

### **2. Formulação do Problema**

Na formulação do problema, o agente descreve os elementos necessários para resolvê-lo, incluindo:

- **Estado inicial**: Situação atual do sistema.
- **Estados objetivos**: Condições que representam o sucesso.
- **Ações possíveis**: Conjunto de operações ou movimentos que o agente pode executar.
- **Função de custo**: Avalia o esforço ou custo associado a cada ação.

Por exemplo, em um jogo de xadrez, o estado inicial é a disposição das peças no tabuleiro, os estados objetivos incluem situações como xeque-mate, e as ações possíveis correspondem aos movimentos permitidos para cada peça.

### **3. Busca**

A busca é a etapa onde o agente explora o espaço de soluções em busca do caminho mais eficiente para atingir o objetivo. Isso é feito de forma simulada, antes da execução real das ações.

**Classificação de algoritmos de busca:**
- **Busca cega**: Explora o espaço de solução sem informação adicional sobre o objetivo. Exemplos incluem busca em largura e profundidade.
- **Busca informada**: Utiliza heurísticas para guiar a busca de forma mais eficiente. Algoritmos como A* e Best-First Search são exemplos dessa abordagem.

Conforme **Nilsson (1982)**, a escolha do algoritmo é essencial para o desempenho do agente, especialmente em espaços de solução muito grandes.

### **4. Execução**

Uma vez identificado o melhor caminho, o agente passa para a etapa de execução. Durante essa fase, é essencial que o agente monitore e se adapte às condições do ambiente, especialmente em cenários dinâmicos ou imprevisíveis. **Ghallab, Nau e Traverso (2004)** destacam que a capacidade de reação e adaptação é um dos fatores mais críticos para a eficácia de agentes em ambientes reais.

---

## **Problemas de Malha Aberta e Malha Fechada**

A abordagem do agente em relação ao controle do ambiente pode ser categorizada em dois tipos principais: **malha aberta** e **malha fechada**. Essa distinção é essencial para determinar a capacidade do agente de lidar com diferentes tipos de problemas e ambientes.

### **1. Sistemas de Malha Aberta**

Em sistemas de malha aberta, as ações do agente não dependem de feedback do ambiente. Isso significa que o agente executa seu plano sem verificar se as condições mudaram ou se o objetivo foi alcançado. Essa abordagem funciona bem em ambientes:
- **Determinísticos**: Onde as condições não mudam de forma inesperada.
- **Previsíveis**: Onde é possível planejar todas as ações antecipadamente.

Por exemplo, em uma esteira de montagem industrial, cada etapa é executada de forma fixa, sem necessidade de adaptação.

### **2. Sistemas de Malha Fechada**

Em contrapartida, sistemas de malha fechada utilizam feedback constante do ambiente para ajustar as ações do agente. Esse tipo de controle é essencial em cenários:
- **Dinâmicos**: Onde o ambiente pode mudar durante a execução do plano.
- **Não determinísticos**: Onde os resultados das ações podem ser incertos.

Por exemplo, em um robô que limpa uma sala, o sistema de malha fechada permite que ele ajuste sua rota caso encontre obstáculos inesperados.

### **Comparativo**

| Característica              | Malha Aberta                    | Malha Fechada                  |
|-----------------------------|---------------------------------|---------------------------------|
| Dependência de feedback     | Não                            | Sim                             |
| Ambiente ideal              | Determinístico e previsível    | Dinâmico e não determinístico |
| Flexibilidade               | Baixa                          | Alta                            |
| Exemplo                     | Esteira de montagem            | Robô autônomo de limpeza      |

Compreender essas diferenças é crucial para projetar agentes adequados a cada tipo de ambiente, garantindo o melhor desempenho possível.

---

## Algoritmos de Busca 

Os algoritmos de busca são fundamentais para resolver problemas, permitindo explorar o espaço de estados e encontrar soluções para atingir os objetivos de um agente.

### **1. Busca Cega (Não Informada)**

A Busca Cega explora o espaço de estados de forma sistemática e exaustiva, sem considerar informações adicionais sobre o objetivo. Esses algoritmos são fáceis de implementar, mas podem ser ineficientes em espaços de busca muito grandes. Os principais algoritmos de busca cega são:

#### **1.1 Busca em Largura (BFS)**

A **Busca em Largura** explora todos os nós de um nível antes de passar para o próximo. É garantido que ela encontra a solução mais curta (ótima), desde que os custos entre os passos sejam iguais. No entanto, exige bastante memória para armazenar os nós visitados.

```python
from collections import deque

def bfs(graph, start, goal):
    visited = set()  # Nós já visitados
    queue = deque([(start, [start])])  # Fila de (nó atual, caminho até ele)
    
    while queue:
        node, path = queue.popleft()  # Remove o primeiro elemento da fila
        
        if node in visited:
            continue
        visited.add(node)
        
        if node == goal:
            return path  # Caminho encontrado
        
        for neighbor in graph.get(node, []):  # Adiciona vizinhos à fila
            queue.append((neighbor, path + [neighbor]))
    
    return None  # Nenhum caminho encontrado
```

---

#### **1.2 Busca em Profundidade (DFS)**

A **Busca em Profundidade** explora os caminhos até o fim antes de retornar e tentar alternativas. Usa menos memória do que BFS, mas não garante encontrar o caminho mais curto.

```python
def dfs(graph, start, goal, path=None, visited=None):
    if path is None:
        path = [start]  # Caminho atual
    if visited is None:
        visited = set()  # Nós visitados
    
    visited.add(start)
    
    if start == goal:
        return path  # Caminho encontrado
    
    for neighbor in graph.get(start, []):
        if neighbor not in visited:
            new_path = dfs(graph, neighbor, goal, path + [neighbor], visited)
            if new_path:
                return new_path
    
    return None  # Nenhum caminho encontrado
```

---

### **2. Busca Informada (Heurística)**

A Busca Informada utiliza heurísticas, ou seja, informações adicionais sobre o problema, para guiar a exploração do espaço de estados. Isso permite priorizar os caminhos mais promissores, reduzindo o custo de processamento. Os principais algoritmos incluem **Best-First Search** (explorado em sala) e **A***.

#### **2.1 Best-First Search**

O algoritmo **Best-First Search** utiliza uma função heurística \( h(n) \) para determinar a prioridade de exploração dos nós. Ele não considera o custo acumulado do caminho e, portanto, pode não encontrar a solução ótima.

```python
import heapq

def best_first_search(graph, heuristics, start, goal):
    visited = set()
    queue = [(heuristics[start], start, [start])]  # Fila de prioridade (h(n), nó, caminho)
    
    while queue:
        _, node, path = heapq.heappop(queue)  # Remove o nó com menor h(n)
        
        if node in visited:
            continue
        visited.add(node)
        
        if node == goal:
            return path  # Caminho encontrado
        
        for neighbor in graph.get(node, []):
            if neighbor not in visited:
                heapq.heappush(queue, (heuristics[neighbor], neighbor, path + [neighbor]))
    
    return None  # Nenhum caminho encontrado
```

---

#### **2.2 Algoritmo A***

O **A*** combina o custo acumulado \( g(n) \) com a heurística \( h(n) \), buscando minimizar o custo total estimado \( f(n) = g(n) + h(n) \). Esse algoritmo garante a solução ótima se \( h(n) \) for admissível (não superestimar o custo real).

```python
def a_star(graph, costs, heuristics, start, goal):
    visited = set()
    queue = [(heuristics[start], 0, start, [start])]  # Fila de prioridade (f(n), g(n), nó, caminho)
    
    while queue:
        _, cost, node, path = heapq.heappop(queue)  # Remove o nó com menor f(n)
        
        if node in visited:
            continue
        visited.add(node)
        
        if node == goal:
            return path  # Caminho encontrado
        
        for neighbor, weight in graph.get(node, []):  # Adiciona vizinhos
            if neighbor not in visited:
                total_cost = cost + weight
                heapq.heappush(queue, (total_cost + heuristics[neighbor], total_cost, neighbor, path + [neighbor]))
    
    return None  # Nenhum caminho encontrado
```

---

### Funções Heurísticas

As **funções heurísticas** são um componente essencial dos algoritmos de busca informada, como o **Best-First Search** e o **A***. Elas fornecem uma maneira de estimar quão perto um estado está do objetivo, ajudando os algoritmos a fazer escolhas mais inteligentes sobre qual caminho seguir. As heurísticas desempenham um papel crucial na eficiência e eficácia desses algoritmos, pois permitem que a busca seja orientada para as soluções mais promissoras, ao invés de explorar todos os caminhos de forma cega.


### Definição de Heurística

Uma **heurística** é uma função que atribui um valor a cada nó de um espaço de busca, indicando a "aproximidade" desse nó em relação ao objetivo. O valor retornado pela função heurística deve ser uma estimativa do custo para alcançar o objetivo a partir do estado avaliado.

**Exemplos de funções heurísticas:**
- **Distância Euclidiana ou Manhattan:** Em problemas de navegação, a heurística pode ser a distância direta entre o nó atual e o objetivo. A escolha depende do tipo de movimento permitido.
- **Número de blocos fora do lugar:** Em problemas como o quebra-cabeça 8 (ou 15), a heurística pode ser o número de peças fora do lugar, ou seja, quantas peças precisam ser movidas para resolver o quebra-cabeça.
- **Custo de ação restante:** Em problemas de otimização, pode-se utilizar o custo estimado das ações restantes para alcançar o objetivo.

A heurística precisa ser **admissível** e, em muitos casos, **consistente** (também chamada de **monótona**) para garantir que o algoritmo A* seja ótimo:

- **Admissibilidade:** Uma heurística é admissível se nunca superestima o custo real de alcançar o objetivo. Isso significa que, para qualquer nó, o valor estimado pela heurística deve ser menor ou igual ao custo real mínimo até o objetivo.
- **Consistência (ou Monotonicidade):** Uma heurística é consistente se, para cada nó e cada sucessor do nó, a diferença entre os valores heurísticos não for maior do que o custo real de mover entre eles.


### Exemplos de Funções Heurísticas

1. **Distância Euclidiana (para navegação em um espaço 2D):**  
   Usada em problemas de navegação em um espaço contínuo, considerando a distância direta entre o nó atual e o objetivo.

2. **Distância Manhattan (para navegação em um espaço 2D com movimento em grade):**  
   Aplicável em grades, quando o movimento é restrito a direções horizontais e verticais, acumulando as distâncias percorridas em cada eixo.

3. **Número de blocos fora do lugar (para o quebra-cabeça 8 ou 15):**  
   Considera o número de peças que estão fora das suas posições corretas. É uma estimativa simples e eficaz para medir o progresso em direção à solução.

4. **Custo de ações restantes (em problemas de otimização):**  
   Em problemas como roteamento de veículos, essa heurística pode estimar o custo das ações restantes necessárias para alcançar o objetivo.


#### As funções heurísticas são ferramentas poderosas para guiar a busca em problemas complexos. Elas permitem que os algoritmos de busca informada, como o **A***, escolham de forma inteligente os caminhos mais promissores, melhorando a eficiência e a precisão na busca pela solução ótima. A escolha da heurística adequada é crucial para o desempenho do algoritmo, e deve ser cuidadosamente selecionada com base nas características do problema em questão.
---

## Busca em Ambientes Complexos

Em alguns problemas de Inteligência Artificial, a busca no espaço de estados não se concentra na trajetória ou caminho percorrido até o objetivo, mas sim no próprio **estado final** que atende às condições de sucesso. Isso é particularmente importante em problemas onde o objetivo final é mais relevante do que o caminho para alcançá-lo.

### Contextualizando a Busca em Ambientes Complexos

Nos ambientes complexos, a busca visa **encontrar uma solução ou um estado final que satisfaça as condições do problema**, mas sem necessariamente se preocupar com o caminho para chegar até esse estado. Em muitos casos, o número de estados possíveis é muito grande e a solução pode envolver uma otimização de vários fatores simultaneamente.

Exemplos de problemas que se encaixam nesse tipo de abordagem incluem:

- **Projeto de Circuito Integrado:** Encontrar uma configuração eficiente de um circuito sem necessariamente se preocupar com a ordem exata das etapas.
- **Layout de Chão de Fábrica:** Determinar a disposição ideal das máquinas e departamentos, visando minimizar o custo de movimentação de materiais e aumentar a eficiência.
- **Programação Automática:** Alocar tarefas de maneira eficiente a um conjunto de recursos, como em escalonamento de processos ou distribuição de tarefas em um cluster de servidores.
- **Otimização de Rede de Telecomunicações:** Maximizar a largura de banda ou minimizar a latência de uma rede, sem se preocupar diretamente com o caminho individual dos pacotes de dados.
- **Planejamento de Safra:** Definir as melhores safras a serem plantadas em uma área, levando em consideração fatores como clima, solo e demanda de mercado.
- **Gerenciamento de Portfólio:** Determinar a melhor alocação de ativos financeiros (ações, títulos, etc.) com o objetivo de maximizar o retorno esperado ou minimizar o risco.

Em problemas dessa natureza, a busca é muitas vezes associada a técnicas de **otimização** e **aproximação**, e os métodos de busca podem ser adaptados para explorar rapidamente grandes espaços de soluções possíveis. A busca não visa um caminho, mas sim o estado final mais adequado ou otimizado.

### Técnicas de Busca em Ambientes Complexos

Aqui estão algumas abordagens usadas na busca em ambientes complexos:

1. **Algoritmos de Busca Local (Local Search)**
2. **Algoritmos Genéticos (Genetic Algorithms)**
3. **Simulated Annealing**
4. **Algoritmos de Programação Linear**

### Exemplo de Simulated Annealing

**Simulated Annealing** (SA) é um algoritmo de otimização inspirado no processo físico de resfriamento de metais. A ideia é explorar soluções de forma aleatória, aceitando soluções piores de forma controlada, para escapar de mínimos locais e alcançar uma solução globalmente ótima.

#### Exemplo: **O Problema de Minimização de Função**

```python
import math
import random

def objective_function(x):
    return x**2 + 10*math.sin(x)  # Função objetivo: minimizar f(x)

def simulated_annealing(start, temperature, cooling_rate):
    current_solution = start
    current_cost = objective_function(current_solution)
    
    while temperature > 1:
        new_solution = current_solution + random.uniform(-1, 1)
        new_cost = objective_function(new_solution)
        
        # Aceita a nova solução com base na temperatura
        if new_cost < current_cost or random.random() < math.exp((current_cost - new_cost) / temperature):
            current_solution = new_solution
            current_cost = new_cost
        
        temperature *= cooling_rate  # Diminui a temperatura
    
    return current_solution, current_cost

# Exemplo de uso
start = 10
temperature = 1000
cooling_rate = 0.995
solution, cost = simulated_annealing(start, temperature, cooling_rate)
print(f"Solução final: {solution

}, Custo: {cost}")
```

Neste exemplo, é usado **Simulated Annealing** para minimizar uma função matemática. O algoritmo explora novas soluções e aceita soluções piores com uma probabilidade que diminui com o tempo (temperatura), ajudando a escapar de mínimos locais.

---

## Algoritmos Genéticos (Algoritmos Evolutivos)

 Os Algoritmos Genéticos são técnicas de otimização inspiradas na evolução natural. Eles trabalham com uma população de indivíduos, onde cada indivíduo representa uma solução possível. Os indivíduos mais aptos, avaliados por uma função de fitness, reproduzem-se através de recombinação e mutação para formar a próxima geração. Esse processo se repete até alcançar uma solução satisfatória.

```python
import random

def fitness(individual):
    return sum(individual)

def mutate(individual):
    idx = random.randint(0, len(individual)-1)
    individual[idx] = 1 - individual[idx]
    return individual

def crossover(parent1, parent2):
    point = random.randint(1, len(parent1)-1)
    return parent1[:point] + parent2[point:]

# Inicialização
population = [[random.randint(0,1) for _ in range(6)] for _ in range(4)]

# Evolução
for generation in range(5):
    population = sorted(population, key=fitness, reverse=True)
    print(f'Geração {generation}: {population}')
    next_gen = population[:2]
    while len(next_gen) < 4:
        parent1, parent2 = random.sample(next_gen, 2)
        child = crossover(parent1, parent2)
        child = mutate(child)
        next_gen.append(child)
    population = next_gen
```

**Referências**

- Russell, S., & Norvig, P. (2010). Artificial Intelligence: A Modern Approach (3ª ed.). Prentice Hall.
- Ghallab, M., Nau, D., & Traverso, P. (2004). Automated Planning: Theory and Practice. Morgan Kaufmann.
- Nilsson, N.J. (1982). Principles of Artificial Intelligence. Tioga Publishing Co.
- Holland, J. H. (1975). Adaptation in Natural and Artificial Systems.
- Goldberg, D. E. (1989). Genetic Algorithms in Search, Optimization, and Machine Learning.
- Mitchell, M. (1998). An Introduction to Genetic Algorithms.
