# Práctica 12. Programación Gráfica Orientada a Eventos. Poker,
### Factor de ponderación: 9

### Objetivos
Los objetivos de esta práctica son:

* Poner en práctica conceptos de Programación Orientada a Eventos en JavaScript.
* Poner en práctica metodologías y conceptos de Programación Orientada a Objetos.
* Poner en práctica conceptos de Programación Gráfica en JavaScript usando la API Canvas.
* Practicar el uso de elementos HTML básicos.
* Practicar el uso de algunas propiedades básicas de CSS para dotar de estilo a una página web simple.
* Poner en práctica metodologías y conceptos de Programación Orientada a Objetos en JavaScript.
* Profundizar en el uso de pruebas de software (testing) utilizando Mocha y Chai.

### Rúbrica de evaluacion del ejercicio
Se señalan a continuación los aspectos más relevantes (la lista no es exhaustiva)
que se tendrán en cuenta a la hora de evaluar esta práctica:

* El comportamiento del programa debe ajustarse a lo solicitado en este enunciado.
* Deben usarse estructuras de datos adecuadas para representar los diferentes elementos que intervienen en el problema.
* Capacidad del programador(a) de introducir cambios en el programa desarrollado.
* Se comprobará que el código que el alumnado escribe se adhiere a las reglas de la 
  [Guía de Estilo](https://google.github.io/styleguide/jsguide.html).
* El código ha de estar documentado con [JSDoc](https://jsdoc.app/). 
  Haga que la documentación del programa generada con JSDoc esté disponible a través de una web alojada en su máquina IaaS de la asignatura.
* El alumnado ha de acreditar su capacidad para configurar y ejecutar 
  [ESLint](https://eslint.org/)
  en sus programas.
* El alumnado ha de acreditar que sabe depurar sus programas usando Visual Studio Code.
* Se comprobarán los tests unitarios que el alumnado ha desarrollado para todos sus códigos usando Mocha y Chai, así como
  su capacidad para ejecutar esos tests unitarios desde la interfaz de VSC. 
  Todo el código de los tests que realice para desarrollar la aplicación estarán ubicados en el directorio
  `test` de proyecto.
* Se comprobará el cubrimiento de código que se consigue con los tests usando la herramienta 
  [CodeCov](https://about.codecov.io/)
  y el informe que la misma genera a través de una web.
* El programa debe ajustarse al paradigma de Orientación a Objetos.
* Todo el código estará ubicado en el directorio `src` del proyecto. Use subdirectorios de éste si le resulta
  conveniente.


### El juego del Poker
En esta práctica se desarrollará una aplicación JavaScript para representar 
[cartas de la baraja francesa](https://en.wikipedia.org/wiki/Standard_52-card_deck), mazos de cartas, manos y jugadas del Póquer.
Consulte
[Wikipedia](http://en.wikipedia.org/wiki/Poker), 
para un conocimiento básico de este juego, en caso de que no lo conozca.
Este documento explica todo lo que se precisa para los ejercicios a realizar.

A pesar de que este documento está escrito en español, se propone que los identificadores 
que se usen en el código JavaScript utilicen la terminología en inglés para el entorno a 
modelar en los programas: cartas (cards), mazo de cartas (deck), etc.

### El problema de las 8 reinas

### La clase *Card*
Se propone desarrollar una clase `Card` que permita representar cartas de la barja francesa.
La clase ha de encapsularse en un módulo ES6 `card.js`.
La baraja francesa está dividida en cuatro palos (en inglés: *suit*), dos de color rojo y dos de color negro:

* ♠ Spades (picas).
* ♥ Hearts (corazones).
* ♦ Diamonds (diamantes).
* ♣ Clubs (tréboles).

Cada palo está formado por 13 cartas, de las cuales 9 son numerales y 4 literales. 
Se ordenan de menor a mayor "rango" de la siguiente forma: A, 2, 3, 4, 5, 6, 7, 8, 9, 10, J, Q, K. 
Las cartas con letras (figuras), se llaman Jack (J), Queen (Q), King (K) y Ace (A).
Dependiendo del juego, un As puede ser más alto que el Rey o más bajo que 2.

Si se quiere definir un objeto para representar una carta de juego, es obvio cuáles deben ser los atributos imprescindibles: valor y palo.
Cualquier implementación que se elija para los atributos ha de permitir comparar cartas para determinar cuál tiene un valor o palo más alto.

Defina una clase `Card` para representar las cartas.
Si no se especifica algo diferente, al crear un objeto de esta clase se crearía un 2 de tréboles.
Para instanciar un objeto Carta se usaría un código como:
```javascript
const myCard = new Card(SUIT, RANK);
```
Desarrolle un método `toString()` que permita imprimir un objeto `Card`.
Las cartas han de poder imprimirse de forma que sean legibles para un humano.
Así en pantalla esperamos encontrar textos como:

```
Ace of Diamonds
9 of Hearts
Jack of Spades
```

Y debería poderse escribir:

```javascript
const jackOfHearts = new Card(HEARTS, JACK);
console.log(jackOfHearts);  // -> Jack of Hearts
```

El directorio `img` de este proyecto contiene ficheros gráficos correspondientes a todas las cartas de la
baraja francesa.

También resulta necesario un mecanismo que permita comparar cartas.
El orden de las cartas no es obvio. 
Por ejemplo, ¿qué carta es mejor, el 3 de tréboles o el 2 de diamantes?
Una tiene un valor más alto, pero la otra tiene un palo más alto. 
Para comparar las cartas, ha de decidirse si es más importante el valor de la carta o el palo.
La respuesta puede depender del juego que se esté jugando, pero para simplificar, 
se hará la elección arbitraria de que el palo es más importante. 
Se asumirá la siguiente ordenación para los palos:

`Clubs < Diamonds < Hearts < Spades`

así que, por ejemplo, todas las picas superan a todos los corazones, y así sucesivamente.

### Mazos. La clase *Deck* 
Una vez diseñadas las cartas, el siguiente paso será definir un mazo (baraja completa). 
Puesto que un mazo está compuesto de cartas, es natural que cada mazo contenga como atributo un conjunto de cartas.
Defina una clase `Deck` para modelar un mazo de cartas. 
El constructor de esa clase deberá inicializar el mazo con el conjunto estándar de 52 cartas.
La clase `Deck` ha de disponer de un método que permita imprimir el mazo:

```
Ace of Clubs
2 of Clubs
3 of Clubs
...
10 of Spades
Jack of Spades
Queen of Spades
King of Spades
```

### Añadir, quitar, barajar y clasificar
Para gestionar el mazo y poder repartir cartas se precisan métodos para
* Eliminar una carta del mazo y devolverla. `popCard()`
* Añadir una carta determinada al mazo. `addCard()`
* Barajar (mezclar) las cartas del mazo de modo que al sacar una carta del mazo, 
  ésta no esté predeterminada por la configuración del mismo (aleatoriedad). `shuffle()`
* Resulta conveniente disponer de un método `sort()` que ordene las cartas del mazo.

### Manos de cartas. La clase *Hand*.
Para avanzar en el diseño de la aplicación propuesta se precisa también una clase 
para representar las manos (las cartas asignadas a un jugador).
Una mano (*Hand*) es similar a un mazo: tanto un mazo como una mano están formados 
por un conjunto de cartas, y ambos requieren operaciones como añadir y quitar cartas.
Una mano también es diferente de un mazo puesto que hay operaciones necesarias para las manos que no tienen sentido para un mazo. 
Por ejemplo, en el póquer podríamos comparar dos manos para ver cuál gana. 
En el bridge, podríamos calcular la puntuación de una mano para hacer una apuesta.
El método que inicialice una mano debería inicializarla con un conjunto vacío de cartas:

```javascript
hand = new Hand('new hand')
console.log(hand.cards);	// -> [ ]
console.log(hand.label);	// -> new hand
```
La clase `Hand` también ha de disponer, como se ha expuesto, de métodos `popCard()` y `addCard()`:

```javascript
let deck = new Deck();
let card = deck.popCard();
hand.addCard(card);
console.log(hand);	// -> King of Spades
```

Un paso adicional es disponer de un método `moveCards()` que toma dos argumentos: una mano y el número de cartas a repartir.
`moveCards()` toma el número de cartas a repartir del mazo y las coloca en la mano.

En algunos juegos, las cartas se mueven de una mano a otra, o de una mano al mazo. Se puede usar `moveCards()` para cualquiera de estas operaciones.

### Ejercicios
Ejercicio 1. Escriba un método de `Deck` llamado `dealHands()` que toma dos parámetros: el número de manos y el número de cartas por mano. 
Debe crear el número apropiado de manos, repartir el número apropiado de cartas por mano y devolver una lista de Manos.

Ejercicio 2. En el póker las manos son de 5 cartas. 
Las siguientes son las posibles manos en el póquer, en orden creciente de valor y decreciente de probabilidad:

* *Pair* (Pareja): dos cartas con el mismo valor.
* *Two Pair* (Doble par): dos pares de cartas con el mismo valor.
* *Three of a kind* (Trío): tres cartas con el mismo valor.
* *Straight* (Escalera): cinco cartas con valores en secuencia (los ases pueden ser altos o bajos, así que el As-2-3-4-5 es una escalera y también lo es el 10-Jack-Queen-King-Ace, pero el Queen-King-Ace-2-3 no lo es).
* *Flush* (Color): cinco cartas con el mismo palo.
* *Full House* (Full): tres cartas con un valor, dos cartas con otro.
* *Four of a Kind* (Poker): cuatro cartas con el mismo valor.
* *Straight Flush* (Escalera real): cinco cartas en secuencia (como se definió anteriormente) y con el mismo palo.

El objetivo de este ejercicio es estimar la probabilidad de conseguir estas diversas manos.

2.1 Desarrolle un programa `poker-hand.js` que implemente una clase `PokerHand()` que representará una mano de Póker.
Escriba un programa `poker-game.js` que reparta siete manos de 
[póquer de 7-cartas](https://en.wikipedia.org/wiki/Seven-card_stud)
véase además [esta otra referencia](http://people.math.sfu.ca/~alspach/comp20/)
y comprueba si alguna de ellas contiene una escalera.

2.2 Añada métodos a `poker-hand.js` llamados `hasPair()`, `hasTwopair()`, `hasThreeOfaKind()` etc. que devuelven Verdadero o Falso según si la mano cumple o no los criterios pertinentes. 
Su código debería funcionar correctamente para manos que contengan cualquier número de cartas (aunque 5 y 7 son los tamaños más comunes).

2.3 Escriba un método `classify()` que determine la clasificación de mayor valor para una mano y establezca el atributo de la etiqueta de esa mano en consecuencia. 
Por ejemplo, una mano de 7 cartas podría contener una escalera y un par; debería ser etiquetada como "flush" (escalera).

2.4 Cuando esté convencido de que sus métodos de clasificación funcionan, el siguiente paso es estimar las probabilidades de las distintas manos. 
Escriba una función en `poker-hand.js` que baraje un mazo de cartas, la divida en manos, clasifique las manos y cuente el número de veces que aparecen las distintas clasificaciones.

Imprima una tabla de las clasificaciones y sus probabilidades y muestre esos resultados tanto por pantalla como a través de un fichero JSON.
Ejecute su programa con un número cada vez mayor de manos hasta que los valores de salida converjan con un grado razonable de precisión.

Compare sus resultados con los valores que se indican [en Wikipedia](https://en.wikipedia.org/wiki/Poker_probability).

#################################################
* Comience por adaptar su programa del problema de las 8 reinas al paradigma orientado a objetos:
  identifique clases y métodos y reescriba ese programa de forma correspondiente.

* Desarrolle una página web cuya interfaz gráfica se asemeje lo más posible, en cuanto a su apariencia, no en
  cuanto a sus funcionalidades, a la que se muestra en esta imagen:
![Ajedrez](https://raw.githubusercontent.com/fsande/PAI-Labs-Public-Data/master/img/p11_Chess/chess.png "Ajedrez")
  También puede ver la interfaz que se pretende imitar iniciando una partida en 
  [esta página de juego on-line de ajedrez](https://lichess.org).
	Su página ha de imitar colores, tipografías, tamaños y distribución de los elementos.
  En el directorio `img` de este proyecto puede Ud. encontrar imágenes gráficas para las figuras del juego.
  Puede Ud. usar estas u otras si le parecen más adecuadas.

* Se colocarán en la página enlaces similares a los que figuran en la página de referencia, pero en su caso
	esos enlaces no estarán operativos (no enlazan a otras páginas) o en todo caso enlazarán con páginas
  alojadas en su máquina IaaS de la asignatura.

* Añada un pie de página (*footer*) en el que incluya información sobre la Universidad,
  titulación y asignatura.

* Añada en su página los siguientes elementos:

1. Un botón `Generar solución` que al ser pulsado dibuje en el tablero sucesivas soluciones al problema de las 8
reinas.

2. Un segundo botón `Partida de ajedrez` que al ser pulsado dibuje en el tablero la configuración inicial de
las piezas de una partida de ajedrez. 

* Sustituya en su página el panel que figura a la derecha del tablero en la web de referencia e incluya en él
  un panel en el que, para cada solución que se muestre para el problema de las 8 reinas imprima 
	en [notación algebraica](https://en.wikipedia.org/wiki/Algebraic_notation_(chess)) las posiciones que ocupa
	cada una de las 8 reinas ubicadas en el tablero.
	
* No añada a la interfaz gráfica (web) de su programa otros elementos que los que se describen en esta especificación.
  Trate asimismo de ceñirse a la utilización de los elementos HTML y CSS estudiados en las clases de teoría.

### Presentación de resultados
La visualización de la ejecución del programa se realizará a través de una página web alojada
en la máquina IaaS-ULL de la asignatura y cuya URL tendrá la forma:

`http://10.6.129.123:8080/einstein-albert-chess.html` [1]

en la que se incustará un canvas para dibujar el tablero.
Sustituya *Albert Einstein* por su nombre y apellido en la URL de su página.

Utilice código HTML y CSS para imitar en la medida de lo posible la apariencia de la web de referencia
[web de referencia](https://lichess.org/).

Diseñe asimismo otra página HTML simple 

`http://10.6.129.123:8080/index.html` [2]

que sirva de "página índice" para los ejercicios de la sesión de evaluación de la práctica.
La página [1] será uno de los enlaces de [2] y a su vez [1] tendrá un enlace "Home" que apunte a [2].
Enlace también en la página índice [2] las páginas que contienen los informes de documentación y de
cubrimiento de código de su proyecto.