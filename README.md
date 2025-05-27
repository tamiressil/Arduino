# Código original:

const int8_t mDirF = 5, mDirT = 6, // declara o pino
 mEsqF = 10, mEsqT = 11, // associado a
 sDir = A0, sEsq = A1; // cada sinal
void setup() {
 pinMode(sDir, INPUT); // configura os pinos
 pinMode(sEsq, INPUT); // como entradas
}
void loop() {
 static const uint8_t vel = 100; // altere a velocidade conforme sua pista
                                 // curvas fechadas exigem velocidades menores
 analogWrite(mEsqF, vel * !digitalRead(sEsq)); // o motor esquerdo avança enquanto o
                                               // sensor esquerdo não detectar uma linha
 analogWrite(mEsqT, vel * digitalRead(sEsq)); // quando detecta, dá ré
 analogWrite(mDirF, vel * !digitalRead(sDir)); // o motor direito avança enquanto o
                                               // sensor direito não detectar uma linha
 analogWrite(mDirT, vel * digitalRead(sDir)); // quando detecta, dá ré
}


# Arduino código att as rodas girando para o mesmo sentido, com a velocidade alta para testagem

const int8_t mDirF = 5, mDirT = 6;     // Motor Direito Frente e Trás
const int8_t mEsqF = 10, mEsqT = 11;   // Motor Esquerdo Frente e Trás
const int8_t sDir = A0, sEsq = A1;     // Sensores IR Direito e Esquerdo

void setup() {
  Serial.begin(9600);

  pinMode(sDir, INPUT);
  pinMode(sEsq, INPUT);

  pinMode(mDirF, OUTPUT);
  pinMode(mDirT, OUTPUT);
  pinMode(mEsqF, OUTPUT);
  pinMode(mEsqT, OUTPUT);
}

void loop() {
  int leituraEsq = digitalRead(sEsq);
  int leituraDir = digitalRead(sDir);

  Serial.print("Sensor Esq: ");
  Serial.print(leituraEsq);
  Serial.print(" | Sensor Dir: ");
  Serial.println(leituraDir);

  bool esquerdaNaLinha = leituraEsq == LOW;
  bool direitaNaLinha = leituraDir == LOW;

  static const uint8_t vel = 150;

  if (!esquerdaNaLinha && !direitaNaLinha) {
    // Seguir reto
    motorEsquerdoFrente(vel);
    motorDireitoFrente(vel);
  } 
  else if (esquerdaNaLinha && !direitaNaLinha) {
    // Virar para esquerda
    pararEsquerdo();
    motorDireitoFrente(vel);
  } 
  else if (!esquerdaNaLinha && direitaNaLinha) {
    // Virar para direita
    motorEsquerdoFrente(vel);
    pararDireito();
  } 
  else {
    // Cruzamento
    motorEsquerdoFrente(vel / 2);
    motorDireitoFrente(vel / 2);
  }

  delay(100);
}

// Inverte lógica do motor esquerdo
void motorEsquerdoFrente(uint8_t v) {
  analogWrite(mEsqF, 0);
  analogWrite(mEsqT, v); // invertido!
}

void motorDireitoFrente(uint8_t v) {
  analogWrite(mDirF, v);
  analogWrite(mDirT, 0);
}

void pararEsquerdo() {
  analogWrite(mEsqF, 0);
  analogWrite(mEsqT, 0);
}

void pararDireito() {
  analogWrite(mDirF, 0);
  analogWrite(mDirT, 0);
}


# Código do kaique:

const int8_t mDirF = 5, mDirT = 6, 
             mEsqF = 10, mEsqT = 11, 
             sDir = A0, sEsq = A1;

void setup() {
  pinMode(sDir, INPUT);
  pinMode(sEsq, INPUT);
}

void loop() {
  static const uint8_t vel = 100;

  // Motor esquerdo - INVERTIDO
  analogWrite(mEsqF, vel * digitalRead(sEsq));    // antes era com "!"
  analogWrite(mEsqT, vel * !digitalRead(sEsq));   // antes era sem "!"

  // Motor direito - mantÃ©m igual
  analogWrite(mDirF, vel * !digitalRead(sDir)); 
  analogWrite(mDirT, vel * digitalRead(sDir));  
}





# Do site:

const int8_t mDirF = 5, mDirT = 6, // declara o pino
 mEsqF = 10, mEsqT = 11, // associado a
 sDir = A0, sEsq = A1; // cada sinal
void setup() {
 pinMode(sDir, INPUT); // configura os pinos
 pinMode(sEsq, INPUT); // como entradas
}
void loop() {
 static const uint8_t vel = 100; // altere a velocidade conforme sua pista
                                 // curvas fechadas exigem velocidades menores
 analogWrite(mEsqF, vel * !digitalRead(sEsq)); // o motor esquerdo avança enquanto o
                                               // sensor esquerdo não detectar uma linha
 analogWrite(mEsqT, vel * digitalRead(sEsq));  // quando detecta, dá ré
 analogWrite(mDirF, vel * !digitalRead(sDir)); // o motor direito avança enquanto o
                                               // sensor direito não detectar uma linha
 analogWrite(mDirT, vel * digitalRead(sDir));  // quando detecta, dá ré
}


# Do site mas as rodas girando no mesmo sentindo: 

const int8_t mDirF = 5, mDirT = 6,  // Motor Direito Frente e Trás
             mEsqF = 10, mEsqT = 11, // Motor Esquerdo Frente e Trás
             sDir = A0, sEsq = A1;   // Sensores

void setup() {
  pinMode(sDir, INPUT);
  pinMode(sEsq, INPUT);

  pinMode(mDirF, OUTPUT);
  pinMode(mDirT, OUTPUT);
  pinMode(mEsqF, OUTPUT);
  pinMode(mEsqT, OUTPUT);
}

void loop() {
  static const uint8_t vel = 100;

  // Sensor esquerdo: quando detecta preto (LOW), inverter lógica do motor
  analogWrite(mEsqF, vel * digitalRead(sEsq));   // antes era com "!"
  analogWrite(mEsqT, vel * !digitalRead(sEsq));  // antes era sem "!"

  // Sensor direito permanece como está
  analogWrite(mDirF, vel * !digitalRead(sDir));
  analogWrite(mDirT, vel * digitalRead(sDir));
}


# Testando o motor :

void setup() {
  pinMode(mEsqF, OUTPUT);
  pinMode(mEsqT, OUTPUT);
}

void loop() {
  analogWrite(mEsqF, 100);
  analogWrite(mEsqT, 0);
  delay(2000);

  analogWrite(mEsqF, 0);
  analogWrite(mEsqT, 100);
  delay(2000);
}

# Testando a fonte H : 
const int8_t mDirF = 5, mDirT = 6; // Motor Direito Frente e Trás
const int8_t mEsqF = 10, mEsqT = 11; // Motor Esquerdo Frente e Trás

void setup() {
  pinMode(mDirF, OUTPUT);
  pinMode(mDirT, OUTPUT);
  pinMode(mEsqF, OUTPUT);
  pinMode(mEsqT, OUTPUT);
}

void loop() {
  // Teste Motor Esquerdo - Frente
  analogWrite(mEsqF, 150);
  analogWrite(mEsqT, 0);
  delay(2000);

  // Teste Motor Esquerdo - Ré
  analogWrite(mEsqF, 0);
  analogWrite(mEsqT, 150);
  delay(2000);

  // Teste Motor Direito - Frente
  analogWrite(mDirF, 150);
  analogWrite(mDirT, 0);
  delay(2000);

  // Teste Motor Direito - Ré
  analogWrite(mDirF, 0);
  analogWrite(mDirT, 150);
  delay(2000);

  // Para tudo
  analogWrite(mEsqF, 0);
  analogWrite(mEsqT, 0);
  analogWrite(mDirF, 0);
  analogWrite(mDirT, 0);
  delay(2000);
}

# Agora

// Declaração dos pinos dos motores e sensores
const int8_t mDirF = 5, mDirT = 6;   // Motor direito: frente e trás
const int8_t mEsqF = 10, mEsqT = 11; // Motor esquerdo: frente e trás
const int8_t sDir = A0, sEsq = A1;   // Sensores direito e esquerdo

void setup() {
  // Configura os sensores como entrada
  pinMode(sDir, INPUT);
  pinMode(sEsq, INPUT);

  // Configura os pinos dos motores como saída
  pinMode(mDirF, OUTPUT);
  pinMode(mDirT, OUTPUT); 
  pinMode(mEsqF, OUTPUT);
  pinMode(mEsqT, OUTPUT);
}

void loop() {
  static const uint8_t vel = 100; // Velocidade dos motores (ajuste conforme necessário)

  // Controle do motor esquerdo
  analogWrite(mEsqF, vel * !digitalRead(sEsq)); // Avança se não detectar linha
  analogWrite(mEsqT, vel * digitalRead(sEsq));  // Ré se detectar linha

  // Controle do motor direito
  analogWrite(mDirF, vel * !digitalRead(sDir)); // Avança se não detectar linha
  analogWrite(mDirT, vel * digitalRead(sDir));  // Ré se detectar linha
}

