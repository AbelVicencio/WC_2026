# Simulación Interactiva del Sorteo de la Copa Mundial de la FIFA 2026 🏆⚽

## 📖 Descripción General
Esta aplicación es una herramienta visual e interactiva desarrollada en Python con **NiceGUI** para simular el sorteo de la fase de grupos del Mundial 2026. Diseñada para entusiastas y analistas, permite visualizar en tiempo real cómo se conforman los 12 grupos del torneo bajo las complejas restricciones geográficas y deportivas de la FIFA.

## ⚙️ Funcionamiento Técnico
La aplicación orquesta una simulación asíncrona que combina:
- **Frontend Reactivo**: Interfaz web moderna que muestra las banderas, los bombos y la asignación paso a paso.
- **Motor de Lógica (`simular_sorteo_func.py`)**: Valida cada movimiento contra las reglas oficiales.
- **Algoritmo de "Lookahead"**: Una característica avanzada que previene "callejones sin salida". Antes de asignar un equipo a un grupo válido, el sistema simula recursivamente si esta asignación permitiría completar el sorteo con los equipos restantes. Si no, descarta la opción, evitando los famosos "bloqueos" que ocurren en sorteos mal planificados.

## 📜 Reglas y Particularidades del Sorteo 2026
Este sorteo presenta desafíos únicos debido al nuevo formato de 48 equipos:

### 1. Estructura de Grupos
- **12 Grupos (A - L)**: A diferencia de los 8 tradicionales.
- **4 Equipos por Grupo**: Se mantiene el formato de liguilla.

### 2. Distribución de Bombos
Los equipos se distribuyen en 4 bombos de 12 equipos cada uno, basados en el **Ranking FIFA** (simulado con datos recientes).
- **Bombo 1 (Cabezas de Serie)**: Incluye a los anfitriones y las mejores selecciones del ranking.
- **Bombos 2, 3 y 4**: Resto de equipos ordenados por ranking.

### 3. Tratamiento de Anfitriones (Hosts)
El sorteo respeta la asignación automática de los tres anfitriones de Norteamérica:
- 🇲🇽 **México**: Asignado fijamente a **A1**.
- 🇨🇦 **Canadá**: Asignado fijamente a **B1**.
- 🇺🇸 **Estados Unidos**: Asignado fijamente a **D1**.

### 4. Restricciones Geográficas (Constraints)
El algoritmo garantiza estrictamente:
- **Principio de Separación Continental**: Ningún grupo puede tener más de un equipo de la misma confederación (CONMEBOL, CONCACAF, CAF, AFC, OFC).
- **Excepción Europea (UEFA)**: Se permite un máximo de **dos** equipos europeos por grupo, dado el alto número de clasificados de esta confederación.

## 🎮 Guía de Uso
1. **Velocidad**: Ajusta el slider para ver el sorteo en cámara lenta (análisis) o rápida.
2. **Controles**:
    - `Play`: Ejecución automática.
    - `Step`: Avanza equipo por equipo (ideal para explicar el procedimiento).
    - `Sorteo Rápido`: Calcula el resultado final instantáneamente.
3. **Visualización**:
    - Los grupos se iluminan en verde cuando se completan.
    - Un banner superior muestra el equipo que está siendo sorteado ("la bolita").
    - Se muestra la lista de equipos restantes en el bombo activo.

## 🚀 Ejecución
```bash
python 02_scripts/GUI_sorteo.py
```

---
*Desarrollado con pasión por el fútbol y el código.*
