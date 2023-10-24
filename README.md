# repc2002.github.io

![ITT Logo](ITT.jpg)
![Actividad-1](https://github.com/REPC2002/repc2002.github.io/blob/main/README.md)

## KeyPad

![Keypad Image](https://www.electronicwings.com/storage/PlatformSection/TopicContent/125/description/4x4%20Keypad.png)

### Características:
- **Matriz de 16 Botones:** Organización matricial (4 filas x 4 columnas)
- **Tipo de Teclado:** Membrana
- **Resistencia:** Mayor resistencia al agua y al polvo
- **Autoadhesivo:** Parte trasera autoadhesiva
- **Tiempo de Rebote:** ≤5 ms
- **Voltaje Operativo Máximo:** 24 V DC
- **Corriente Operativa Máxima:** 30 mA
- **Resistencia de Aislamiento:** 100 MΩ (@ 100 V)
- **Voltaje Dieléctrico:** 250 VRMS (@ 60Hz, por 1 min)
- **Expectativa de Vida:** 1.000.000 de operaciones
- **Dimensiones del Pad:** 6.9 x 7.7 cm (aproximadamente)
- **Cable de Cinta Plana:** 8.3 cm de largo (aproximadamente, incluido el conector)
- **Conector:** Tipo DuPont hembra de una fila y 8 contactos con separación estándar 0.1" (2.54 mm)
- **Temperatura de Operación:** 0 a 50 °C

### Aplicaciones:
- Sistemas de Seguridad
- Selección de Menús
- Entrada de Datos en Sistemas Embebidos

### Codigo de ejemplo:

```arduino
byte pinesFilas[] = {9, 8, 7, 6};
byte pinesColumnas[] = {5, 4, 3, 2};
char teclas[4][4] = {
  {'1', '2', '3', 'A'},
  {'4', '5', '6', 'B'},
  {'7', '8', '9', 'C'},
  {'*', '0', '#', 'D'}
};

void setup() {
  for (int nL = 0; nL <= 3; nL++) {
    pinMode(pinesFilas[nL], OUTPUT);
    digitalWrite(pinesFilas[nL], HIGH);
  }
  for (int nC = 0; nC <= 3; nC++) {
    pinMode(pinesColumnas[nC], INPUT_PULLUP);
  }

  Serial.begin(9600);
  Serial.println("Teclado 4x4");
  Serial.println();
}

void loop() {
  // Barrido por las filas
  for (int nL = 0; nL <= 3; nL++) {
    digitalWrite(pinesFilas[nL], LOW);

    // Barrido en columnas buscando un LOW
    for (int nC = 0; nC <= 3; nC++) {
      if (digitalRead(pinesColumnas[nC]) == LOW) {
        Serial.print("Tecla: ");
        Serial.println(teclas[nL][nC]);
        while (digitalRead(pinesColumnas[nC]) == LOW) {}
      }
    }
    digitalWrite(pinesFilas[nL], HIGH);
  }
  delay(10);
}
