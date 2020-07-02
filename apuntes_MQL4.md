## Variables
son espacion reservados en la memoria que, como su nombre lo indica, pueden cambiar de contenido a lo largo de la ejecucion de un programa, es decir es un dato que guardamos para usar en nuestro algoritmo.
por ejemplo: 
- El valor del MACD, el RSI, la fecha, etc. 
algunos datos como las velas nos lo proporciona el MT4, otros datos los podemos crear nosotros, como por ejemplo: la variacion entre maximo y minimo de una vela.
cada variable tiene un tipo de dato: 
 - Int o numero entero
 - Double o numero decimal
 - Bool verdadero o falso 
 - Datatime o fecha

## condicionales

conjunto de intrucciones que se ejecutan si se cumple una condicion. En nuestra vida diaria usamos este tipo de situaciones. Si(un ordenador esta roto){lo reparo;}

## bucles 

es una sentencia que ejecuta repetidas veces un trozo de codigo, hasta que la condicion asignada a dicho bucle deja de cumplirse. es decir, un conjunto de intrucciones que se repiten durante un tiempo determinado.
En el trading seria yo quiero que se repita estra intruccion durante 50 velas o quiero que se repita mientras una variable en una condicional sea cierto, por ejemplo: mientras una vela sea bajista se active una condicion.

## ejemplos basicos
```c++
void OnStart() //funcion predeterminada, indica que el script se ejecutara en la grafica

    {
        int resultado = 0;
        //int i es un contador, mientras i sea menor al numero de velas, aumentar 1 al contador cada vez que se ejecute
        //time es la posicion del bucle, la posicion esta entre [] la posicion actual es [0], posicion anterior [1], ...., [n]
        for(int i = 0; i < 50; i++){ 
            resultados++;
        }
    Alert("numero de velas alcistas en las ultimas 50 es: " + resultado);
    }   
```
```c++
void OnStart()
    {
        int resultado = 0;
        for(int i = 0; i < 50; i++){
            resultado++;
        }
    Alert("numero de velas alcistas con cierres mayores que el maximo anterior: " + resultado);
    }
```

```c++
//void no guarda nada., una variable o un valor.
void onStart() //funcion predeterminada, indica que el script se ejecutara en la grafica

{
    int velastotales = 0;
    int velasalcistas = 0; 
    int velasbajistas = 0;
    //bucle, bars = numero de velas 
    for(int i = 0; i < Bars; i++){
        //TimeMonth es el tiempo del mes 
        if( TimeMonth(Time[i]) == 11 ){
            velastotales = velastotales +1;
            if(Close[i] < Open[i]){
                velasalcistas++;
            }
            if(Close[i] > Open[i]){
                velasbajistas++;
            }
        }
    }
    Alert("Velas totales: " + velastotales);
    Alert("Velas alcistas: " + velasalcistas);
    Alert("Velas bajistas: " + velasbajistas);
}
```
```c++
void OnStart()
   {
   
      int resultado_a = 0;
      int resultado_b = 0;
      //iRSI es un indicador, tiene parametros para su respectiva configuracion
      for(int i = 0; i < 50; i++){
         if(iRSI(NULL,PERIOD_CURRENT,14,PRICE_CLOSE,i) > 70){
            resultado_a++;
         }else{
            resultado_b++;
         }
      }
      Alert("RSI > 70 " + resultado_a);
      Alert("RSI < 70 " + resultado_b);     
   }
```


## Rally de navidad

Existe una hipotesis llamado el Rally de Navidad, el cual tiene la creencia de que el mercado sera alcista con mayor retorno del normal durante el mes de diciembre. 
el planteamineto es el siguiente:
- paso 1: Obtener el retorno medio mensual
- paso 2: Comparar el retorno de Diciembre con la media
obteniendo el retorno medio:
Rentabilidad_total(%)/numero de velas
Mientras(velas){
    Rentabilidad_total = cierre - open + rentabilidad_total;
    Numero_de_velas++;
}
Existe rally de diciembre? comprobacion de la hipotesis
Si(retorno_diciembre > retorno_medio){
    hay rally
}si no{
    no hay rally
}

```c++
void OnStart()
  {
    Alert("Simbolo: " + Symbol());
    double retorno_medio_mes = 0;
    double retorno = 0;
    int contador = 0;
    for(int i=0; i < Bars; i++){
        retorno = ((Close[i] * 100) / Open[i]) - 100;
        retorno_medio_mes = retorno_medio_mes + retorno;
        contador++; 
    }
    
    retorno_medio_mes = (retorno_medio_mes / contador);
    int rally = 0;
    int total = 0;
    for(int i = 0; i < Bars; i++){ 
        retorno = ((Close[i] * 100) / Open[i]) - 100;
        if(TimeMonth(Time[i]) == 12){
            total++;
            retorno = ((Close[i] * 100) / Open[i]) - 100;
                (Close[i] * 100 / Open[i]) - 100;
                if(retorno > retorno_medio_mes){
                    Alert("Año con Rally: " + TimeYear(Time[i]) + " - Retorno: " + retorno + " - Media: " + retorno_medio_mes);
                    rally++;
                }
                else
                    Alert("año sin rally: " + TimeYear(Time[i]) + " - Retorno: " + retorno + " - Media: " + retorno_medio_mes);
                    
        }
    }
    Alert("Años con rally: " + rally + " Años totales: " + total);  
  }
```
