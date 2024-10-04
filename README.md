# Trayecto de actividades:
## Ejercicio 1: introducción a los protocolos binarios - caso de estudio
### Preguntas:
#### Cómo se ve un protocolo binario?
Un protocolo binario es una manera de intercambiar datos entre sistemas usando representaciones binarias (ceros y unos) en lugar de texto legible por humanos (como ASCII o UTF-8):

1. Cabecera (Header)
La cabecera contiene información necesaria para interpretar el mensaje, como la longitud, el tipo de mensaje, o la versión del protocolo.

Longitud del mensaje: Cuántos bytes en total se esperan. Ejemplo: 2 bytes para la longitud del mensaje.
Tipo de mensaje: Identifica qué tipo de datos se están enviando (comando, respuesta, etc.). Ejemplo: 1 byte para el tipo de mensaje.
Versiones u otros datos: Otros campos importantes pueden ser el número de versión o identificadores.

2. Cuerpo del mensaje (Payload)
Esta es la parte del mensaje que contiene los datos reales, que pueden estar estructurados de forma binaria. Los valores pueden representar, por ejemplo:

Números enteros: Representados en binario, por ejemplo, 16 bits (2 bytes) para un entero.
Datos flotantes: Usan representaciones binarias como IEEE 754 (32 o 64 bits).
Cadenas de caracteres: Pueden estar codificadas usando representaciones binarias para cada carácter o incluso comprimidas.
Bits individuales: A veces, solo ciertos bits dentro de un byte indican banderas o configuraciones.

3. Checksum o CRC (Opcional)
Se puede incluir un campo de verificación para asegurar que los datos no fueron corrompidos durante la transmisión.

Ejemplo:
Supongamos que queremos enviar un mensaje simple que indica un comando y un número entero. Este podría estructurarse de la siguiente manera:

Campo	Tamaño (bits)	Valor
Cabecera		
Longitud	16	00001000 (8 en decimal, indicando 8 bytes en total)
Tipo de mensaje	8	00000001 (comando)
Cuerpo		
Número entero	32	00000000 00000000 00000001 01010100 (340 en decimal)
El mensaje binario completo sería:
00001000 00000001 00000000 00000000 00000001 01010100

Este protocolo binario tiene una cabecera que define la longitud del mensaje y el tipo de mensaje (un comando), seguido de un número entero en el cuerpo del mensaje. Los sistemas que se comuniquen usando este protocolo deben saber interpretar esta estructura de bytes.

Ventajas del protocolo binario:
Mayor eficiencia: Los datos ocupan menos espacio que en los protocolos basados en texto.
Más difícil de interpretar: Para un humano sin las herramientas adecuadas, es difícil leer o manipular directamente.
Si estás pensando en implementarlo, necesitarás definir con precisión cada campo y cómo serán interpretados en ambos extremos de la comunicación.

#### Puedes describir las partes de un mensaje?
Un mensaje ya sea binario o basado en texto, suele tener varias partes o secciones bien definidas que permiten a los sistemas emisores y receptores entender la información que se está transmitiendo.

1. Cabecera (Header)
La cabecera es la primera parte de un mensaje y contiene información crítica para interpretar los datos que siguen. Dependiendo del protocolo, puede incluir:

-Tamaño del mensaje: Informa cuántos bytes tiene el mensaje completo.
-Tipo de mensaje: Define el tipo de operación o contenido del mensaje, como un comando, una respuesta o datos.
-Identificador del remitente/destinatario: En algunos protocolos, el remitente o el destinatario pueden estar codificados en la cabecera.
-Número de secuencia: En mensajes que forman parte de una transmisión más grande o dividida, este campo indica el orden.
-Versión del protocolo: Algunos protocolos incluyen la versión para asegurar que ambos sistemas usen la misma.
Ejemplo (binario):
Si el mensaje comienza con un byte de tipo y dos bytes para la longitud, la cabecera podría verse como:
00000001 00000000 00001000
Esto significa que el tipo de mensaje es 1 y la longitud es 8 bytes.

2. Cuerpo del mensaje (Payload)
El cuerpo es donde se encuentran los datos reales que se están transmitiendo. Esto varía enormemente dependiendo del propósito del mensaje:

-Datos de usuario: Pueden ser números, texto codificado, archivos, imágenes, etc.
-Comandos: Instrucciones que el receptor debe ejecutar.
-Respuestas: Datos enviados como respuesta a un mensaje previo, como el estado de un sensor, resultados de una operación, etc.
El cuerpo del mensaje puede estar estructurado en varios campos. En protocolos binarios, estos datos pueden estar codificados en formato binario compacto, como enteros, valores flotantes, o incluso bits individuales para indicadores.

Ejemplo (texto):
Si el mensaje está basado en texto, el cuerpo podría contener datos como "Temperatura=25.3", indicando que se está enviando la medición de un sensor de temperatura.

3. Campo de verificación (Checksum/CRC)
Este campo es opcional, pero es muy común en protocolos más complejos. Sirve para verificar que el mensaje no fue alterado durante la transmisión, usando algoritmos como Checksum o CRC (Cyclic Redundancy Check).

-Checksum: Un valor calculado a partir de los bytes del mensaje. Al recibir el mensaje, se recalcula el checksum y se compara con el valor recibido para asegurar que los datos no están corruptos.
-CRC: Similar al checksum, pero más avanzado. Puede detectar errores de transmisión más complejos.
Ejemplo:
El checksum podría ser un número de 2 bytes que se adjunta al final del mensaje, por ejemplo, 0x4B57.

4. Pie de mensaje (Footer o Terminador)
El pie es una marca opcional que indica el final del mensaje. En muchos protocolos, este campo se omite, ya que la longitud del mensaje está incluida en la cabecera. Sin embargo, en otros casos se utiliza un delimitador específico para marcar el final, como:

Carácter de terminación: Por ejemplo, en protocolos de texto, puede ser una secuencia de caracteres como \n o \r\n.
Byte especial: En protocolos binarios, puede ser un byte específico que indica que no hay más datos, como 0xFF.
Ejemplo (texto):
Un mensaje podría terminar con el carácter de nueva línea \n o un símbolo de fin de transmisión, como ETX (End of Text).

#### Para qué sirve cada parte del mensaje?
Cada parte de un mensaje tiene una función específica que garantiza que la comunicación entre los sistemas sea eficiente, organizada y confiable:

1)abecera (Header): Proporciona metadatos esenciales sobre el mensaje, tales como el tipo, tamaño, versiones y el orden en caso de transmisión fragmentada.
2)Cuerpo (Payload): Contiene los datos reales que el emisor desea comunicar al receptor, como comandos, lecturas de sensores o información textual.
3)Campo de verificación (Checksum/CRC): Verifica la integridad de los datos, asegurando que no hayan sido corrompidos durante la transmisión.
4)Pie o delimitador (Footer): Marca el final del mensaje, ayudando a segmentar los datos correctamente y asegurando que se procesen mensajes completos.

## Ejercicio 2: experimento

1. if (Serial)
Esta función se utiliza para comprobar si la comunicación serial está disponible y lista para usarse.
ej:
```
void setup() {
  if (Serial) {
    Serial.println("Serial communication is ready.");
  }
}
```

2.available()
Devuelve el número de bytes disponibles para leer en el buffer de entrada serial.
ej:
```
void loop() {
  if (Serial.available() > 0) {
    char data = Serial.read(); // Lee un byte disponible
    Serial.print("Received: ");
    Serial.println(data);
  }
}
```

3.availableForWrite()
Devuelve el número de bytes disponibles en el buffer de salida serial, lo que indica cuánto espacio queda para escribir datos.
ej:
```
void loop() {
  if (Serial.availableForWrite() > 0) {
    Serial.println("Writing to serial...");
  }
}
```

4.begin()
Inicializa la comunicación serial a una velocidad especificada en baudios.
ej:
```
void setup() {
  Serial.begin(9600);  // Inicia la comunicación serial a 9600 baudios
}
```

5.end()
Termina la comunicación serial.
ej:
```
void loop() {
  Serial.end();  // Finaliza la comunicación serial
}
```

6.find()
Busca una secuencia específica de caracteres en el flujo serial y devuelve true si se encuentra.
ej:
```
void loop() {
  if (Serial.find("hello")) {
    Serial.println("Found 'hello'");
  }
}
```

7.findUntil()
Busca una secuencia de caracteres hasta que encuentra esa secuencia o una secuencia de terminación específica.
ej:
```
void loop() {
  if (Serial.findUntil("start", "end")) {
    Serial.println("Found 'start' before 'end'");
  }
}
```

8.flush()
Espera hasta que todos los datos en el buffer de salida serial hayan sido enviados.
ej:
```
void loop() {
  Serial.print("Sending data...");
  Serial.flush();  // Espera hasta que se envíen todos los datos
}
```

9.parseFloat()
Lee un valor flotante de la entrada serial.
ej:
```
void loop() {
  if (Serial.available() > 0) {
    float value = Serial.parseFloat();  // Lee el valor flotante
    Serial.print("Float received: ");
    Serial.println(value);
  }
}
```

10.parseInt()
Lee un valor entero de la entrada serial.
ej:
```
void loop() {
  if (Serial.available() > 0) {
    int value = Serial.parseInt();  // Lee el valor entero
    Serial.print("Integer received: ");
    Serial.println(value);
  }
}
```

11.peek()
Devuelve el siguiente byte en el buffer serial sin removerlo.
ej:
```
void loop() {
  if (Serial.available() > 0) {
    char nextChar = Serial.peek();  // Mira el siguiente byte
    Serial.print("Next character is: ");
    Serial.println(nextChar);
  }
}
```

12.print()
Envía datos al puerto serial como texto.
ej:
```
void loop() {
  Serial.print("Hello, ");  // Envía "Hello, " sin salto de línea
}
```

13.println()
Envía datos al puerto serial con un salto de línea al final.
ej:
```
void loop() {
  Serial.println("World!");  // Envía "World!" con salto de línea
}
```

14.read()
Lee un byte del buffer serial.
ej:
```
void loop() {
  if (Serial.available() > 0) {
    char data = Serial.read();  // Lee un byte
    Serial.print("Data read: ");
    Serial.println(data);
  }
}
```

15.readBytes()
Lee múltiples bytes del buffer serial y los guarda en un array.
ej:
```
char buffer[10];

void loop() {
  if (Serial.available() > 0) {
    Serial.readBytes(buffer, 10);  // Lee hasta 10 bytes
    Serial.println("Data read into buffer");
  }
}
```

16.readBytesUntil()
Lee bytes del buffer serial hasta encontrar un carácter delimitador o alcanzar un número máximo de bytes.
ej:
```
char buffer[10];

void loop() {
  if (Serial.available() > 0) {
    Serial.readBytesUntil('\n', buffer, 10);  // Lee bytes hasta encontrar '\n'
    Serial.println("Received line");
  }
}
```

17.readString()
Lee la entrada serial completa y la convierte en una cadena de caracteres.
ej:
```
void loop() {
  if (Serial.available() > 0) {
    String data = Serial.readString();  // Lee toda la entrada como string
    Serial.print("String received: ");
    Serial.println(data);
  }
}
```

18.readStringUntil()
Lee la entrada serial hasta que encuentra un carácter delimitador y la devuelve como una cadena de caracteres.
ej:
```
void loop() {
  if (Serial.available() > 0) {
    String data = Serial.readStringUntil('\n');  // Lee hasta nueva línea
    Serial.print("Received until new line: ");
    Serial.println(data);
  }
}
```

19.setTimeout()
Establece el tiempo máximo en milisegundos para esperar datos en funciones como readBytes() o readString().
ej:
```
void setup() {
  Serial.setTimeout(5000);  // Espera hasta 5 segundos por los datos
}
```

20.write()
Envía datos binarios (bytes) al puerto serial.
ej:
```
void loop() {
  byte data = 65;  // Representa 'A' en ASCII
  Serial.write(data);  // Enviar un byte
}
```

21.serialEvent()
Esta función se llama automáticamente cuando llegan datos nuevos al puerto serial. Se debe declarar en tu código, y se ejecutará siempre que haya datos entrantes disponibles.
ej:
```
void serialEvent() {
  while (Serial.available()) {
    char data = Serial.read();
    Serial.print("Event received: ");
    Serial.println(data);
  }
}
```

22.(No existe)Serial.readBytesUntil()
La razón es porque en un protocolo binario usualmente no tiene un carácter de FIN DE MENSAJE, como si ocurre con los protocolos ASCII, donde usualmente el último carácter es el \n.

## Ejercicio 3: ¿Qué es el endian?