# 🚗 Arduino RC Car Project
## 📌 Projekti idee ja eesmärk

Selle projekti eesmärk on luua Arduino baasil juhitav auto, mida saab juhtida raadiosaatja (puldiga).
Eesmärgid:

* õppida Arduino kasutamist

* ühendada mootorid läbi draiveri

* lugeda signaale vastuvõtjalt

* realiseerida auto liikumise juhtimine

## 🤖 Roboti kirjeldus ja tööpõhimõte

Robot on kahe rattaga auto, mida juhitakse FlySky puldiga.
Tööpõhimõte:

* Pult saadab signaale (kanalid CH1 ja CH3)

* Vastuvõtja võtab signaalid vastu

* Arduino loeb signaali funktsiooniga pulseIn()

* Signaal teisendatakse kiiruseks ja suunaks

* L298N mootoridraiver juhib mootoreid

## ⚙️ Ühendusskeem


Peamised ühendused:

* Vastuvõtja → Arduino

* CH1 (rool) → A0

* CH3 (gaas) → A1

* VCC → 5V

* GND → GND


Mootoridraiver L298N

* ENA → pin 5

* IN1 → pin 2

* IN2 → pin 3

* ENB → pin 6

* IN3 → pin 4

* IN4 → pin 7

## 🔧 Kasutatud komponendid

* Arduino UNO

* L298N mootoridraiver

* 2 DC mootorit

* FlySky FS-i6 pult

* Vastuvõtja FS-iA6B

* Aku (toiteallikas)

* Juhtmed

## 💻 Programmikood
```
// Arduino RC auto juhtimine

int enA = 5;
int in1 = 2;
int in2 = 3;

int enB = 6;
int in3 = 4;
int in4 = 7;

int ch1Pin = A0; // rool
int ch3Pin = A1; // gaas

void setup() {
  pinMode(enA, OUTPUT);
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);

  pinMode(enB, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);

  Serial.begin(115200);
}

void loop() {
  int ch1 = pulseIn(ch1Pin, HIGH, 30000);
  int ch3 = pulseIn(ch3Pin, HIGH, 30000);

  int steering = map(ch1, 1000, 2000, -255, 255);
  int throttle = map(ch3, 1000, 2000, -255, 255);

  int leftMotor  = throttle + steering;
  int rightMotor = throttle - steering;

  leftMotor  = constrain(leftMotor, -255, 255);
  rightMotor = constrain(rightMotor, -255, 255);

  setMotor(enA, in1, in2, leftMotor);
  setMotor(enB, in3, in4, rightMotor);
}

void setMotor(int en, int in1, int in2, int speed) {
  if (speed > 0) {
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
    analogWrite(en, speed);
  } else if (speed < 0) {
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
    analogWrite(en, -speed);
  } else {
    digitalWrite(in1, LOW);
    digitalWrite(in2, LOW);
    analogWrite(en, 0);
  }
}
```

## 📸 Projekti fotod ja videod

[Vaata video](/VideoProject1-ezgif.com-video-to-gif-converter.gif)



## ⚠️ Probleemid ja lahendused

Projekti käigus tekkisid järgmised probleemid:

Üks mootor ei töötanud → vale ühendus

Vastuvõtja ei saatnud signaali → vale pin

CH3 väärtus oli 0 → halb kontakt

Auto liikus ise → vale signaali keskpunkt

Lahendused:

juhtmete kontrollimine

Serial Monitori kasutamine

ühenduste parandamine

## 🎯 Kokkuvõte

Selle projekti käigus õppisin:

* Arduino kasutamist

* mootorite juhtimist

* raadiosignaalide lugemist

* vigade leidmist ja parandamist

* roboti ehitamist

## 🚀 Tulemus

Projekt on edukalt valmis. Auto suudab:

* liikuda edasi ja tagasi

* pöörata vasakule ja paremale

* peatuda
