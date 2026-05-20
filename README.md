# Proyecto final PIC - Lab 5

Este repositorio contiene el desarrollo completo de un flujo de diseño fotónico integrado orientado a la simulación, modelado y generación de layout de un filtro/WDM basado en interferómetros Mach-Zehnder (MZI). El trabajo combina simulaciones electromagnéticas, modelos compactos de circuito, diseño de acopladores direccionales y construcción final del layout en GDS usando `gdsfactory` y un PDK local para la plataforma `upvfab`.

## Que se puede encontrar en el repositorio

El proyecto esta organizado como una secuencia de notebooks y archivos auxiliares que documentan cada etapa del diseño:

- `P01_T01_Simulation_Propagation_Modes.ipynb`: simulación de modos de propagación en guías SOI. En este notebook se define la estructura de la guía, se calcula el indice efectivo en el rango de 1500 nm a 1600 nm, se analiza la dispersión modal y se extraen parametros como el indice de grupo y la dispersión cromatica.
- `P01_T02_Design_Directional_Coupler.ipynb`: diseño y aproximación del acoplador direccional. Se estudia la relación entre el gap, la longitud de acoplo y la transferencia de potencia, usando una aproximación basada en la longitud `Lpi`.
- `P01_T03_MZI_Filter_SAX_Model.ipynb`: construcción del modelo compacto del filtro usando SAX. Aqui se modelan guías dispersivas, acopladores direccionales ideales y bloques MZI en cascada para formar la respuesta espectral del WDM.
- `P02_T04_Filter_Die_Layout.ipynb`: diseño del layout de los bloques principales del filtro, incluyendo las estructuras S1 y S2 y pruebas de patrones repetibles en `gdsfactory`.
- `P02_T05_Layout_Die.ipynb`: integración final del diseño sobre el die.

Tambien se incluyen archivos de soporte y resultados intermedios:

- `Lab5_PDK.py`: contiene funciones de alto nivel para generar los bloques del proyecto, incluyendo `ARTS1`, `ARTS2`, `WDM` y `die`.
- `Lab5_Die.gds`: archivo GDS final generado para el die del laboratorio.
- `SOI_Waveguide.npy`: datos guardados de la simulación de la guía SOI, usados para evitar repetir calculos de modos costosos.
- `Gap_vs_Lpi.npy`: datos relacionados con la longitud de acoplo del acoplador direccional en función del gap.
- `img/`: figuras generadas durante las simulaciones y pruebas del WDM, incluyendo respuestas de etapas intermedias y el diseño del multiplexor.

## PDK y herramientas locales

Dentro de `src/upvfab` se incluye una versión local del PDK utilizado en el proyecto. Este PDK define tecnologia, capas, celdas parametrizadas y componentes fotónicos utiles para construir el layout:

- guías rectas, bends y transiciones;
- acopladores direccionales;
- MZIs;
- MMIs;
- anillos;
- grating couplers;
- heaters, vias y metalizaciones;
- estructuras de die y pads;
- archivos de configuración para KLayout.

El paquete `src/upvfab_design_tools` contiene funciones auxiliares para el trabajo de simulación y analisis, como utilidades para modos, geometría, propagación y visualización.

## Flujo general del proyecto

El trabajo sigue una ruta progresiva:

1. Se simula una guía SOI para obtener el indice efectivo y parametros dispersivos relevantes.
2. Se diseña un acoplador direccional compatible con restricciones de fabricación.
3. Se construyen modelos compactos para guías, acopladores y bloques MZI.
4. Se validan las respuestas de las etapas del filtro/WDM.
5. Se trasladan los bloques a layout usando `gdsfactory`.
6. Se integra el diseño completo en un die y se exporta el archivo `Lab5_Die.gds`.

## Dependencias principales

El proyecto usa Python 3.12 o superior. Las dependencias principales estan definidas en `pyproject.toml`:

- `gdsfactory==9.34.0`
- `gplugins[sax,femwell,tidy3d]==2.0.1`
- `cspdk>=1.4.2`
- `pytz`

Estas librerias permiten trabajar con layout fotónico, simulación, modelos compactos y herramientas asociadas al flujo de diseño.

## Nota

Algunos modelos usados durante el desarrollo son aproximaciones de primer orden. Por ejemplo, el acoplador direccional ideal usado en SAX no incluye todos los efectos no ideales, como perdidas por exceso, dependencia espectral completa, asimetrias de fabricación o efectos de bends y tapers. Aun asi, el flujo permite validar la idea del WDM y llevarla hasta un layout GDS funcional.