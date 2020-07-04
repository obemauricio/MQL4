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

## Indicadores

creamos un indicador dando clic derecho sobre el panel de indicadores, luego crear en metaeditor
- onCalculate singinifica extrae los precios de la grafica
- ontimer solo funciona durante una cierta hora
- el indicador puede estar en ventana separada o sobre el grafico
- el indicador puede tener maximos y minimos para dar señales por ejemplo

```c++
//Bars tiene propiedades, el cual indica el simbolo y el periodo: period_curren utiliza el periodo del chart
for(int i = 0; i < Bars(Symbol(),PERIOD_CURRENT); i++){
   
      int tendencia = 0;
      
      if(Close[i] > Open[i]){
         tendencia = tendencia + 1; 
      }
      
      if(Close[i] < Open[i]){
         tendencia = tendencia - 1;
      }
      //iRSI(const string symbol, int timeframe, int period, ENUM_APPIED_PRICE aplied_price, int shift)
      if(iRSI(NULL,0,14,0,i) > 50){ 
         tendencia = tendencia + 1;
      }else{
         tendencia = tendencia - 1;
      }
      if(iClose(NULL,43200,i) > iOpen(NULL,43200,i)){
         tendencia = tendencia + 1;
      }else{
         tendencia = tendencia - 1;
      }
      
      if(iRSI(NULL,1440,2,0,i) > 70){
         tendencia = tendencia + 1;
      }
      if(iRSI(NULL,1440,2,0,i) < 30){
         tendencia = tendencia - 1;
      }
      
      if(iMomentum(NULL,0,10,0,1) > iMomentum(NULL,0,21,0,i)){
         tendencia = tendencia + 1;
      }else{
         tendencia = tendencia - 1;
      }
      
      datosBuffer[i] = tendencia;
      
    }
```
otro indicador

```c++
for(int i=0;i<Bars(Symbol(),PERIOD_CURRENT);i++){
      datoBuffer[i] = MathAbs( iMomentum(NULL,0,10,0,i) - iMomentum(NULL,0,21,0,i) );
   }
```

## Asesor Experto en MQL4
Al abrir un editor de este tipo nos encontramos con tres estructuras en el codigo: 
- int OnInit() lo que hará al inicio
- void OnDeInit(const int reason) lo que hara al terminar la ejecucion
- void OnTick() lo que hara vela por vela
los parametros externos son los inputs: 
- magic number: identificador unico de cada sistema
- SL stop loss
- TP take profit
- lots lotaje
- filter otro parametro que se quiera introducir
- filtera comentario personalizado

primeramente definimos los inputs mas importantes:
```c++
#property copyright "Copyright 2020, MetaQuotes Software Corp."
#property link      "https://www.mql5.com"
#property version   "1.00"
#property strict

input int magic = 10; //siempre es int
input double lots = 0.01; //los lotes son decimales
input double SL = 3; //se calcula de acuerdo a la volatilidad
input double TP = 3; 

// para calcular los TP y los SL normalmente se hace 
// Cierre + (Volatilidad * TP) para un take profit
// Cierre - (Volatilidad * SL) en el caso de una compra 
```
luego definimos las funciones

```c++
void OnDeinit(const int reason)
  {
//---
   
  }
  
  bool compra(){
      if(iRSI(NULL,0,14,0,1)>50 && iMomentum(NULL,0,21,0,1)> 100){
         return True;
      }else{
         return False;
      }
  }
  
  bool venta(){
      if(iRSI(NULL,0,14,0,1)>90 && iMACD(NULL,0,12,26,9,0,0,1)>0){
         return True;
      }else{
         return False;
      }
  
  }
``` 
en void onStick() se debe definir primeramente si existen operaciones abiertas
```c++
void OnTick()
  {
//---
   //Comprobar que no existan operaciones abiertas con el mismo identificador, una operacion por sistema
   bool trade = True;
   //el contador se movera por cada orden abierta
   for(int i=0;OrdersTotal();i++){ 
      //el indice = i porque es lo que esta dando vueltas
      //select by pos = seleccionar por posicion
      //mode_trades = modo operaciones
      //este seleccionara la orden
      int order = OrderSelect(i,SELECT_BY_POS,MODE_TRADES);
      if(OrderMagicNumber()== magic && OrderSymbol() == Symbol()){
         trade = False;
         break; //rompemos el bucle si cumple la condicion
      } 
   }
   
   if(trade == True && Hour()==11){
      if(compra()==True){
         //ordersend= simbolo=actual=Null, tipo de orden =OP buy= compra,volumen=lots, precio= ask(compramos caro vendemos el barato)
         //slippage=10 puntos significa 1 pip, double SL= Open[0]-(iATR(NULL,0,21,1)*SL),Take profit = Open[0]+(iATR(NULL,0,21,1)*TP),
         //comentario = "mi EA de prueba",int magig number = magic, datetime=0 sin explicacion,color arrow= clrGreen) 
         int compra = OrderSend(NULL,OP_BUY,lots,Ask,10,Open[0]-(iATR(NULL,0,21,1)* SL),Open[0]+(iATR(NULL,0,21,1)*TP),"mi primer EA",magic,0,clrGreen);
      }
      if(venta()==True){
         int venta = OrderSend(NULL,OP_SELL,lots,Bid,10,Open[0]+(iATR(NULL,0,21,1)*SL), Open[0]-(iATR(NULL,0,21,1)*TP),"Mi EA de prueba",magic,0,clrGreen);
      }      
   
   }
    //Para hacer una salia manual
   if( trade == False){
      for(int i=0;i<OrdersTotal();i++){
         int os = OrderSelect(i,SELECT_BY_POS,MODE_TRADES);
         if(OrderMagicNumber() == magic && OrderSymbol() == Symbol() && Hour()==12){
            
            if(OrderType() == OP_BUY){
               int cierrelargos = OrderClose(OrderTicket(),OrderLots(),Bid,10,clrAliceBlue);
            }
            if(OrderType() == OP_SELL){
               int cierrecortos = OrderClose(OrderTicket(),OrderLots(),Ask,10,clrAliceBlue);
            }
         }
      }
   }

  }
```
Este codigo nos servira como estructura mas adelante, sin embargo ya se puede probar la estrategia en MT4 y ver sus distintas funcionalidades. 
haciendo algunos ajustes mas, podetemos tener la plantilla que nos servira mas adelante:
```c++
//+------------------------------------------------------------------+
//|                                                 primeraclase.mq4 |
//|                        Copyright 2020, MetaQuotes Software Corp. |
//|                                             https://www.mql5.com |
//+------------------------------------------------------------------+
#property copyright "Copyright 2020, MetaQuotes Software Corp."
#property link      "https://www.mql5.com"
#property version   "1.00"
#property strict

input int magic = 10; //siempre es int
input double lots = 0.01; //los lotes son decimales
input double SL = 3; //se calcula de acuerdo a la volatilidad
input double TP = 3;
input int opt_hora_apertura = 10;
input int opt_numero_velas = 0;
double open = 0;
  

// para calcular los TP y los SL normalmente se hace 
// Cierre + (Volatilidad * TP) para un take profit
// Cierre - (Volatilidad * SL) en el caso de una compra 

//+------------------------------------------------------------------+
//| Expert initialization function                                   |
//+------------------------------------------------------------------+
int OnInit()
  {
//---
   
//---
   return(INIT_SUCCEEDED);
  }
//+------------------------------------------------------------------+
//| Expert deinitialization function                                 |
//+------------------------------------------------------------------+
void OnDeinit(const int reason)
  {
//---
   
  }
  
  bool compra(){
      if(1==1){
         return True;
      }else{
         return False;
      }
  }
  
  bool venta(){
      if(1==1){
         return True;
      }else{
         return False;
      }
  
  }
  
//+------------------------------------------------------------------+
//| Expert tick function                                             |
//+------------------------------------------------------------------+
void OnTick()
  {
//---
   //Comprobar que no existan operaciones abiertas con el mismo identificador, una operacion por sistema
   bool trade = True;
   //el contador se movera por cada orden abierta
   for(int i=0;OrdersTotal();i++){ 
      //el indice = i porque es lo que esta dando vueltas
      //select by pos = seleccionar por posicion
      //mode_trades = modo operaciones
      //este seleccionara la orden
      int order = OrderSelect(i,SELECT_BY_POS,MODE_TRADES);
      if(OrderMagicNumber()== magic && OrderSymbol() == Symbol()){
         trade = False;
         break; //rompemos el bucle si cumple la condicion
      } 
   }
   
   if(trade == True && Hour()==opt_hora_apertura){
      if(compra()==True){
         //ordersend= simbolo=actual=Null, tipo de orden =OP buy= compra,volumen=lots, precio= ask(compramos caro vendemos el barato)
         //slippage=10 puntos significa 1 pip, double SL= Open[0]-(iATR(NULL,0,21,1)*SL),Take profit = Open[0]+(iATR(NULL,0,21,1)*TP),
         //comentario = "mi EA de prueba",int magig number = magic, datetime=0 sin explicacion,color arrow= clrGreen) 
         int compra = OrderSend(NULL,OP_BUY,lots,Ask,10,Open[0]-(iATR(NULL,0,21,1)* SL),Open[0]+(iATR(NULL,0,21,1)*TP),"mi primer EA",magic,0,clrGreen);
         open = Open[0];
      }
      if(venta()==True){
         int venta = OrderSend(NULL,OP_SELL,lots,Bid,10,Open[0]+(iATR(NULL,0,21,1)*SL), Open[0]-(iATR(NULL,0,21,1)*TP),"Mi EA de prueba",magic,0,clrGreen);
         open = Open[0];
      }      
   
   }
   
   if( trade == False){
      for(int i=0;i<OrdersTotal();i++){
         int os = OrderSelect(i,SELECT_BY_POS,MODE_TRADES);
         if(OrderMagicNumber() == magic && OrderSymbol() == Symbol() && Open[opt_numero_velas]==open){
            
            if(OrderType() == OP_BUY){
               int cierrelargos = OrderClose(OrderTicket(),OrderLots(),Bid,10,clrAliceBlue);
            }
            if(OrderType() == OP_SELL){
               int cierrecortos = OrderClose(OrderTicket(),OrderLots(),Ask,10,clrAliceBlue);
            }
         }
      }
   }

  }
//+------------------------------------------------------------------+
```
