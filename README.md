# SlapBot

SlapBot és un robot físic capaç de jugar al joc de cartes **El Tapete** de manera autònoma.  
Combina visió per computador, control de motors i sensors per reconèixer cartes, prendre decisions i competir contra jugadors humans en temps real.

---

## 🧠 Descripció general

El robot es compon de quatre mòduls principals:

- **Mecanisme de "slap"**: braç que colpeja la taula quan detecta una combinació guanyadora.
- **Llença-cartes**: mecanisme que permet al robot descartar cartes durant la partida.
- **Sistema de visió**: càmera connectada a una Raspberry Pi que reconeix les cartes jugades mitjançant un model de Roboflow.
- **Sensors de detecció de guanyador**: dos sensors d’ultrasons que detecten qui ha picat primer a la taula.

El reconeixement de cartes es fa mitjançant una **API de Roboflow**, integrada al codi de `src/raspberry_pi4`, que processa les imatges captades per la càmera i retorna el valor de la carta al programa principal.  
Inicialment s’havia creat una xarxa neuronal pròpia (`src/neural_network`), però tot i obtenir una bona precisió en entrenament (>0.9), no donava resultats fiables en temps real, per la qual cosa es va optar per utilitzar Roboflow.

---

## 🎥 Vídeo de demostració

[![Veure vídeo](https://img.youtube.com/vi/LgzfWQ4xZNc/0.jpg)](https://youtu.be/LgzfWQ4xZNc)

---

## ⚙️ Tecnologies utilitzades

**Software**
- Python 3 (Raspberry Pi 4)
- C++ (Arduino Nano)

**API**
- Projecte de Roboflow per a detecció de cartes  
  (vegeu `src/raspberry_pi4/execute_program_and_cite_project_api_roboflow`)

**Hardware**
- Raspberry Pi 4  
- Càmera Eye PlayStation 3  
- Arduino Nano  
- 2 sensors d’ultrasons HC-SR04  
- Motor DC i mini motor DC  
- Controladora TB6612FNG per a motors  
- Altaveu  
- Font d’alimentació de 6 piles (7,2 V a 9 V)

---

## 🧩 Arquitectura

**Esquema de hardware**

![Esquema de hardware](https://github.com/1606206/SlapBot_RLP/blob/main/Hardware/esquema_hardware_slapbot.png)

**Diagrama de software**

![Diagrama de software](https://github.com/1606206/SlapBot_RLP/blob/main/src/diagrama_software.jpg)

---

## 📁 Estructura del codi

- **`src/arduino`**  
  Controla els sensors i motors. L’Arduino detecta qui pica primer a la taula (robot o jugadors humans) i envia aquesta informació a la Raspberry Pi. També activa el mecanisme del braç i el llença-cartes quan cal.

- **`src/neural_network`**  
  Conté el codi i el model entrenat per al reconeixement de cartes mitjançant xarxes neuronals. Finalment no es va utilitzar en el sistema final, però s’inclou com a referència i documentació del procés.

- **`src/raspberry_pi4`**  
  Implementa el programa principal, la gestió de torns i la comunicació amb l’Arduino, la càmera i l’API de Roboflow. Inclou instruccions d’execució al fitxer `explicacio.md`.

---

## 🃏 Funcionament del joc

- La partida comença sempre amb el robot per sincronitzar el sistema.  
- Durant el seu torn, el robot llença una carta. Si ha de jugar més d’una, espera a detectar l’última carta llençada abans de continuar.  
- Durant el torn dels jugadors humans, el robot reconeix les cartes jugades amb la càmera i el sistema de visió.  
- Seguint les normes d’**El Tapete**, el robot pica a la taula quan detecta:
  - dues cartes consecutives del mateix valor
  - un “sandvitx” (dues cartes iguals separades per una diferent)
- Els jugadors han de picar entre el sensor i la pila de cartes; si ho fan abans que el robot, se’n porten les cartes.
- Després de cada ronda, si el robot guanya, s’ha de **recarregar el dispensador de cartes manualment** durant una pausa de 20 segons.
- El jugador que acabi amb totes les cartes és el guanyador.

---

## 👥 Autors

- Pol Reyes Martí  
- Eric Rodríguez Merichal 
- Valentí Torrents Vila 
- Guillermo Vivancos Alonso
