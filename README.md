# SPD 
![tinkercad_presentacion](https://github.com/vpjm95/vpjm95/assets/57650096/0738de76-6c21-4280-8aa6-a25ad4c629e8)

## Integrantes 
- Thomas Rodriguez
- Jose Manuel Vlillegas
- Mariana M 
##  proyecto : contador Display 7 segmentos
![tinkercad_parte_1](https://github.com/vpjm95/vpjm95/assets/57650096/d94ee336-5bd7-4b9d-b31b-b03c01de114f)
 
### Descripcion 
- una placa arduina conectado a dos display de siete segmentos que es controlado por tres botones, uno permite aumentar el contador de a un digito del 0 al 99
  el segundo al presionarlo disminuye el conteo y el tercero reinicia el display.

  
## proyecto : contador Display 7 segmentos parte 2 
![tinkercad](https://github.com/vpjm95/vpjm95/assets/57650096/80ce0063-34c6-471e-a25c-2c24da466afa)

### Descripcion 

- Una placa arduina conectada a dos display de siete segmentos y a dos botones , un interruptor deslizante (switch ) de dos posiciones dependiendo de la posicion del interruptor podemos mostrar un contador del 0 al 99 o numeros primos del 0 al 99. y agregamos un motor de aficionado este puede convertir la energia electrica en energia mecanica dando uso de muchas maneras.

 ## Función principal 
 A 10 ,B 11, C 5 , D 6 ,E 7 ,G 8, F 9 , UNIDAD A4, DECENA A5, OFF 0, SUM_BUTTON 4,SUBS_BUTTON 3 ,RESET_BUTTON 2 , SWITCH 2: son  #define que utilizamos para agregar los leds, asociandolo a pines de la placa arduino.
 ~~~ C
 void setup()
{
  for (int i = 5; i <= 11; i++)
    pinMode(i, OUTPUT);
  pinMode(SUM_BUTTON, INPUT_PULLUP);
  pinMode(SUBS_BUTTON, INPUT_PULLUP);

  pinMode(SWITCH, INPUT_PULLUP);
  
  pinMode(UNIDAD, OUTPUT);
  pinMode(DECENA, OUTPUT);
  
  pinMode(12, OUTPUT);
  pinMode(13, OUTPUT);
  
  digitalWrite(UNIDAD, 0);
  digitalWrite(DECENA, 0);
  showDisplay(0);
}
~~~ 
En la  función void setup :se  inicializa los pines de la placa Arduino para que puedan ser utilizados como entradas o salidas.
~~~ C
int click(void)
  
  int Sum = digitalRead(SUM_BUTTON);
  int Substraction = digitalRead(SUBS_BUTTON);
  
  if (Sum == 1)
  {
    previousSum = 1;
  }
  if (Substraction == 1)
  {
    previousSubs = 1;
  }
  if (Sum == 0 && Sum != previousSum)
  {
  	previousSum = Sum;
    return SUM_BUTTON;
  }
  if (Substraction == 0 && Substraction != previousSubs)
  {
  	previousSubs = Substraction;
    return SUBS_BUTTON;
  }
  
  return -1;
}

{
~~~ 
~~~ C
void increment()
{
  if (number < 99)
  {
    number ++;
  }
  else
  {
    number = 0;
  }
}

void decrease()
{  
  if (number > 0)
  {
    number --;
  }
  else
  {
    number = 99;
  }  
}

bool primeNumbers(int number)
{
  if (number <= 1) 
  {
    return false;
  }
  for (int i = 2; i * i <= number; i++) 
  {
    if (number % i == 0)
    {
      return false;
    }
  }
  return true;
}
~~~ 
  /*Funcion especifica para ver si el boton fue presionado una vez,
  Que no cuenten por mas si se mantiene presionado,
  y devuelva el valor BUTTON para la funcion loop()*/

~~~ C
void loop()
{
  digitalWrite(13,1);
  /*Suma, Resta o resetea el valor Nummero que va a mostrar,
  si pasa de 0 a menos, da la vuelta a 99, y caso contrario
  quere sumar mas de 99.
  Al final, llama a la funcion printNumber para calcular
  su posicion.*/


  int Switch = digitalRead(SWITCH);

  if (Switch == HIGH) 
  {
    int readClick = click();
    if (readClick == SUM_BUTTON) 
    {
      increment();
    } 
    else if (readClick == SUBS_BUTTON)
    {
      decrease();
    }
    printNumber(number);
  } 
  else if (Switch == LOW)
  {
    if (!primeNumbers(number)) 
    {
      do 
      {
        number++;
      } while (!primeNumbers(number));
    }
    printNumber(number);
  }
}

~~~ C
void turnDigit(int digit)
{
  //Seleciona cual UNIDAD o DIGITO va a ser mostrada
  if (digit == UNIDAD)
  {
    digitalWrite(UNIDAD, LOW);
    digitalWrite(DECENA, HIGH);
    delay(TIMEDELAY);
    //delay da tiempo para que sea visible el display
  }
  else if (digit == DECENA)
  {
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, LOW);
    delay(TIMEDELAY);
  }
  else
  {
    //Los apaga a ambos
    digitalWrite(UNIDAD, HIGH);
    digitalWrite(DECENA, HIGH);
  }
}
~~~
~~~ C
void printNumber(int number)
{
  //Muestra los dos numeros en el display
  turnDigit(OFF);
  //Muestra ambos display para apagarlos
  showDisplay(number / 10);
  turnDigit(UNIDAD);
  //Llama a la funcion para ver que caso encender la unidad
  turnDigit(OFF);
  showDisplay(number - 10*((int)number / 10));
  /* Esta cuenta da la unidad y decimal.
EJ, si number es 23, hacemos 23/10, da 2,3 -> que se convierte en 2.
El 2 se multiplica por 10, dando el Decimal: 2 Finalmente, se resta 23(number)
menos el resultado 20, dando la Unidad: 3*/
  turnDigit(DECENA);
  //Llama a la funcion para ver que caso encender la decena
}


~~~ C

void showDisplay(int number)
{
  //Enciende los displays entre unidad y decimal
  turnOffDisplay();
  
	switch(number % 10)
  {
    case 0:
    	digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
    	digitalWrite(C, LOW);
    	digitalWrite(D, LOW);
    	digitalWrite(E, LOW);
    	digitalWrite(F, LOW);
    	break;
    case 1:
    	digitalWrite(B, LOW);
    	digitalWrite(C, LOW);
    	break;
    case 2:
    	digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
    	digitalWrite(D, LOW);
    	digitalWrite(E, LOW);
    	digitalWrite(G, LOW);
    	break;
    case 3:
    	digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
    	digitalWrite(C, LOW);
    	digitalWrite(D, LOW);
    	digitalWrite(G, LOW);
    	break;
    case 4:
    	digitalWrite(B, LOW);
       	digitalWrite(C, LOW);
       	digitalWrite(F, LOW);
        digitalWrite(G, LOW);
    	break;
    case 5:
    	digitalWrite(A, LOW);
        digitalWrite(C, LOW);
        digitalWrite(D, LOW);
        digitalWrite(F, LOW);
        digitalWrite(G, LOW);
    	break;
    case 6:
    	digitalWrite(A, LOW);
        digitalWrite(C, LOW);
        digitalWrite(D, LOW);
    	digitalWrite(E, LOW);
        digitalWrite(F, LOW);
        digitalWrite(G, LOW);
    	break;
    case 7:
        digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
    	digitalWrite(C, LOW);
    	break;
    case 8:
        digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
    	digitalWrite(C, LOW);
    	digitalWrite(D, LOW);
    	digitalWrite(E, LOW);
    	digitalWrite(F, LOW);
    	digitalWrite(G, LOW);
    	break;
    case 9:
    	digitalWrite(A, LOW);
    	digitalWrite(B, LOW);
    	digitalWrite(C, LOW);
    	digitalWrite(D, LOW);
    	digitalWrite(F, LOW);
    	digitalWrite(G, LOW);
    	break;
    }
}  
~~~ 
~~~ C
  
void turnOffDisplay()
{
  //Apaga los displays que no estan en uso para que no quede residuo
  digitalWrite(A, HIGH);
  digitalWrite(B, HIGH);
  digitalWrite(C, HIGH);
  digitalWrite(D, HIGH);
  digitalWrite(E, HIGH);
  digitalWrite(F, HIGH);
  digitalWrite(G, HIGH);
}
  ~~~ 
##  Link al proyecto
Parte 1 
https://www.tinkercad.com/things/ilaSsHgMmkz-bodacious-habbi-densor/editel

parte 2 
  https://www.tinkercad.com/things/iQh2glDjtYk?sharecode=0d5E5kLgmVP71zSz8f1w6UXAddd7WWGimW6Ec6a1AXQ
## link al video del proceso 
  https://youtu.be/srCOkz9Xgco?si=So-sDlO4tI2P2_uc
  

  




















