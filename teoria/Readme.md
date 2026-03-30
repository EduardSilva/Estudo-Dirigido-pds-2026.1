# Resumo Teórico: Modelagem de Sinais e Sistemas Discretos

Este documento apresenta os fundamentos matemáticos para a representação e análise de sinais discretos, bem como a modelagem de sistemas digitais, relacionando a teoria matemática com a implementação prática em engenharia.

## 1. Sinais Contínuos e Discretos

A modelagem de sistemas de Processamento Digital de Sinais (PDS) exige a transição do domínio analógico para o digital.

* **Sinais Contínuos ($x(t)$):** São definidos para qualquer valor dentro de um intervalo contínuo de tempo. Fisicamente, representam grandezas analógicas ininterruptas, como sinais térmicos de sensores industriais ou sinais de vibração mecânica.
* **Sinais Discretos ($x[n]$):** Conforme abordado por Oppenheim e Schafer, são sequências numéricas definidas apenas em instantes específicos e discretos, indexados por um número inteiro $n$. Na prática, essas sequências são obtidas por meio do processo de amostragem de um sinal contínuo ($x[n] = x(nT_s)$, onde $T_s$ é o período de amostragem). Em arquiteturas de hardware digital, o índice $n$ está diretamente atrelado aos ciclos de *clock* do sistema, permitindo que fenômenos físicos sejam processados computacionalmente.

## 2. Sequências Elementares e Operações Matemáticas

Sinais complexos podem ser decompostos e analisados a partir de sequências fundamentais:

* **Impulso Unitário ($\delta[n]$):** Essencial para caracterizar a resposta ao impulso de um sistema.
    $$\delta[n] = \begin{cases} 1, & n = 0 \\ 0, & n \neq 0 \end{cases}$$
* **Degrau Unitário ($u[n]$):** Utilizado para modelar o acionamento abrupto de sistemas ou sinais que começam em $n=0$.
    $$u[n] = \begin{cases} 1, & n \ge 0 \\ 0, & n < 0 \end{cases}$$
* **Exponenciais Complexas ($e^{j\omega n}$):** Base para a transformada de Fourier discreta e para a análise de resposta em frequência, representando componentes senoidais e cossenoidais
O processamento digital depende de operações diretas nessas sequências:
* **Deslocamento ($x[n-k]$):** Representa um atraso (ou avanço) temporal. Em implementações digitais (RTL), um atraso de $k$ amostras é fisicamente mapeado para a passagem do sinal por $k$ registradores em cascata.
* **Inversão ($x[-n]$):** Espelhamento do sinal em relação ao eixo vertical.
* **Escalonamento:** Pode ocorrer na amplitude do sinal (multiplicação por constante) ou no domínio do tempo (mudança na taxa de amostragem, como processos de decimação e interpolação).

## 3. Caracterização Energética e Temporal

Como destaca Proakis, a análise energética é uma etapa fundamental no desenvolvimento de algoritmos de PDS.

* **Energia ($E$):** A energia total de um sinal discreto é o somatório do módulo ao quadrado de suas amostras. Sinais práticos que possuem começo e fim definidos geralmente são sinais de energia.
    $$E = \sum_{n=-\infty}^{\infty} |x[n]|^2$$
* **Potência Média ($P$):** A potência indica a taxa média de entrega de energia. Sinais periódicos e de duração infinita (como um *clock* elétrico ou ruído constante) possuem energia infinita, sendo classificados e analisados pela sua potência média finita.
    $$P = \lim_{N \to \infty} \frac{1}{2N+1} \sum_{n=-N}^{N} |x[n]|^2$$

## 4. Modelagem e Classificação de Sistemas Discretos

Segundo Lathi, modelar um sistema significa identificar as propriedades que determinam seu comportamento dinâmico na relação entre entrada e saída. As classificações estruturais incluem:

* **Sistemas com e sem Memória:**
    * *Sem memória:* A saída $y[n]$ depende exclusivamente da entrada no instante atual $x[n]$. Equivale a circuitos estritamente combinacionais.
    * *Com memória:* A saída depende de valores passados (ex: $x[n-1]$) ou futuros. Para processar tais sinais digitalmente, exige-se o uso de elementos de estado (como *Flip-Flops* ou buffers de memória) para reter amostras anteriores.
* **Linearidade:** O sistema é linear se satisfaz o princípio da superposição, englobando a aditividade ($T\{x_1[n] + x_2[n]\} = y_1[n] + y_2[n]$) e a homogeneidade ou escalonamento ($T\{ax[n]\} = ay[n]$).
* **Causalidade :** Um sistema é causal quando sua saída $y[n]$ depende apenas de amostras de entrada presentes e passadas ($k \le n$). Sistemas físicos que operam em tempo real, como o processamento de sinais adquiridos por sistemas embarcados em periféricos (ex: via barramento CAN), devem obrigatoriamente ser causais. Sistemas não-causais só podem ser implementados se o sinal inteiro já estiver gravado em memória.
* **Invariância no Tempo:** Um sistema é invariante se suas características operacionais permanecem constantes. Se a entrada é deslocada por $k$ amostras ($x[n-k]$), a saída é idêntica à original, porém deslocada pela mesma quantia ($y[n-k]$). Sistemas Lineares e Invariantes no Tempo (LIT) formam a base do design de filtros digitais.
* **Estabilidade BIBO (Bounded-Input Bounded-Output) :** O sistema é estável se qualquer entrada de amplitude limitada resultar em uma saída também limitada em amplitude. Em implementações lógicas, garantir a estabilidade impede o transbordamento numérico (*overflow*) durante as operações matemáticas no domínio discreto.
* **Invertibilidade :** Ocorre quando entradas diferentes produzem sempre saídas diferentes, permitindo projetar um sistema inverso que recupere o sinal $x[n]$ original a partir de $y[n]$.
