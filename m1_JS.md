# JS sincrónico

El motor de ejecución de JS trabaja en conjunto con otros programas.

Esto da la sensación de que está ocurriendo todo al mismo tiempo, aunque en realidad no es así, por qué en realidad hay operaciones asincrónicas.

El lenguaje JavaScript (JS) es un lenguaje de programación síncrono, lo que significa que las instrucciones se ejecutan secuencialmente, una tras otra, en el orden en que se escriben en el código. Cuando una instrucción se ejecuta, el programa espera a que se complete antes de pasar a la siguiente. Esto es importante para tareas donde el orden de ejecución es crítico, como la manipulación de datos o la interacción con el usuario. Sin embargo, JS también ofrece formas de realizar operaciones asincrónicas, como las llamadas AJAX o las Promises, que permiten que ciertas tareas se ejecuten en segundo plano sin bloquear la ejecución principal del programa. Esto es fundamental para desarrollar aplicaciones web interactivas y receptivas.

En resumen, JavaScript es síncrono en su naturaleza, lo que significa que las instrucciones se ejecutan secuencialmente, pero también admite operaciones asincrónicas para mantener la capacidad de respuesta de las aplicaciones web.


# JS es Single threaded

`Un "hilo de ejecución" o "thread" se refiere a la unidad más pequeña de ejecución en un programa. Es la parte de un programa que puede ejecutarse de manera independiente. Los hilos permiten la ejecución concurrente o paralela de tareas en un programa.`

## JavaScript:

`One Process, One Thread:` En un entorno como un navegador web, JavaScript es single-threaded. Esto significa que solo hay un hilo de ejecución principal en el que se ejecuta el código JavaScript. El código se ejecuta secuencialmente, uno detrás del otro. Si se produce un bloqueo en una tarea (por ejemplo, una tarea de larga duración), puede hacer que la interfaz de usuario se vuelva no receptiva.

Ejemplo en JavaScript (navegador):

```JavaScript
console.log('Tarea 1');
// Simular una tarea de larga duración
for (let i = 0; i < 1000000000; i++) {}
console.log('Tarea 2');

```
One Process, Multiple Threads: Algunos lenguajes, como Java o C++, permiten crear múltiples hilos dentro del mismo proceso. Cada hilo puede ejecutar tareas de manera concurrente. Esto es útil para la ejecución paralela de tareas en sistemas multiprocesadores. Un ejemplo común es la programación en paralelo o la ejecución de múltiples tareas al mismo tiempo.

Ejemplo en Java:
```Java
Thread hilo1 = new Thread(() -> {
    System.out.println("Tarea 1");
});

Thread hilo2 = new Thread(() -> {
    System.out.println("Tarea 2");
});

hilo1.start();
hilo2.start();

```

Multi Processes, One Thread per Process: Algunos sistemas operativos permiten que múltiples procesos se ejecuten de forma independiente, pero cada proceso tiene un solo hilo de ejecución. Cada proceso es independiente y no comparte memoria con otros procesos.

Ejemplo en sistemas Unix/Linux:

```bash
Proceso 1 (un hilo)
Proceso 2 (un hilo)

```

Multiple Processes, Multiple Threads per Process: En sistemas modernos, puedes tener múltiples procesos, y cada proceso puede contener múltiples hilos de ejecución. Esto permite un alto nivel de paralelismo, donde múltiples tareas se ejecutan al mismo tiempo en múltiples procesos y múltiples hilos.

Ejemplo en un sistema operativo multitarea:

```bash
Proceso 1 (Hilos: Hilo 1, Hilo 2)
Proceso 2 (Hilos: Hilo 1, Hilo 2)

```

# Event Loop (Bucle de Eventos)

El Event Loop es el **componente** que maneja la ejecución de código asíncrono. Supervisa constantemente la pila de llamadas y la cola de mensajes. Cuando la pila de llamadas está vacía, toma un mensaje de la cola y lo coloca en la pila de llamadas para su ejecución.

## explicación más completa

explicación simple de lo que ocurre cuando un intérprete de JavaScript se encuentra con una ejecución asíncrona, relacionado con el Event Loop, la Call Stack, Web APIs y la Callback Queue

```JavaScript
console.log("Inicio del script");

let mensaje = "Este es un mensaje dentro de setTimeout";

setTimeout(function() {
    console.log(mensaje);
}, 1000);

console.log("Fin del script");

```

1. Inicio del script: `console.log("Inicio del script")` en la pila de llamadas o **Call Stack**.

2. Declaración de variable: Cuando encontramos la declaración let mensaje, se reserva espacio en la memoria para la variable mensaje. En este punto, la variable se declara, pero aún no tiene un valor asignado.

3. Asignación de valor a la variable: La cadena de texto "Este es un mensaje dentro de setTimeout" se asigna a la variable mensaje.

4. setTimeout: esta función no se ejecuta inmediatamente. En su lugar, se envía al **Web API** para contar un temporizador de 1000 milisegundos (1 segundo). Esta función se envía al Web API. Luego de ese tiempo se ejecuta la función que imprimirá el valor de mensaje
    > La **Web API** o `Interfaz de Programación de Aplicaciones Web` es un conjunto de funciones y capacidades proporcionadas por el navegador (o entorno de ejecución) que `permiten la interacción con elementos web`, como **temporizadores** (como setTimeout), **solicitudes de red**, **manejo de eventos del usuario y más**.

5. Fin del script: `Fin del script` se va a la **Call Stack**.

6. Callback del setTimeout: Una vez que se completa el temporizador, la función que pasamos a setTimeout se coloca en la **Callback Queue** (a continuación más detalles).

## Ahora entra en juego el Event Loop:

8. Event Loop: El Event Loop monitorea la pila de llamadas o **call stack** y verifica si está vacía. Cuando la pila está vacía (después de `Fin del script`), el **Event Loop** toma la función del **Callback Queue** y la coloca en la **call stack**.
    > La **Callback Queue** almacena las funciones **callbacks** que están listas para ejecutarse. En este contexto, cuando el tiempo especificado ha transcurrido, la `call-back` pasa a **Callback Queue**.
    > **Resumen**: cuando el interprete se encuentra con `setTimeout`, se programa un temporizador para que se ejecute después de 1 segundo. Esto significa que JS no bloquea la ejecución del programa, y el temporizador se inicia en segundo plano. Mientras tanto, JS continúa ejecutando otras partes del código. En este caso, `Fin del script` se coloca en **call stack**. Después de aproximadamente 1 segundo, el temporizador se completa y se dispara. La **CB** que pasamos a setTimeout se coloca en la **Callback Queue**. **Event Loop** monitorea la **call back** para verificar si está vacía Cuando la pila está vacía (después de `Fin del script`), **Event Loop** toma la **CB** de `setTimeout`que está en **Callback Queue** y la coloca en la **call back** para su ejecución.




