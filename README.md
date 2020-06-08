# Resolución del juego de Conecta 4 en prolog

:office: Universidad de Huelva ([UHU](http://www.uhu.es/index.php))
:calendar: Curso 2019-2020
:mortar_board: Representación del conocimiento
:boy: Manuel Jesús Grávalos Cano


## Introducción

En esta parte del proyecto se desarrolla la implementación del juego conecta 4 en Prolog. Se ha tomado el codigo perteneciente al usuario de github rvinas, en concreto el proyecto [connect 4 prolog](https://github.com/rvinas/connect-4-prolog). El código propuesto presenta una inteligencia artificial básica que permite al usuario jugar contra esta, para poder comprobar que lo implementado sea correcto. Se recomienda usar [GNU Prolog](http://www.gprolog.org/) para una correcta visualización del ejercicio.
*Recomendado leer este documento al completo si se pretende modificar el código así como si se pretende entender sin conocer en profundidad el lenguaje.*

## Juego conecta 4
![ ]([https://lh3.googleusercontent.com/_BwyWmPD6lwF84EFhBvHR1HLn5LfdNf_nfRD9i442-OJYBMb6D9gPSQTh0Ddl8mCiVv53lu4VReliuwFQG3LazL6VTn_TtFkn-XB6mYNhgwKPNZ_Svm4lwq13J59K3CqYyJIXRjeXgLeALtGpgzh49t6fmfmVCeNbwSg_WrBS0faGMABC3wXp6fUJgXtquo47A32jTCFV9rxxHLNvxqkHPOAJjUFj-fENcwIcKtnplgwycHeyJni9fUoT2E49eWu_P16ShGkgUiTF9Fet1k6ikEsMm3jN_-W2gRCcTgoSDN-IJvadA9iEorMYqhAjfYcTMUlX5FLFxny5CefItRoNR_FxK8VFDCiYIUJXQMqZD4pEQIcPEpvSeGj5PnVASpk-8_fptiC2K1lrIra5WVotkRS2MO1sRssO7euTT2-L2xFutT24__u1RRmRsotx0QFqN3GfDONuJtawcr8k28LPuPouGObJIgBoF_UShfEIsD1-My3P1CdbRy1TNQJN6juaZQn0S7zuNg20exlscgbmHRychJFy-9uEPnkd6s9z3o8bsB_Q8OA-fLcqYGnjqWt5kK5r6ofAOfiMHXAO-SQSgviNd7Wb21Qmh7mQ13FIi4_8tq2xNUNB9CXZtMOb0rMGin1e2JdeN3wRy4EnD2I80saF3Soq1ivadwBLgpcHhjpCcpx6Y5zNqUbrlRe=w189-h159-no?authuser=0](https://lh3.googleusercontent.com/_BwyWmPD6lwF84EFhBvHR1HLn5LfdNf_nfRD9i442-OJYBMb6D9gPSQTh0Ddl8mCiVv53lu4VReliuwFQG3LazL6VTn_TtFkn-XB6mYNhgwKPNZ_Svm4lwq13J59K3CqYyJIXRjeXgLeALtGpgzh49t6fmfmVCeNbwSg_WrBS0faGMABC3wXp6fUJgXtquo47A32jTCFV9rxxHLNvxqkHPOAJjUFj-fENcwIcKtnplgwycHeyJni9fUoT2E49eWu_P16ShGkgUiTF9Fet1k6ikEsMm3jN_-W2gRCcTgoSDN-IJvadA9iEorMYqhAjfYcTMUlX5FLFxny5CefItRoNR_FxK8VFDCiYIUJXQMqZD4pEQIcPEpvSeGj5PnVASpk-8_fptiC2K1lrIra5WVotkRS2MO1sRssO7euTT2-L2xFutT24__u1RRmRsotx0QFqN3GfDONuJtawcr8k28LPuPouGObJIgBoF_UShfEIsD1-My3P1CdbRy1TNQJN6juaZQn0S7zuNg20exlscgbmHRychJFy-9uEPnkd6s9z3o8bsB_Q8OA-fLcqYGnjqWt5kK5r6ofAOfiMHXAO-SQSgviNd7Wb21Qmh7mQ13FIi4_8tq2xNUNB9CXZtMOb0rMGin1e2JdeN3wRy4EnD2I80saF3Soq1ivadwBLgpcHhjpCcpx6Y5zNqUbrlRe=w189-h159-no?authuser=0))+N
Conecta 4 es un juego con un tablero vertical de 6 filas y 7 columnas para dos jugadores, donde los jugadores se alternan y ponen una ficha de su color o forma en una de las columnas, de forma que siempre quedará en la fila más inferior que esté libre.

## Predicados en prolog
En este apartado se explicarán los distintos predicados que existen para que funcione el juego, aunque sean invisibles para el jugador.

### connect4.
Para iniciar el juego, se utilizará la instrucción connect4. Posteriormente, solo tendremos que indicar con una letra de la A a la G en qué columna se desea jugar en cada turno. Si se introduce en una columna errónea, porque ya esté llena o porque se salga del margen, se pedira nuevamente

	connect4:- initial(X),show(X),nextMove('X',X), !.

### initial(-X)

Carga el tablero. Líneas de este tipo ya se han utilizado en los árboles para evitar crearlos en cada llamada.

    initial(board([['-','-','-','-','-','-'],
			       ['-','-','-','-','-','-'],
			       ['-','-','-','-','-','-'],
			       ['-','-','-','-','-','-'],
			       ['-','-','-','-','-','-'],
			       ['-','-','-','-','-','-'],
			       ['-','-','-','-','-','-']])).

### show(+A), iShow(+X,+N), showLine(+X,+N,+X2), iShowLine(+X,+X2)

Muestra el tablero. Primero indicará las letras pertenecientes a las columnas y, posteriormente, recorrerá columna a columna.
A será algo que concatene con board(X).
X2 	es X pero sin la linea visible.
N es la columna en la que nos encontramos.

    show(board(X)):- write('  A B C D E F G'), nl, iShow(X,6).
    iShow(_,0).
	iShow(X,N):- showLine(X,N,X2),Ns is N-1,iShow(X2,Ns).
	showLine(X,N,X2):- write(N), write(' '),iShowLine(X,X2), nl.
	iShowLine([],_).
	iShowLine([[X|X2]|XS],[X2|XS2]):- write(X), write(' '),iShowLine(XS,XS2).
	
Podemos observar la inducción dada durante la teoría en el recorrido de iShow, puesto que cada vez que se llama, Ns toma una unidad menos que N hasta que llega a 0, caso base que siempre será verdadero.

### nextMove(+J,+X)
Indica en que columna X colocará ficha el jugador J. Además, comprueba si este último ha conseguido o no ganar. Si una columna está llena o no es una columna válida, el jugador J volverá a intentar colocar ficha.

    nextMove('X',X):- wins('O',X),write('Machine wins!').
	nextMove('O',X):- wins('X',X),write('You win!').
	nextMove(_,X):- full(X),write('Draw').
	nextMove('X',X):- repeat, %Repite si una columna está llena
		  readColumn(C), play('X',C,X,X2),!,show(X2),nextMove('O',X2). 
	nextMove('O',X):- machine('O','X',X,X2),show(X2),nextMove('X',X2).


### wins(+X,+T)
Indica si el jugador X ha logrado una combinación de 4 en el tablero T

    %Comprueba si hay alguna columna en T con 4 piezas juntas del jugador X
	wins(X,board(T)):- append(_, [C|_], T), % comprueba si hay una columna...
	           append(_,[X,X,X,X|_],C). % ...con 4 piezas del jugador X
	%Comprueba si hay alguna fila en T con 4  piezas conectadas del jugador X      
    wins(X,board(T)):- append(_,[C1,C2,C3,C4|_],T), % Comprueba si exiten 4 columnas pegadas... 
		   append(I1,[X|_],C1), % ...que tienen las fichas del jugador X...
		   append(I2,[X|_],C2),
		   append(I3,[X|_],C3),
		   append(I4,[X|_],C4),
		   length(I1,M), length(I2,M), length(I3,M), length(I4,M). %...y que cada pieza tiene la misma altura
	%Comprueba las diagonales de tipo \ en T con 4 piezas conectadas del jugador X
	wins(X,board(T)):- append(_,[C1,C2,C3,C4|_],T), % comprueba si existen 4 columnas conectadas en el tablero...
		   append(I1,[X|_],C1), %...de forma que todas contengan una pieza de X...
		   append(I2,[X|_],C2),
		   append(I3,[X|_],C3),
		   append(I4,[X|_],C4),
		   length(I1,M1), length(I2,M2), length(I3,M3), length(I4,M4),
		   M2 is M1+1, M3 is M2+1, M4 is M3+1. %...y que cada pieza este en la misma diagonal \ 
	%Comprueba las diagonales de tipo / en T con 4 piezas conectadas del jugador X
	wins(X,board(T)):- append(_,[C1,C2,C3,C4|_],T), % comprueba si existen 4 columnas conectadas en el tablero...
		   append(I1,[X|_],C1), %...de forma que todas contengan una pieza de X...
		   append(I2,[X|_],C2),
		   append(I3,[X|_],C3),
		   append(I4,[X|_],C4),
		   length(I1,M1), length(I2,M2), length(I3,M3), length(I4,M4),
		   M2 is M1-1, M3 is M2-1, M4 is M3-1. %...y que cada pieza este en la misma diagonal /

### full(+T)

Es verdad si el tablero T no tiene ningún espacio jugable, es decir, donde los jugadores puedas colocar ficha

    full(board(T)):- \+ (append(_,[C|_],T), append(_,['-'|_],C)).

### readColumn(-C), associateColumn(+L,-C), associateChar(-L, +C)
Se utilizan para leer las columnas en las que juegan los jugadores.

    %Escribe en pantalla y espera leer un caracter válido
    readColumn(C):- nl, write('Column: '),
		repeat,	get_char(L), associateColumn(L,C), col(C), !.
	%Usamos el corte para evitar la repeticion infinita
	
	%associateColumn(L,C) la columna C esta asociada con el carácter L
	associateColumn(L,C):- atom_codes(L,[La|_]),
		       C is La - 65.

	%associateChar(L, C) El carácter L es el carácter esta asociado con la columna C
	associateChar(L, C):- Ln is 65+C,atom_codes(L,[Ln]).
	
	%valid columns
	col(0).
	col(1).
	col(2).
	col(3).
	col(4).
	col(5).
	col(6).


### machine(+R,+O,+T,-T2)
R es la pieza de la maquina, O la del oponente, T el tablero y T2 unificara si es un tablero que haga que un jugador termine el juego.

    % Gana si es posible
	machine(R,_,T,T2):- iMachine(R,T,C,T2),nl, write('machine: '),associateChar(L,C),write(L), nl,!.
	
	% Si no puede, intenta colocar en un lugar que facilite a la máquina R a ganar y evite que gane el jugador O
	machine(R,O,T,T2):- findall((Col,TA), (col(Col), play(R,Col,T,TA),\+ iMachine(O,TA,_,_), goodMove(R,Col,T)), [(C,T2)|_]), nl, write('machine: '), associateChar(L,C), write(L), nl,!.
	
	% Si el jugador O no esta cerca de ganar, colocará donde facilite que la máquina R conecte 4
	machine(R,O,T,T2):- findall((Col,TA), (col(Col), play(R,Col,T,TA),\+ iMachine(O,TA,_,_)), 
	[(C,T2)|_]),nl, write('machine: '), associateChar(L,C),write(L), nl,write('-'),!.
	
	% Si no, intentará poner en una posible posición que evite una combinación del jugador O
	machine(R,O,T,T2):- iMachine(O,T,C,_),  play(R,C,T,T2),nl, write('machine: '),
		    associateChar(L,C), write(L), nl.
	% Como última opción, colocará en cualquier lugar
	machine(R,_,T,T2):- col(C), play(R,C,T,T2), nl, write('machine: '), associateChar(L,C), write(L), nl.

### iMachine(+R,+C,+T,-T2)

Se satisface si T2 es el resultado de un tablero ganador al poner el jugador R una pieza en la columna C del tablero T.

    iMachine(R,T,C,T2):- findall((Col,TA), (col(Col), play(R,Col,T,TA),wins(R,TA)),[(C,T2)|_]).

### goodMove(+R,-C,+T)
Con la implementación actual, solo consideramos buenos movimientos aquellos que permiten ganar en una columna, no se consideran filas o diagonales.

    goodMove(R,Col,board(T)):- append(I,[C|_],T),length(I,Col), maxConnected(R,C,MaxConn), MaxConn >= 4.	

### maxConnected(+R,+C,-MaxConn)
MaxConn unifica con el nº de piezas del jugador R que estén conectadas en la columna C.

    maxConnected(_,[],0).
	maxConnected(R,[X|_],0):- X\=R.
	maxConnected(R,['-'|X],N):- maxConnected(R,X,Ns),N is Ns+1.
	maxConnected(R,[R|X],N):- maxConnected(R,X,Ns),N is Ns+1.

## Posibles mejoras

### Construir una interfaz gráfica
En lugar de utilizar texto para representar el tablero, crear e implementar una interfaz gráfica que represente el estado del juego en cada instante.

### Mejorar la inteligencia artificial
Actualmente, la máquina contra la que juega el usuario tiene una inteligencia muy simple, por lo que para temas de dificultar al jugador es dificil. Sería conveniente que, para esta parte, se implementase antes qué son buenos movimientos en diagonales y en líneas horizontales.

## Bibliografía

([Guias de prolog](https://www.youtube.com/channel/UCpHSyDp3b5s5TzuKL_OEFvQ))