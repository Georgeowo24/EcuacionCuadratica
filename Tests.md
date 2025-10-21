# Documentación de Pruebas
***Integrantes:***
- *Jorge Torres Romero*
- *Cristian Hernández Bonilla*
- *Víctor Mejía Rosas*

## *Caso de Prueba*
Se realizarán las pruebas sobre un proyecto de una página donde se enseña que es una ecuación cuadrática y comó resolverla. <br>
![InicioPagina](/img/Inicio.png)
<br>
A continuación se muestra el ejercicio usado de ejemplo usado en la web <br>
![Ejercicio](/img/Ejercicio.png)
## Prueba Unitaria
***Objetivo:*** Verificar que la lógica de verificarRespuestas funciona correctamente de forma aislada con un caso conocido. <br>
***Escenario:*** Para la ecuación *x² - 5x + 6 = 0*, las raíces son *2* y *3*.
```
function pruebaUnitaria_VerificacionCorrecta() {
  logTestResult('--- Iniciando Prueba Unitaria ---', 'info');
  // 1. Preparación
  // Forzamos los valores de las raíces a un caso conocido
  r1 = 2;
  r2 = 3;
  document.getElementById('respuesta1').value = '3.05'; // Dentro de la precisión
  document.getElementById('respuesta2').value = '1.95'; // Dentro de la precisión

  // 2. Ejecución 
  verificarRespuestas();

  // 3. Verificación 
  const feedback = document.getElementById('feedback').innerHTML;
  if (feedback.includes('¡Correcto!')) {
    logTestResult('PASS: La función de verificación aceptó respuestas correctas (con precisión).', 'pass');
  } else {
    logTestResult('FAIL: La función de verificación no aceptó las respuestas correctas.', 'fail');
  }
}

```
## Prueba de Integración
***Objetivo:*** Verificar que el flujo completo del usuario funciona como se espera. <br>
***Escenario:*** El usuario genera una ecuación, introduce las respuestas correctas y las comprueba
```
function pruebaDeIntegracion_FlujoCompleto() {
  logTestResult('--- Iniciando Prueba de Integración ---', 'info');
  // 1. Generar una ecuación
  let tieneRaicesReales = false;
  let intentos = 0;
  // Intentamos generar una ecuación con raíces reales para poder probar el flujo
  while(!tieneRaicesReales && intentos < 10) {
    tieneRaicesReales = generarEcuacion();
    intentos++;
  }

  if (!tieneRaicesReales) {
    logTestResult('FAIL: No se pudo generar una ecuación con raíces reales para la prueba.', 'fail');
    return;
  }

  logTestResult('INFO: Ecuación con raíces reales generada exitosamente.', 'info');

  // 2. "Introducir" las respuestas correctas (las tomamos de las variables globales)
  document.getElementById('respuesta1').value = r1;
  document.getElementById('respuesta2').value = r2;
        
  // 3. Comprobar las respuestas
  verificarRespuestas();

  // 4. Verificar el resultado
  const feedback = document.getElementById('feedback').innerHTML;
  if (feedback.includes('¡Correcto!')) {
    logTestResult('PASS: El flujo completo (Generar -> Resolver -> Verificar) funciona.', 'pass');
  } else {
    logTestResult('FAIL: El flujo completo falló en la verificación.', 'fail');
  }
}
```

## Prueba de Rendimiento
***Objetivo:*** Medir el tiempo que toma generar un gran número de ecuaciones.
***Escenario:*** Se llama a la función generarEcuacion 10,000 veces y se mide el tiempo total.
```
function pruebaDeRendimiento_GeneracionMasiva() {
  logTestResult('--- Iniciando Prueba de Rendimiento ---', 'info');
  const numIteraciones = 10000;
  const startTime = performance.now();

  for (let i = 0; i < numIteraciones; i++) {
    generarEcuacion();
  }

  const endTime = performance.now();
  const totalTime = endTime - startTime;

  logTestResult(PASS: Se generaron ${numIteraciones} ecuaciones en ${totalTime.toFixed(2)} ms., 'pass');
  logTestResult(INFO: Tiempo promedio por generación: ${(totalTime / numIteraciones).toFixed(4)} ms., 'info');
}
```
## Resultados Obtenidos
A continuación se muestran los resultados obtenidos en las pruebas:
![ResultadosPruebas](/img/Pruebas.png)
