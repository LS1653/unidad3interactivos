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

El término endian se refiere al orden en que se almacenan y transmiten los bytes de datos en una computadora o durante una comunicación entre dispositivos. Existen dos principales tipos de endian que determinan el orden de los bytes en números que ocupan más de un byte, como los números en punto flotante o enteros grandes:

### Little Endian:
 El byte de menor peso (el que contiene los bits menos significativos) se almacena o transmite primero. Es decir, el orden es de menor a mayor peso. Este formato es común en arquitecturas de procesadores como las utilizadas por Intel.

### Big Endian:
 El byte de mayor peso (el que contiene los bits más significativos) se almacena o transmite primero. Es decir, el orden es de mayor a menor peso. Este formato es común en arquitecturas como las de algunos procesadores RISC y en redes (es el estándar para las transmisiones de red).

Ejemplo:

Supongamos que tenemos el número hexadecimal 0x12345678, que en binario es:
00010010 00110100 01010110 01111000

Esto lo podemos dividir en 4 bytes:
0x12 = 00010010
0x34 = 00110100
0x56 = 01010110
0x78 = 01111000

1. Big Endian:
En big endian, los bytes se almacenan en el mismo orden en que se escriben. El número completo sería:
0x12 0x34 0x56 0x78
Entonces, en big endian, el valor en decimal es 305419896.

2. Little Endian:
En little endian, los bytes se almacenan en el orden inverso, es decir, el byte de menor peso se almacena primero. Así que, el número en memoria se ve así:
0x78 0x56 0x34 0x12
En little endian, el valor en decimal es 2018915346.

## Ejercicio 4: transmitir números en punto flotante

¿Cómo transmitir un número en punto flotante? Veamos dos alternativas:
Opción 1:
```
void setup() {
    Serial.begin(115200);
}

void setup() {
    Serial.begin(115200);  // Inicia la comunicación serie a una velocidad de 115200 baudios.
}

void loop() {
    // 45 60 55 d5
    // https://www.h-schmidt.net/FloatConverter/IEEE754.html
    static float num = 3589.3645;  // Declara una variable flotante estática con el valor 3589.3645. 
                                   // La palabra clave 'static' asegura que el valor de 'num' no se reinicie en cada iteración de loop.

    if(Serial.available()){  // Comprueba si hay datos disponibles en el buffer de entrada serial.
        if(Serial.read() == 's'){  // Lee un byte de los datos seriales y verifica si es el carácter 's'.
            Serial.write ( (uint8_t *) &num, 4);  // Si el carácter es 's', envía los 4 bytes que representan 
                                                  // la variable 'num' en formato binario por el puerto serie.
        }
    }
}

```

Opción 2. Aquí primero se copia la información que se desea transmitir a un buffer o arreglo:
```
void setup() {
    Serial.begin(115200);  // Inicia la comunicación serie a 115200 baudios.
}

void loop() {
    // 45 60 55 d5
    // https://www.h-schmidt.net/FloatConverter/IEEE754.html
    float num = 3589.3645;  // Declara una variable de tipo float con el valor 3589.3645.

    static uint8_t arr[4] = {0};  // Declara un arreglo estático de 4 bytes (uint8_t), 
                                  // que se usará para almacenar la representación binaria del float.

    if (Serial.available()) {  // Verifica si hay datos disponibles en el puerto serial.
        if (Serial.read() == 's') {  // Lee un byte de la comunicación serie y comprueba si es el carácter 's'.
            
            memcpy(arr, (uint8_t *)&num, 4);  // Copia los 4 bytes de la variable 'num' (float) al arreglo 'arr'. 
                                              // Usa la función 'memcpy' para mover los bytes desde la dirección de 'num' 
                                              // a 'arr'.

            Serial.write(arr, 4);  // Envía los 4 bytes del arreglo 'arr' por el puerto serial.
        }
    }
}

```

### Preguntas:

#### ¿En qué *endian* estamos transmitiendo el número?
Estamos transmitiendo por el little endian en donde se estan enviando los byytes del menis significativo al más significativo, lo que devuelve el número 3589.3645.

#### Y si queremos transmitir en el *endian* contrario, ¿Cómo se modifica el código?
Modificación del código para transmitir en big endian:
Modifica el código de tal manera para que transmita los bytes en big endian:
Primer codigo:
```
void setup() {
    Serial.begin(115200);  // Inicia la comunicación serie a una velocidad de 115200 baudios.
}

void loop() {
    static float num = 3589.3645;  // Declara una variable flotante estática con el valor 3589.3645.

    if (Serial.available()) {  // Comprueba si hay datos disponibles en el buffer de entrada serial.
        if (Serial.read() == 's') {  // Lee un byte de los datos seriales y verifica si es el carácter 's'.
            uint8_t arr[4];  // Crea un arreglo para almacenar los bytes.
            memcpy(arr, (uint8_t*)&num, 4);  // Copia los 4 bytes de 'num' al arreglo 'arr'.

            // Invertir los bytes para enviar en big endian
            uint8_t temp;
            temp = arr[0];
            arr[0] = arr[3];
            arr[3] = temp;
            temp = arr[1];
            arr[1] = arr[2];
            arr[2] = temp;

            // Envía los 4 bytes en big endian
            Serial.write(arr, 4);  // Envía el arreglo de 4 bytes por el puerto serie.
        }
    }
}
```

Segundo codigo:
```
// Función que se ejecuta una vez al iniciar el programa
void setup() {
    Serial.begin(115200);  // Inicia la comunicación serie a una velocidad de 115200 baudios.
}

// Función que se ejecuta repetidamente después de setup()
void loop() {
    // 45 60 55 d5 // Representación en hexadecimal del número 3589.3645 en formato IEEE 754
    static float num = 3589.3645;  // Declara una variable flotante estática llamada 'num' con el valor 3589.3645.
                                    // La palabra clave 'static' asegura que 'num' mantenga su valor entre las iteraciones de 'loop'.
    static uint8_t arr[4] = {0};  // Declara un arreglo estático de 4 bytes (uint8_t) llamado 'arr' y lo inicializa a cero.
                                    // Este arreglo se utilizará para almacenar la representación binaria de 'num'.

    // Verifica si hay datos disponibles en el buffer de entrada serial
    if (Serial.available()) {
        // Lee un byte de los datos seriales y verifica si es el carácter 's'
        if (Serial.read() == 's') {
            // Copia los 4 bytes que representan la variable 'num' al arreglo 'arr'
            memcpy(arr, (uint8_t *)&num, 4);  // 'memcpy' copia los bytes de 'num' a 'arr', tratando 'num' como un arreglo de bytes

            // Envía los bytes en orden inverso (big endian)
            for (int8_t i = 3; i >= 0; i--) {  // Itera sobre los 4 bytes de 'arr' en orden inverso
                Serial.write(arr[i]);  // Envía el byte actual por el puerto serie
            }
        }
    }
}
```

## Ejercicio 5: envía tres números en punto flotante
Ahora te voy a pedir que practiques. La idea es que transmitas dos números en punto flotante en ambos endian.
### Big endian
```
// Función que se ejecuta una vez al iniciar el programa
void setup() {
    Serial.begin(115200);  // Inicia la comunicación serie a una velocidad de 115200 baudios.
}

// Función que se ejecuta repetidamente después de setup()
void loop() {
    // Declaración de los números en punto flotante
    static float num1 = 4.33;  
    static float num2 = 5798374784734.0387473;  
    static uint8_t arr[4];  // Arreglo para almacenar los bytes de los números

    // Verifica si hay datos disponibles en el buffer de entrada serial
    if (Serial.available()) {
        // Lee un byte de los datos seriales y verifica si es el carácter 's'
        if (Serial.read() == 's') {
            // Copia los bytes de num1 a arr y envía en little endian
            memcpy(arr, (uint8_t *)&num1, 4);  // Copia los bytes de num1
            Serial.write(arr, 4);  // Envía los bytes en little endian

            // Envía los bytes de num1 en big endian
            for (int8_t i = 3; i >= 0; i--) {  // Itera sobre los 4 bytes de 'arr' en orden inverso
                Serial.write(arr[i]);  // Envía el byte actual por el puerto serie
            }

            // Repite el proceso para num2
            memcpy(arr, (uint8_t *)&num2, 4);  // Copia los bytes de num2
            Serial.write(arr, 4);  // Envía los bytes en little endian

            // Envía los bytes de num2 en big endian
            for (int8_t i = 3; i >= 0; i--) {  // Itera sobre los 4 bytes de 'arr' en orden inverso
                Serial.write(arr[i]);  // Envía el byte actual por el puerto serie
            }
        }
    }
}
```

### little endian
```
// Función que se ejecuta una vez al iniciar el programa
void setup() {
    Serial.begin(115200);  // Inicia la comunicación serie a 115200 baudios.
}

// Función que se ejecuta repetidamente después de setup()
void loop() {
    static float num1 = 4.33;  // Declara una variable flotante estática llamada 'num1'.
    static float num2 = 5798374784734.0387473;  // Declara otra variable flotante estática llamada 'num2'.

    static uint8_t arr[4];  // Declara un arreglo estático de 4 bytes (uint8_t), 
                             // que se usará para almacenar la representación binaria del float.

    if (Serial.available()) {  // Verifica si hay datos disponibles en el puerto serial.
        if (Serial.read() == 's') {  // Lee un byte de la comunicación serie y comprueba si es el carácter 's'.

            // Envío de num1 en little endian
            memcpy(arr, (uint8_t *)&num1, 4);  // Copia los 4 bytes de 'num1' al arreglo 'arr'.
            Serial.write(arr, 4);  // Envía los 4 bytes del arreglo 'arr' por el puerto serial.

            // Envío de num2 en little endian
            memcpy(arr, (uint8_t *)&num2, 4);  // Copia los 4 bytes de 'num2' al arreglo 'arr'.
            Serial.write(arr, 4);  // Envía los 4 bytes del arreglo 'arr' por el puerto serial.
        }
    }
}
```