# Controlador ADRC (Active Disturbance Rejection Control)

Este repositorio documenta el análisis, modelado y simulación del controlador **ADRC** (Active Disturbance Rejection Control), una técnica moderna de control basada en espacio de estados, enfocada en el rechazo activo de perturbaciones y no linealidades.

## Introducción

Este informe presenta el desarrollo, análisis y simulación de un sistema de control basado en la técnica ADRC (Active Disturbance Rejection Control), junto con la implementación práctica de mecanismos físicos modelados en Simulink y Simscape Multibody. El objetivo es demostrar cómo se puede diseñar un sistema robusto que rechace perturbaciones sin requerir un modelo matemático exacto, y cómo este puede aplicarse a sistemas reales mediante hardware-in-the-loop (HIL).

Su principal ventaja es que **no requiere un modelo matemático riguroso**. Se basa en estimar estados y perturbaciones mediante un **observador de estados extendido**.

**¿Qué es ADRC?**
El ADRC es una técnica de control moderna que busca compensar perturbaciones internas y externas sin depender de un modelo matemático preciso. Fue propuesta por Han y desarrollada por Gao (2014), y se basa en tres componentes clave:

Generador de trayectorias: Convierte una referencia en perfiles de posición, velocidad y aceleración.
Observador de Estados Extendido (ESO): Estima estados reales, perturbaciones y no linealidades.
Controlador proporcional: Aplica una ley de control basada en los estados estimados.

## Componentes del Sistema ADRC

### 1. Generador de trayectorias
- Convierte una referencia de posición en perfiles de posición, velocidad y aceleración.

### 2. Observador de Estados Extendido (ESO)
- Estima:
  - Estados reales
  - Perturbaciones externas
  - No linealidades del sistema
  - Error de estado estacionario

### 3. Controlador Proporcional
- Compensa cada estado estimado con una ganancia proporcional.
- La acción de control se genera como:  
  `u = referencia - (K1*z1 + K2*z2 + ... + Kn*zn)`

##  Ventajas Clave del ADRC

- Rechaza perturbaciones de forma activa sin necesidad de modelos detallados.
- Integra el error sin requerir una acción integral explícita.
- Funciona bien incluso con sistemas no lineales aproximados como lineales.
- Los polos del sistema pueden ubicarse libremente mediante el observador.

## Implementación Matemática
•	Se introduce un estado adicional para representar perturbaciones.
•	El observador estima la dinámica desconocida y las perturbaciones.
•	La acción de control se define como:
`u=referencia−(k1z1+k2z2+⋯+knzn)`

**Variantes**
LADRC: Versión lineal con observador tipo Luenberger.
NADRC: Versión no lineal con observador extendido no lineal.

**Resultados de Simulación**
Se realizaron pruebas con diferentes tipos de señales de entrada (escalón, rampa, sinusoidal) y perturbaciones (rampa, paso, ruido). El ADRC mostró mejor desempeño que el PID tradicional en términos de rechazo de perturbaciones y seguimiento de referencia.

## Parte 2: Simulación de Mecanismos Físicos

Se trabajó con un banco de pruebas que incluye:

**Péndulo invertido**
Helicóptero con 2 grados de libertad
Volante con mecanismo de equilibrio
Estos mecanismos fueron modelados en Simscape y controlados mediante bloques HIL en Simulink.

**Componentes del Sistema**
Motor DC con driver de potencia.
Tarjeta de adquisición de datos (DAQ).
Sensores: corriente, encoder, giroscopio, acelerómetro.
Conectividad: USB tipo C/A.

**Flujo de Trabajo**
Diseño del mecanismo físico.
Modelado en Simscape.
Implementación del control en Simulink.
Simulación con gemelo digital.
Pruebas físicas con el motor real.
Captura y análisis de datos.
Consideraciones de Diseño
Materiales ligeros para evitar sobrecarga.
Montaje preciso al eje del motor.
Acondicionamiento de señales (voltaje/PWM).
Protección contra bloqueo mecánico.

### Modelo extendido:

- Se introduce un nuevo estado para representar perturbaciones: `f(x) = x3`
- Ecuaciones:
  - `x1. = x2`
  - `x2. = f(x) + u`
  - `x3. = 𝑓̂ (x)`

### Observador de Estados:

- Utiliza ganancias `L1, L2, L3` para corregir errores.
- Polos del observador deben ubicarse **más a la izquierda** del plano que los del sistema.
- Se realiza ubicación de polos para forzar error → 0 con un polinomio deseado.


## Conclusión
El uso combinado de ADRC y Simscape permite diseñar sistemas de control robustos, adaptables y eficientes. La integración con hardware real mediante HIL proporciona una plataforma poderosa para validar diseños en entornos reales, lo que es esencial en la formación de ingenieros mecatrónicos.
