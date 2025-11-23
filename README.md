# Simulador de Sorteo FIFA World Cup 2026

Este proyecto implementa una simulación interactiva y visual del sorteo de la Copa Mundial de la FIFA 2026, utilizando **Python** y **NiceGUI**. 

El sistema no solo visualiza el sorteo, sino que asegura matemáticamente que se cumplan todas las restricciones geográficas de la FIFA mediante algoritmos de validación y "lookahead" (búsqueda anticipada).

## 📂 Estructura del Proyecto

El núcleo de la simulación reside en tres scripts principales dentro de `02_scripts/`:

1.  **`sorteo_fifa.py`**: La aplicación frontend (NiceGUI). Orquesta el flujo, maneja el estado y visualiza los resultados.
2.  **`simular_bombos.py`**: Lógica de preparación de datos. Genera los bombos basándose en el ranking FIFA y simula los repechajes.
3.  **`simular_sorteo_func.py`**: El "cerebro" lógico. Contiene las funciones de validación de restricciones y el algoritmo de lookahead para evitar bloqueos en el sorteo.
4.  **`simulacion_sorteo_fifa.py`**: Versión de línea de comandos (CLI). Ejecuta la misma lógica de sorteo que la versión web pero muestra los resultados finales directamente en la terminal en formato de texto, ideal para pruebas rápidas o ejecución sin interfaz gráfica.

---

## 🧠 Lógica Detallada

### 1. Generación de Bombos (`simular_bombos.py`)

Este script es responsable de preparar el universo de equipos antes del sorteo.

*   **Fuentes de Datos**: Lee los clasificados y el Power Ranking de la FIFA desde archivos Excel/CSV.
*   **Simulación de Repechajes**: 
    *   Para los cupos aún no definidos (UEFA y FIFA Play-offs), simula ganadores aleatorios (`sample(1)`) para completar la lista de 48 equipos.
*   **Asignación de Bombos**:
    *   **Bombo 1 (Cabezas de Serie)**: Incluye obligatoriamente a los anfitriones (**México, Canadá, Estados Unidos**) y completa los 9 cupos restantes con los equipos mejor rankeados en el ranking FIFA.
    *   **Bombos 2 y 3**: Se llenan secuencialmente con los siguientes 12 mejores equipos por ranking.
    *   **Bombo 4**: Contiene los equipos restantes y los ganadores de los repechajes simulados.

### 2. Algoritmos de Sorteo (`simular_sorteo_func.py`)

Esta es la parte crítica que asegura un sorteo válido. Dado que el sorteo tiene restricciones fuertes, una asignación puramente aleatoria fallaría frecuentemente (llegando a "callejones sin salida" donde los últimos equipos no tienen grupo válido).

#### A. Validación de Restricciones (`checker_validez_grupo`)
Cada vez que se intenta asignar un equipo a un grupo, se verifica:
*   **Regla General**: Ningún grupo puede tener más de un equipo de la misma confederación.
*   **Excepción UEFA**: Se permiten hasta dos equipos europeos por grupo.

#### B. Algoritmo de Lookahead (Búsqueda Anticipada)
Esta es la función más avanzada (`lookahead`). Antes de confirmar la asignación de un equipo a un grupo, el sistema se "pregunta": 
> *"Si pongo a este equipo aquí, ¿será posible asignar legalmente a **todos** los equipos restantes de este bombo en los grupos que quedan?"*

*   **Funcionamiento**:
    1.  Simula temporalmente la asignación.
    2.  Ejecuta una búsqueda recursiva (Backtracking) intentando asignar los equipos restantes a los grupos vacíos.
    3.  Si la recursión encuentra una solución válida para todos, la asignación original se aprueba.
    4.  Si no, se descarta esa opción y se prueba otra, evitando así que el sorteo se bloquee en los pasos finales.

### 3. Interfaz y Orquestación (`sorteo_fifa.py`)

*   **Tecnología**: Utiliza [NiceGUI](https://nicegui.io/) para crear una interfaz web reactiva.
*   **Flujo Asíncrono**: Utiliza `asyncio` para permitir que la animación del sorteo (resaltado de grupos, aparición de banderas) ocurra sin congelar la interfaz.
*   **Gestión de Estado**: Mantiene el estado del sorteo (equipos sorteados, slots ocupados) en una clase `SorteoManager`, permitiendo reinicios rápidos sin recargar el servidor.
*   **Visualización**: Mapea los códigos de país a banderas usando `FlagCDN` para una experiencia visual rica.

---

## 🚀 Ejecución

Para iniciar la simulación:

```bash
# Para la versión gráfica (Web)
python 02_scripts/sorteo_fifa.py

# Para la versión de consola (CLI)
python 02_scripts/simulacion_sorteo_fifa.py
```

La aplicación estará disponible en `http://localhost:5555`.