# Controlador ADRC (Active Disturbance Rejection Control)

Este repositorio documenta el análisis, modelado y simulación del controlador **ADRC** (Active Disturbance Rejection Control), una técnica moderna de control basada en espacio de estados, enfocada en el rechazo activo de perturbaciones y no linealidades.

## 🔎 Introducción

El ADRC es una técnica joven (desarrollada alrededor de 2011) que se ha vuelto relevante para aplicaciones en:

- Control de movimiento
- Sistemas eléctricos de alta velocidad (ej. turbinas eólicas)
- Sistemas con señales rápidas o perturbaciones fuertes

Su principal ventaja es que **no requiere un modelo matemático riguroso**. Se basa en estimar estados y perturbaciones mediante un **observador de estados extendido**.

## 📉 Componentes del Sistema ADRC

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

## ⚖️ Ventajas Clave del ADRC

- Rechaza perturbaciones de forma activa sin necesidad de modelos detallados.
- Integra el error sin requerir una acción integral explícita.
- Funciona bien incluso con sistemas no lineales aproximados como lineales.
- Los polos del sistema pueden ubicarse libremente mediante el observador.

## 📊 Implementación Matemática

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

### Acción de Control:

```matlab
u = referencia - (k1*z1 + k2*z2 + ... + kn*zn)
