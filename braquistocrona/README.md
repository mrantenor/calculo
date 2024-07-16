# BRAQUISTÓCRONA
## PROBLEMA:

Podemos utilizar o cálculo de variações para resolver um problema clássico na história da física: a braquistócrona. Considere uma parlícula que se move em um campo de força constante que inicia do repouso de algum ponto $(x_1,y_1)$ a algum ponto mais inferior $(x_2,y_2)$. Encontre o caminho que permite que a partícula atravesse o trânsito no menor tempo possível.


A energia cinética $T$ e potencial $U$ do sistema podem ser descritos como:
$$\normalsize{ T = \frac{1}{2}m\dot{x}}$$
$$\normalsize{ U = -Fx = -mgx }$$
Se medirmos o potencial no ponto $x=0$, então a particula parte do repouso em um campo conservativo:
$$\normalsize{E = T + U = 0}$$
$$\normalsize{E = \frac{1}{2}m\dot{x}^2 - mgx = 0}$$
$$\normalsize{\frac{1}{2}m\dot{x}^2 = mgx}$$
$$\normalsize{\dot{x} = \sqrt{2gx}}$$
Obtendo o elemento de distância e utilizando a relação $\Delta s = \dot{x} \Delta t$ para obter a função do tempo no intervalo de $(x_1,y_1)$ a $(x_2,y_2)$: 
$$\normalsize{ds = \sqrt{dx^2+dy^2}}$$
$$\normalsize{ds = \sqrt{1+\dot{y}^2} dx}$$
Dividindo pela velocidade para obter $dt$:
$$\normalsize{\frac{dt}{dx} = \frac{ds}{dx}\frac{1}{\dot{x}} = \sqrt\frac{1+\dot{y}^2}{2gx}}$$
Integrando para obter o tempo e simultâneamente extraíndo a constante $1/\sqrt{2g}$  para simplificar os cálculos:
$$\normalsize{t =\frac{1}{\sqrt{2g}} \int_{x_1}^{x_2} \sqrt\frac{1+\dot{y}^2}{x}dx}$$ Neste ponto, podemos utilizar a equação de Euler, atribuíndo o integrando como função a ser derivada:
$$\normalsize{\frac{\partial f}{\partial y} - \frac{d}{dx} \Big( \frac{\partial f}{\partial \dot{y}}\Big) = 0}$$
$$\normalsize{f=\sqrt\frac{1+\dot{y}^2}{x}}$$
Sabendo que $\partial f /\partial y = 0$, $\partial f / \partial \dot{y}$ só pode ser uma constante
$$\normalsize{\frac{d}{dx} \Big( \frac{\partial f}{\partial \dot{y}}\Big)} = 0$$
Portanto:
$$\normalsize{\frac{\partial f}{\partial \dot{y}} = \frac{\partial }{\partial \dot{y}}\Bigg[ \frac{1+\dot{y}^2}{x}\Bigg ]^{1/2}} = \frac{1}{2} \Bigg [ \frac{1+\dot{y}^2}{x}\Bigg ]^{-1/2}\frac{2\dot{y}}{x} = \text{constante}$$
Assumindo que a constante seja $1/\sqrt{2a}$ e utilizando manipulações algébricas para isolar $\dot{y}$
$$\normalsize{\Bigg[\frac{x}{1+\dot{y}^2}\Bigg]^{1/2}\frac{\dot{y}}{x}=\frac{1}{\sqrt{2a}}}$$
$$\normalsize{\dot{y}=\frac{x}{\sqrt{2ax-x^2}}}$$
Podemos integrar para obter a primitiva:
$$\normalsize{y = \int_{x_1}^{x_2}\frac{x}{\sqrt{2ax-x^2}} dx}$$
Usando a substituição de variável:

$$x = a(1-\cos\theta)$$
$$dx = a\sin\theta d \theta$$
A integral fica
$$\normalsize{y = \int_{\theta_{1}}^{\theta_2}a(1-\cos\theta)d\theta}$$
$$\normalsize{y = a(\theta-\sin\theta)+\alpha}$$
Sendo $\alpha$ constante.
Portanto as equações paramétricas do cilóide que passa pela origem são:
$$\begin{array}{rcl}
x =  a(1-cos\theta)\\ 
y =  a(\theta-\sin\theta)\\ 
\end{array} \Bigg \} $$

---

## CÓDIGO
Para iniciar o código, vamos importar duas bibliotecas `matplotlib.pyplot` para visualização gráfica e `numpy` como suporte matemático.

```
import matplotlib.pyplot as plt
from numpy import *
```

### Funções
Em primeiro lugar, é viável criar uma classe para a função que iremos obter. Essa classe torna mais intuitiva a identificação das variáveis que a função retornará. Por exemplo, quando quisermos obter o valor da posição x da partícula, basta chamarmos a função com o sufixo ".x_position": `braq().x_position`. Então, criaremos variáveis das entradas e saídas da função: constante `a` raio do ciclóide, variável `theta`, a quantidade `n` de passos que `theta` irá variar, as posições x e y como `x_position` e `y_position`, respectivamente e, por fim as posições x e y do ciclóide, respectivamente `x_circ` e `y_circ`.

```
class infos:
    def __init__(self, a, theta, x_position, y_position, x_circ, y_circ, n):
        self.a = a
        
        self.theta = theta
        
        self.x_position = x_position
        self.y_position = y_position
        
        self.x_circ = x_circ
        self.y_circ = y_circ
        
        self.n = n
```

Iremos definir a função da braquistócrona, onde colocaremos os valores de entrada `a` que é o raio do ciclóide, `n` numero inteiro de passos em que `theta` irá variar e por fim, `theta` que será um vetor no qual irá variar de uma posição inicial específica, até uma posição final que será um multiplo de $\pi$ ( a entrada será no formato `theta=(0,2)`, que neste exemplo irá variar de $0$ a $2\pi$).

A seguir, após definirmos as entradas da função, iremos definir as variáveis:
- **theta_**: A variável `theta_` irá pegar os valores iniciais e finais atribuídos na entrada `theta` da função e irá variar na quantidade de passos especificada na entrada `n`, se apropriando da função `linspace` do `numpy`;
- **alpha**: Esta variável será o ângulo do ciclóide no plano xy variando de $0$ a $2\pi$ para ilustrar um círculo fechado;
- **x_position**: Iremos colocar a função paramétrica da posição x da partícula $x = a(1-\cos\theta)$ (`cos` é uma função do `numpy`);
- **y_position**: Iremos colocar a função paramétrica da posição y da partícula $y = a(\theta-\sin\theta)$ (`sin` é uma função do `numpy`);
- **x_circ**: Será a função paramétrica de x da origem do ciclóide;
- **y_circ**: Será a função paramétrica de y da origem do ciclóide.

E por fim, iremos utilizar a classe criada na última célula `infos` para retornar os valores de `a`, `theta_`, `x_position`, `y_position`, `x_circ`, `y_circ` e `n`.
```
def braq(a = 1, n = 200, theta = (0,1.4)):
    theta_ = linspace(theta[0],theta[1] * pi,n)
    alpha = linspace(0,2*pi,n)
    
    x_position = a * (1 - cos(theta_))
    y_position = a * (theta_ - sin(theta_))
    
    x_circ = a * cos(alpha)
    y_circ = a * sin(alpha)
    
    return infos(a, theta_, x_position, y_position, x_circ, y_circ, n)
```


### Gráfico
Neste instante, temos todas as funções necessárias para plotar o gráfico. Em primeiro lugar, iremos criar uma estrutura de repetição, que irá variar `i` de $0$ até `n` em intervalos de $3$ (esse valor será a quantidade de frames o inteiro mais próximo da razão `n`$/3$).

Utilizaremos a função `figure` do `matplotlib.pyplot`, para definir o tamanho da plotagem e a resolução (dpi=250).

#### Partícula e sua Trajetória
Para desenhar a trajetória que a partícula irá percorrer, basta plotar as suas respectivas posições desconectado da estrutura de repetição.
```
plt.plot(braq().y_position, 
		 braq().x_position,
		 c='b') ### TRAJETÓRIA (X1,Y1) até (X2,Y2) cor azul
```
![[004.jpg]]

Agora, no exemplo gráfico do livro ele demonstra uma linha tracejada que ilustra toda a trajetória que a partícula poderia percorrer se não houvesse limitação das posições iniciais e finais. Aqui iremos alterar os parâmetros iniciais e finais da trajetória, para que ela percorra um caminho mais longo, com `theta` de $0$ a $5\pi/2$:
```
plt.plot(braq(theta=(0,2.5)).y_position,
		 braq(theta=(0,2.5)).x_position,
		 c='grey',lw=1, ls='--',zorder=1) ### Tracejada
```
![[008.jpg]]


Para a partícula se movendo, iremos utilizar variável `i` da estrutura de repetição para identificar cada posição em cada passo dos vetores `x_position` e `y_position`, acrescidos com a identificação do índice do vetor `[i]`, assim nos retornará a partícula em cada *frame*.
```
plt.scatter(braq().y_position[i], # Posição X da partícula
                braq().x_position[i], # Posição Y da partícula
                c='r',zorder=10) #Cor vermelha e ordena a partícula para o topo
```
![[1.gif]]

Para colocar os pontos estácionários $(x_1,y_1)$ e $(x_2,y_2)$, iremos utilizar a função `scatter`, no ultimo ponto iremos ultilizar os penultimos `[-2]` valores das posições x e y da trajetória da partícula:

```
plt.scatter(0,0,c='g',zorder=9) ### (X1,Y1) (0,0) cor verde (c='g')

plt.scatter(braq().y_position[-2],
			braq().x_position[-2],c='g',zorder=9)  ### (X2,Y2) 
```
![[2.gif]]

#### Circunferência
Para ilustrar a circunferência, temos que visualizar alguns elementos separadamente. O primeiro a ser analisado, será o raio que acompanha a partícula. Seu tamanho será unitário, então, iremos criar um vetor  com auxílio da função `linspace` que varia de 0 a 1, em 100 passos, que multiplicará o seno do angulo `theta` da função variando na estrutura de repetição, somado a posição da partícula, também variando de acordo com a estrutura de repetição (`lw` é a espessura da circunferência):

$$\normalsize{x_{circulo} = r\cos\theta+x_{particula}}$$
$$\normalsize{y_{circulo} = r\sin\theta+y_{particula}}$$

```
    plt.plot(linspace(0,1,100)*sin(braq().theta[i])+braq().y_position[i],   
             linspace(0,1,100)*cos(braq().theta[i])+braq().x_position[i],
             lw=0.7,c='k')            
```
![[3.gif]]

Agora, basta plotar a circunferência. A partícula não varia em $y$, apenas em $x$, onde plotamos a circunferência `braq().x_circ` e `braq().y_circ` em seus eixos, e a variação em $x$ é de acordo com `theta`:
```
plt.plot(-braq().y_circ + braq().theta[i],
		  braq().x_circ + braq().a,
		 c='k',lw=0.7,ls='--')  ### CIRCUNFERÊNCIA

```
![[4.gif]]

Iremos, então acrescer um raio que se move apenas ao longo do eixo $y$, para demonstrar como $\theta$ varia com a trajetória da partícula ilustrado na circunferência:
```
plt.plot(linspace(0,1,100)*sin(braq().theta[0])+braq().theta[i],    
		 linspace(0,1,100)*cos(braq().theta[0]),
		 lw=0.7,c='k')
```
![[5.gif]]
E para finalizarmos a etapa da circunferência, vamos acrescer uma circunferência menor, que irá aumentar conforme $\theta$ aumenta, para ilustrar a variação do ângulo. Nosso objetivo neste ponto é fazer com que `theta` crie uma trejetória a medida em que o indice da estrutura de repetição aumenta. Então, utilizaremos `[:i]` em `theta` para que isso aconteça. O fator de multiplicidade $0,2$ é para que o tamanho da circunferência menor seja $20\%$  da circunferência maior.

```
plt.plot(braq().theta[i] - braq().a * sin(braq().theta[:i])*0.2,
		 -braq().a * cos(braq().theta[:i])*0.2 + braq().a,
		 c='k',lw=0.7)    ### ANGULO SENDO PREENCHIDO
```
![[6.gif]]
### Textos
Com auxílio da função `annotate`, iremos colocar os textos no gráfico, com seus parâmetros de classe: texto, posição x e y e tamanho da fonte. Alguns textos irão variar conforme a posição da partícula e seus elementos, então utilizaremos também as funções da posição dentro da estrutura de repetição para realizá-los.
```
    plt.annotate('($x_1$,$y_1$)',
			    (0.1,-0.1),
			    fontsize=10)
			    
    plt.annotate('($x_2$,$y_2$)',
			    (5.5,1.5),
			    fontsize=10)
			    
    plt.annotate('P($x$,$y$)',
			    (braq().y_position[i]+0.3,braq().x_position[i]+0.3),
			    fontsize=8)
			    
    plt.annotate(r'$\theta$',
				(sin(braq().theta[i])+braq().y_position[i]-0.5,braq().a),
				fontsize=10)
				
    plt.annotate(r'$a$',
			    (sin(braq().theta[i])+braq().y_position[i]-0.3,braq().a-0.5),
			    fontsize=10)
```

#### Eixos
Com auxilio das funções `hlines` e `vlines` acrescemos uma linha horizontal e vertical que coincidem na origem. As funções `x_ticks` e `x_labels` nos ajudam a restringir os traços do gráfico sejam multiplos inteiros de $a\pi$ para o eixo $y$ e multiplos de $a$ para o eixo $x$ e `axis` ajuda a forçar o gráfico ser quadrado, para que a circunferência não esteja distorcida.

```
    plt.hlines(0,-3,9,color='k',lw=1)
    plt.vlines(0,-2,4,color='k',lw=1)
    
    x_ticks = [-pi*braq().a,0,pi*braq().a,2*pi*braq().a,3*pi*braq().a]
    x_labels = [r'$-\pi a$',r'$0$',r'$\pi a$',r'$2\pi a$',r'$3\pi a$'] 
    
    y_ticks = [-1*braq().a,0,1*braq().a,2*braq().a,3*braq().a]
    y_labels = [r'$-a$',r'$0$',r'$a$',r'$2a$',r'$3a$'] 

    plt.xticks(ticks=x_ticks, labels=x_labels)
    plt.yticks(ticks=y_ticks, labels=y_labels)
    
    plt.axis('square')
    plt.xlabel('Y')
    plt.ylabel('X')

    plt.ylim(3,-1)
    plt.xlim(-2,8)
```
Por fim, temos a ilustração completa:
![[7.gif]]

Ao todo, o código gráfico ficou:
```
for i in arange(0,braq().n,3):
    plt.figure(figsize=(8,4),dpi=250)
    ############################
    ######## PARTÍCULA #########
    ############################
    plt.scatter(braq().y_position[i], # Posição X da partícula
                braq().x_position[i], # Posição Y da partícula
                c='r',zorder=10) #Cor vermelha e ordena a partícula para o topo
    
    plt.plot(braq().y_position, 
             braq().x_position,
             c='b') ### TRAJETÓRIA (X1,Y1) até (X2,Y2)
    
    plt.plot(braq(theta=(0,2.5)).y_position,
             braq(theta=(0,2.5)).x_position,
             c='grey',lw=1, ls='--',zorder=1) ### Tracejada
    
    
    ################################
    ##### Pontos Estacionários #####
    ################################
    plt.scatter(0,0,c='g',zorder=9) ### (X1,Y1)
    plt.scatter(braq().y_position[-2],
                braq().x_position[-2],c='g',zorder=9)  ### (X2,Y2)
    

    ##########################
    ##### Circunferência #####
    ##########################
    plt.plot(linspace(0,1,100)*sin(braq().theta[i])+braq().y_position[i],   
             linspace(0,1,100)*cos(braq().theta[i])+braq().x_position[i],
             lw=0.7,c='k')     #raio que acompanha a partícula                                             
    
    plt.plot(linspace(0,1,100)*sin(braq().theta[0])+braq().theta[i],    
             linspace(0,1,100)*cos(braq().theta[0]), lw=0.7,c='k')      #### RAIO IMOVEL "a" #####

    
    plt.plot(braq().theta[i] - braq().y_circ,
             braq().x_circ + braq().a,
             c='k',lw=0.7,ls='--')  ### CIRCUNFERÊNCIA
    
    
    plt.plot(braq().theta[i] - braq().a * sin(braq().theta[:i])*0.2,
             -braq().a * cos(braq().theta[:i])*0.2 + braq().a,
             c='k',lw=0.7)    ### ANGULO SENDO PREENCHIDO
    
    ###########################
    ######### TEXTOS ##########
    ###########################
    plt.annotate('($x_1$,$y_1$)',(0.1,-0.1),fontsize=10)
    plt.annotate('($x_2$,$y_2$)',(5.5,1.5),fontsize=10)
    plt.annotate('P($x$,$y$)',(braq().y_position[i]+0.3,braq().x_position[i]+0.3),fontsize=8)
    plt.annotate(r'$\theta$',(sin(braq().theta[i])+braq().y_position[i]-0.5,braq().a),fontsize=10)
    plt.annotate(r'$a$',(sin(braq().theta[i])+braq().y_position[i]-0.3,braq().a-0.5),fontsize=10)
    
    
    #########################
    ######## GRÁFICO ########
    #########################
    plt.hlines(0,-3,9,color='k',lw=1)
    plt.vlines(0,-2,4,color='k',lw=1)
    
    x_ticks = [-pi*braq().a,0,pi*braq().a,2*pi*braq().a,3*pi*braq().a]
    x_labels = [r'$-\pi a$',r'$0$',r'$\pi a$',r'$2\pi a$',r'$3\pi a$'] 
    
    y_ticks = [-1*braq().a,0,1*braq().a,2*braq().a,3*braq().a]
    y_labels = [r'$-a$',r'$0$',r'$a$',r'$2a$',r'$3a$'] 

    plt.xticks(ticks=x_ticks, labels=x_labels)
    plt.yticks(ticks=y_ticks, labels=y_labels)
    
    plt.axis('square')
    plt.xlabel('Y')
    plt.ylabel('X')

    plt.ylim(3,-1)
    plt.xlim(-2,8)

    plt.savefig(str(i).zfill(3)+'.jpg') #Salva diversos frames na pasta
    plt.close()
```

## REFERÊNCIAS
Thornton, Stephen T., and Jerry B. Marion. _Dinâmica clássica de partículas e sistemas_. Cengage Learning, 2011.