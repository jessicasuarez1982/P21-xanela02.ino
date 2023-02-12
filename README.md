/*
PRÁCTICA 21 MOTOR CON INVERSIÓN DE XIRO PASANDO POR PARO.
Programa para simular unha ventá dun coche, de maneira
simplificada. O pulsador acciona o motor de subida ó ser premido, se o volvo premer
pasa por paro e coa 3ª pulsación invirte o xiro.
Mentras os motores están accionados deben
responder ó accionamento do pulsador (responsive).

Entradas: Pulsador (dixital)
Saida: Relé (2x dixital)

Autor: Jéssica Suárez Parada
Data:12/02/2023
*/

#define motorArriba 12
#define motorAbaixo 11
#define pulsador 7
int estado = 1;      //Toma valores 0,1,2,3. En 0 sube en 2 baixa, en 1 e 3 para.
int contador = 0; //Contador co numero de impulsos do motor

void setup(){
  pinMode(motorArriba, OUTPUT);
  pinMode(motorAbaixo,OUTPUT);
  pinMode(pulsador,INPUT);
  
  Serial.begin(9600);  //Velocidade de sincronismo das comunicacións
}

void loop() {
  //Lectura pulsador
  if(digitalRead(pulsador)) {  
    estado = (++estado) % 4; //carga o valor do resto da division /3. ex: 1/3, resto =1, este é o estado que toma como lectura o pulsador 
//   estado++ ;asigna e despois actualiza. ++estado; actualiza e logo designa.
//   if(estado > 3) {  //cando o estado é maior que 2,o orde da suma e da división.
//   estado = 0;     //volve ó estado 0
//   }

    contador = 100;
    while(digitalRead(pulsador)) {
      delay(20);
    }
  }
  //Fin da lectura do pulsador
  
  Serial.print("Valor do contador: ");
  Serial.println(contador);
  Serial.print("  | Estado: ");
  Serial.println(estado);
  
  //Accionamento dos motores
  if(contador > 0) {         //Conta o número de impulsos do motor
    if(estado == 0){
    digitalWrite(motorArriba, HIGH);
    digitalWrite(motorAbaixo, LOW);
    delay(70);
    contador--;
  }
  else if(estado==2) {
    digitalWrite(motorArriba, LOW);
    digitalWrite(motorAbaixo, HIGH);
    delay(70);
    contador--;
  }
    else {
    digitalWrite(motorArriba, LOW);
    digitalWrite(motorAbaixo, LOW);
    delay(70);
    contador--;  
    }
  }
  else {      //Se o motor está parado , lee o pulsador 10 veces/seg.
    digitalWrite(motorArriba, LOW);
    digitalWrite(motorAbaixo, LOW);
    delay(100);
  }
  //Fin do accionamento dos motores
  //delay (100)
