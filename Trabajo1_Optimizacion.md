# Trabajo 1 – Optimización numérica, metaheurística y combinatoria

**Presentado por:**
Santiago Jimenez Henao
Catalina Arango Sierra
Sara Castano Montoya
Juan Diego Cifuentes

**Profesor:** Juan David Ospina Arango

**Monitor:** Andrés Mauricio Zapata Rincón

Martes 13 de Octubre de 2026

**Universidad Nacional de Colombia**
**Facultad de Minas**
**Redes Neuronales y Algoritmos Bioinspirados - 2026-02**

---

## Tabla de contenido

- Enlace Repositorio Git
- Parte 1: Optimización numérica
  - Metodología y Regla de Comparación Justa
  - Resultados de los Experimentos
  - Visualización
  - Discusión
- Fundamentación Matemática Parte 1
  - Función de Rosenbrock
  - Función de Rastrigin
  - Algoritmos utilizados
- Uso de Inteligencia Artificial y Cacería de la Alucinación P1
- Parte 2: Optimización combinatoria
  - Problema
  - Fundamentación Matemática Parte 2
  - Fuentes de información
  - Resultados
  - Uso de Inteligencia Artificial y Cacería de la Alucinación P2
- Referencias

---

## Enlace Repositorio Git

<https://github.com/jucifuenteso/Redes-neuronales-yalgoritmos-bioinspirados.git>

---

## Parte 1: Optimización numérica

### Metodología y Regla de Comparación Justa

Se compararon Rosenbrock y Rastrigin en 2D y 3D con cinco métodos: descenso por gradiente de paso fijo, descenso con backtracking, algoritmo genético (GA), enjambre de partículas (PSO) y evolución diferencial (DE). Se ejecutaron 30 corridas por configuración - 20 configuraciones en total - con semillas distintas entre corridas y semillas emparejadas entre métodos para que compartieran los puntos iniciales o las poblaciones iniciales correspondientes.

La métrica de éxito fue $f(x_{\text{final}}) - f^* \leq 10^{-4}$ con $f^* = 0$ para ambas funciones. El presupuesto por corrida fue de $50.000\,n$ evaluaciones equivalentes: 100.000 en 2D y 150.000 en 3D.

Cada algoritmo contabiliza por separado sus llamadas a la función y al gradiente. Para comparar métodos basados en gradiente con métodos de caja negra se usó:

$$\text{evaluaciones equivalentes} = n_f + 2n \cdot n_{\nabla f}.$$

La equivalencia cobra una evaluación de gradiente como $2n$ evaluaciones de $f$, el costo de aproximarla mediante diferencias centrales en $n$ dimensiones. Es una regla conservadora para el gradiente, pues en estos experimentos se dispone de gradientes analíticos. Por eso también se reportan $n_f$ y $n_{\nabla f}$ por separado. El backtracking cuenta las evaluaciones de $f$ que usa para probar pasos; el descenso de paso fijo no cuenta las evaluaciones de $f$ que solo se usan para registrar la curva. La malla de fondo de las animaciones tampoco se contabiliza.

### Resultados de los Experimentos

La tabla resume los valores finales $f(x_{\text{final}}) - f^*$ de las 30 corridas. "Media ± DE" muestra la media y la desviación estándar muestral; "mejor" y "peor" son el mínimo y el máximo observados. Los pares de evaluaciones indican los promedios de llamadas a $f$ / $\nabla f$ por corrida.

**Tabla 1.1.** *Resultados de la optimización de la función Rosenbrock en dos y tres dimensiones utilizando descenso por gradiente, algoritmos evolutivos, PSO y evolución diferencial*

**Rosenbrock 2D**

| Método | Media | Desv. est. | Mejor | Peor | Éxito (%) | Eval. f | Eval. ∇f | Eval. equiv. | Hasta el umbral |
|---|---|---|---|---|---|---|---|---|---|
| GD paso fijo | 2.47e-10 | 2.17e-10 | 2.92e-11 | 1.20e-09 | 100.0 | 0 | 25 000 | 100 000 | 34 276 |
| GD backtracking | 6.75e-13 | 3.04e-13 | 5.94e-13 | 2.25e-12 | 100.0 | 30 238 | 15 115 | 90 698 | 20 808 |
| GA | 3.88e-04 | 8.49e-04 | 6.40e-08 | 3.61e-03 | 63.3 | 21 344 | 0 | 21 344 | 10 480 |
| PSO | 6.90e-14 | 9.65e-14 | 1.25e-17 | 3.63e-13 | 100.0 | 11 853 | 0 | 11 853 | 1 780 |
| DE | 2.56e-30 | 4.64e-30 | 0 | 1.60e-29 | 100.0 | 6 339 | 0 | 6 339 | 1 120 |

**Rosenbrock 3D**

| Método | Media | Desv. est. | Mejor | Peor | Éxito (%) | Eval. f | Eval. ∇f | Eval. equiv. | Hasta el umbral |
|---|---|---|---|---|---|---|---|---|---|
| GD paso fijo | 8.57e-12 | 1.09e-11 | 1.05e-12 | 6.32e-11 | 100.0 | 0 | 24 906 | 149 437 | 45 306 |
| GD backtracking | 7.33e-13 | 1.32e-14 | 7.11e-13 | 7.55e-13 | 100.0 | 32 853 | 16 422 | 131 387 | 32 102 |
| GA | 1.13e-02 | 1.05e-02 | 1.90e-05 | 3.92e-02 | 6.7 | 149 964 | 0 | 149 964 | 94 668 |
| PSO | 4.63e-11 | 4.70e-11 | 1.39e-12 | 1.96e-10 | 100.0 | 96 330 | 0 | 96 330 | 26 010 |
| DE | 3.43e-03 | 1.88e-02 | 5.24e-25 | 1.03e-01 | 93.3 | 15 246 | 0 | 15 246 | 3 660 |

**Tabla 1.2.** *Resultados de la optimización de la función Rastrigin en dos y tres dimensiones utilizando descenso por gradiente, algoritmos evolutivos, PSO y evolución diferencial*

**Rastrigin 2D**

| Método | Media | Desv. est. | Mejor | Peor | Éxito (%) | Eval. f | Eval. ∇f | Eval. equiv. | Hasta el umbral |
|---|---|---|---|---|---|---|---|---|---|
| GD paso fijo | 14.19 | 9.98 | 0.995 | 40.79 | 0.0 | 0 | 7.7 | 30.7 | — |
| GD backtracking | 11.48 | 8.82 | 0.995 | 31.84 | 0.0 | 71.7 | 31.5 | 197.8 | — |
| GA | 8.39e-09 | 3.89e-08 | 0 | 2.14e-07 | 100.0 | 6 232 | 0 | 6 232 | 994 |
| PSO | 0 | 0 | 0 | 0 | 100.0 | 8 931 | 0 | 8 931 | 2 100 |
| DE | 0 | 0 | 0 | 0 | 100.0 | 7 252 | 0 | 7 252 | 2 120 |

**Rastrigin 3D**

| Método | Media | Desv. est. | Mejor | Peor | Éxito (%) | Eval. f | Eval. ∇f | Eval. equiv. | Hasta el umbral |
|---|---|---|---|---|---|---|---|---|---|
| GD paso fijo | 23.61 | 11.76 | 1.990 | 44.77 | 0.0 | 0 | 8.3 | 49.6 | — |
| GD backtracking | 20.83 | 12.95 | 0.995 | 42.78 | 0.0 | 73.4 | 32.4 | 268.0 | — |
| GA | 2.25e-11 | 9.03e-11 | 0 | 4.86e-10 | 100.0 | 13 166 | 0 | 13 166 | 2 436 |
| PSO | 0 | 0 | 0 | 0 | 100.0 | 15 796 | 0 | 15 796 | 5 010 |
| DE | 0 | 0 | 0 | 0 | 100.0 | 15 466 | 0 | 15 466 | 6 900 |

**Figura 1.1.** *Valor final de cada corrida*

> *[Figura: Rosenbrock 2D y 3D — valor final de cada corrida (30 por método; raya = mediana), escala logarítmica de f − f* con piso en 1e-16 y línea de umbral de éxito. Métodos: GD paso fijo, GD backtracking, GA, PSO, DE.]*

**Figura 1.2.** *Valor final de cada corrida*

> *[Figura: Rastrigin 2D y 3D — valor final de cada corrida, misma escala y convenciones que la Figura 1.1.]*

**Figura 1.3.** *Convergencia con el mismo presupuesto*

> *[Figura: Rosenbrock 2D y 3D — convergencia (mediana y rango intercuartil de f − f*, 30 corridas por método) frente a evaluaciones equivalentes de f.]*

**Figura 1.4.** *Convergencia con el mismo presupuesto*

> *[Figura: Rastrigin 2D y 3D — convergencia (mediana y rango intercuartil de f − f*) frente a evaluaciones equivalentes de f (n_f + 2n·n_∇f).]*

### Visualización

- <https://github.com/jucifuenteso/Redes-neuronales-yalgoritmos-bioinspirados/blob/main/Parte1/resultados/optimizacion_rastrigin_2d.gif>
- <https://github.com/jucifuenteso/Redes-neuronales-yalgoritmos-bioinspirados/blob/main/Parte1/resultados/optimizacion_rosenbrock_2d.gif>

### Discusión

**Rosenbrock - un mínimo, pero un valle difícil.** La función tiene un único mínimo global, aunque no es globalmente convexa. Cerca del óptimo es localmente convexa, pero su valle curvo y estrecho produce mal condicionamiento: $\kappa$ es aproximadamente 2 508 en 2D y 2 950 en 3D. El gradiente fue muy fiable: ambos descensos tuvieron 100 % de éxito y valores finales muy pequeños. Sin embargo, el paso fijo necesitó una mediana de 34 276 evaluaciones equivalentes en 2D y 45 306 en 3D para cruzar el umbral. El backtracking lo hizo con 20 808 y 32 102, respectivamente.

DE y PSO también alcanzaron el umbral en todas las corridas 2D; sus medianas fueron menores: 1 120 y 1 780 evaluaciones equivalentes. En 3D, PSO tuvo 100 % de éxito y una mediana de 26 010; DE usó 3 660 hasta el umbral, pero falló en 2 de 30 corridas. GA fue menos fiable - 63.3 % de éxito en 2D y 6.7 % en 3D -, mostrando que no todos los métodos heurísticos se adaptan bien al valle estrecho.

**Rastrigin - cuencas bien condicionadas, muchas soluciones locales.** Rastrigin es globalmente no convexa y multimodal, con 121 mínimos locales en 2D y 1 331 en 3D dentro del dominio estudiado. Cada cuenca es aproximadamente redonda y está bien condicionada localmente ($\kappa \approx 1$), por lo que el gradiente converge rápido una vez dentro de ella. El problema es llegar a la cuenca correcta: en las 30 corridas principales, ninguno de los descensos alcanzó el umbral; el mínimo local más bajo no global tiene valor cercano a 0.995. Un experimento adicional de 10 000 inicializaciones estimó para el descenso de paso fijo una probabilidad de éxito de 0.94 % en 2D y 0.09 % en 3D. En cambio, GA, PSO y DE lograron 100 % de éxito en ambas dimensiones, con medianas de 994–2 120 evaluaciones equivalentes en 2D y 2 436–6 900 en 3D.

**Balance.** El gradiente aportó precisión y confiabilidad en el caso de un único mínimo, pero pagó el costo del condicionamiento de Rosenbrock y no escapó de los mínimos locales de Rastrigin. Los heurísticos aportaron exploración global y fueron más eficientes en estas pruebas, sobre todo en Rastrigin; a cambio, su éxito no es garantizado, como muestran los resultados de DE y GA en Rosenbrock 3D. La comparación también depende del precio atribuido al gradiente: si se contara cada gradiente como una sola evaluación de $f$, en Rosenbrock 3D el paso fijo superaría a PSO, aunque DE seguiría usando menos evaluaciones para cruzar el umbral.

---

## Fundamentación Matemática Parte 1

### Función de Rosenbrock

La función de Rosenbrock es una función no convexa utilizada como problema de prueba del rendimiento para algoritmos de optimización que se introdujo por Howard H. Rosenbrock en 1960. Es también conocida como Rosenbrock la función del valle o la función del plátano.

El mínimo global está dentro de un valle plano, largo, estrecho y de forma parabólica. Encontrar el valle es trivial. Sin embargo, converger al mínimo global es difícil.

La función está definida por:

$$f(x, y) = (a - x)^2 + b(y - x^2)^2$$

Tiene un mínimo global en $(x, y) = (a, a^2)$, donde $f(x, y) = 0$. Generalmente $a = 1$ y $b = 100$. Sólo en el caso trivial de $a = 0$ la función es simétrica y el mínimo está en el origen.

### Función de Rastrigin

En optimización matemática, la función de Rastrigin es una función no convexa que se utiliza como problema de prueba de rendimiento para algoritmos de optimización. Es un ejemplo típico de función multimodal no lineal. Fue propuesta por primera vez en 1974 por Rastrigin como una función bidimensional y ha sido generalizada por Rudolph. La versión generalizada fue popularizada por Hoffmeister y Bäck y Mühlenbein et al. Encontrar el mínimo de esta función es un problema bastante difícil debido a su gran espacio de búsqueda y su gran número de mínimos locales.

En un $n$-dominio dimensional que se define por:

$$f(\mathbf{x}) = An + \sum_{i=1}^{n} \left[ x_i^2 - A\cos(2\pi x_i) \right]$$

donde $A = 10$ y $x_i \in [-5.12, 5.12]$. Hay muchos extremos:

El mínimo global se encuentra en $x = 0$ donde $f(x) = 0$.
El valor máximo de la función para $x_i \in [-5.12, 5.12]$ está ubicado en $x = (\pm 4.52299366\ldots, \ldots, \pm 4.52299366\ldots)$:

| Número de dimensiones | Valor máximo en ± 4.52299366 |
|---|---|
| 1 | 40.35329019 |
| 2 | 80.70658039 |
| 3 | 121.0598706 |
| 4 | 161.4131608 |
| 5 | 201.7664509 |
| 6 | 242.1197412 |
| 7 | 282.4730314 |
| 8 | 322.8263216 |
| 9 | 363.1796117 |

### Algoritmos utilizados

Todos buscan el valor más bajo posible de la función, manteniéndose dentro de los límites definidos para las variables. La diferencia principal es cómo eligen el siguiente punto que van a probar.

**Descenso por gradiente con paso fijo**
Empieza desde un punto y consulta el gradiente, que indica hacia dónde aumenta más la función. El algoritmo se mueve en la dirección contraria para reducirla. En cada paso usa siempre el mismo tamaño de movimiento. En estos experimentos fue más pequeño para Rosenbrock y algo mayor para Rastrigin. Si un movimiento sale del dominio permitido, se ajusta al borde. Se detiene cuando los cambios son suficientemente pequeños o cuando se agota el presupuesto.

**Descenso por gradiente con búsqueda de paso (backtracking)**
También usa el gradiente para elegir la dirección de descenso, pero no fija de antemano cuánto avanzar. Prueba un paso y comprueba si reduce la función lo suficiente. Si no, lo hace más pequeño y vuelve a probar. Cuando encuentra un paso aceptable, actualiza el punto y continúa. Al igual que el método anterior, respeta los límites del dominio y se detiene al alcanzar la tolerancia o el presupuesto.

**Algoritmo genético (GA)**
Mantiene un grupo de soluciones candidatas, llamado población. En cada generación:

1. Elige padres mediante torneos: toma algunos candidatos al azar y selecciona el de mejor resultado.
2. Combina pares de padres para producir hijos. En cada variable, el hijo puede tomar valores dentro de un intervalo que incluye los valores de sus padres y una extensión alrededor de ellos.
3. Cambia aleatoriamente algunas variables de los hijos, añadiendo pequeñas perturbaciones.
4. Conserva las mejores soluciones de la generación anterior y las combina con los hijos.

Los hijos que quedan fuera de los límites permitidos se recortan al borde. El proceso se detiene si el mejor resultado deja de mejorar durante muchas generaciones o si se termina el presupuesto.

**Optimización por enjambre de partículas (PSO)**
También trabaja con una población, pero cada candidato es una partícula que se mueve por el dominio. Cada partícula recuerda su mejor posición anterior y el enjambre recuerda la mejor posición encontrada por cualquiera de ellas. En cada iteración, la velocidad de una partícula se combina con dos tendencias: volver hacia su propia mejor posición y acercarse a la mejor posición del enjambre. Se añade aleatoriedad para que las partículas no sigan exactamente el mismo camino. La velocidad está limitada para evitar saltos demasiado grandes. Si una partícula sale del dominio, se coloca en el borde y se detiene el movimiento en la coordenada que lo sobrepasó.

**Evolución diferencial (DE)**
Mantiene una población de soluciones y produce una candidata nueva para cada individuo. Para ello, toma tres soluciones distintas: usa una como base y añade a ella una diferencia entre las otras dos. Esa diferencia determina tanto la dirección como el tamaño del cambio. Después combina la candidata nueva con la solución actual, variable por variable. Finalmente, compara ambas: se conserva la candidata si su resultado es igual o mejor; de lo contrario, se mantiene la solución actual. Los valores que salen del dominio se recortan a sus límites.

**Diferencia general**
Los dos descensos por gradiente siguen la información local de la pendiente. GA, PSO y DE no usan esa información: prueban y combinan soluciones para explorar el dominio. Por eso el gradiente puede ser muy eficaz cuando su dirección lleva al mínimo buscado, mientras que los métodos heurísticos pueden explorar distintas regiones, aunque no garantizan encontrar el mejor resultado en todas las corridas.

---

## Uso de Inteligencia Artificial y Cacería de la Alucinación P1

### Prompt A

> "Lee estos archivos y analízalos para las siguientes instrucciones que te daré (*Material de estudio de la clase*)
> Ahora debes hacer los siguiente 1.considera de la siguientes funciones las 2 mas facil y óptima a trabajar:
> -Función de Rosenbrock
> -Función de Rastrigin
> -Función de Schwefel
> -Función de Griewank
> -Función de Goldstein-Price
> -Función de las seis jorobas de camello
>
> Ahora continua con los siguientes puntos y reglas a trabajar:
> 2. Optimicen las funciones en dos y tres dimensiones con descenso por gradiente partiendo de una condición inicial aleatoria.
> 3. Optimicen las mismas funciones con algoritmos evolutivos, optimización por enjambre de partículas (PSO) y evolución diferencial.
> 4. Representen con un GIF animado o un video el proceso de optimización del descenso por gradiente y de al menos uno de los métodos heurísticos.
>
> Regla de comparación justa
>
> Para comparar métodos deben contar las evaluaciones de forma explícita y justificada:
> -Reporten por separado el número de evaluaciones de f y el número de evaluaciones de f.
> -Propongan y justifiquen una equivalencia entre ambas. Por ejemplo, aproximar el gradiente por diferencias centrales cuesta 2n evaluaciones de f en dimensión n.
> -Hagan varias corridas independientes (al menos 30 por configuración) y reporten estadísticas: media, desviación estándar, mejor y peor valor, y la tasa de éxito con un umbral que ustedes definan.
>
> Discusión
> ¿Qué aportaron los métodos de descenso por gradiente y qué aportaron los métodos heurísticos? Respondan con base en el valor final de la función objetivo y el número de evaluaciones, apoyándose en la geometría de cada función (convexidad local, número de mínimos locales, condicionamiento).
>
> Todo esto haz que quede en la carpeta llamada Parte1"

### Respuestas al prompt

> Con las funciones y optimizadores ya implementados, ahora haré una corrida piloto para verificar gradientes y calibrar paso, presupuesto y criterio de parada antes de lanzar el experimento completo de 30 corridas.
>
> ////////////////////////////////////////////////////////////////////////////
>
> El experimento completo ya está corriendo en segundo plano (30 corridas × 2 funciones × 2 dimensiones × 5 métodos, más las figuras y los GIF); tarda unos minutos. Cuando termine reviso las figuras y los GIF, y escribo el informe README_P1.md con las tablas y la discusión.
>
> ////////////////////////////////////////////////////////////////////////////
>
> El experimento terminó bien y las dos figuras estáticas quedaron correctas; estoy puliendo los GIF (el fondo de Rastrigin quedaba demasiado cargado y pesaban 7–9 MB) y luego escribo el informe.
>
> ////////////////////////////////////////////////////////////////////////////
>
> Encontré un warning pendiente: el cálculo de la mediana sobre configuraciones sin corridas exitosas. Lo corrijo y vuelvo a ejecutar todo para generar los resultados finales.
>
> ////////////////////////////////////////////////////////////////////////////
>
> Escrito para: el profesor y el monitor del curso, que son quienes van a calificar el informe.
> La Parte 1 quedó completa en Parte1/: código, resultados de las 30 corridas, figuras, dos GIF y el informe con la discusión en README_P1.md. python main.py corre sin advertencias en unos dos minutos y medio, y dos ejecuciones seguidas dieron estadísticas idénticas.
>
> **Funciones elegidas**
> Elegí Rosenbrock y Rastrigin. Goldstein-Price y seis jorobas de camello solo existen en 2D, así que no permiten el caso 3D. Entre las cuatro restantes, estas dos son las más simples y cada una aísla una dificultad distinta: Rosenbrock tiene un solo mínimo pero está mal condicionada (κ ≈ 2 500), y Rastrigin tiene 121 mínimos locales en 2D y 1 331 en 3D, cada uno bien condicionado.
>
> **Resultados principales**
> Éxito significa terminar con f − f* ≤ 10⁻⁴; las evaluaciones son la mediana de evaluaciones equivalentes hasta cruzar ese umbral.
>
> ////////////////////////////////////////////////////////////////////////////
>
> **Gradiente:** fiable y preciso con un solo mínimo, pero lento por el condicionamiento. En Rastrigin converge en 8 gradientes, siempre a un mínimo local.
> **Heurísticos:** PSO y DE encuentran el mínimo global de Rastrigin siempre y son más baratos en Rosenbrock, pero sin garantías (DE falló 2 de 30 en Rosenbrock 3D; GA casi todas).
>
> **Decisiones que tomé y puedes cambiar en config.yaml**
> **Equivalencia:** un gradiente se cobra como 2n evaluaciones de f (diferencias centrales). Es desfavorable para el gradiente; el informe también revisa qué cambia si se cobra como 1.
> **Presupuesto:** 50 000·n evaluaciones equivalentes por corrida, igual para todos los métodos.
> **Dos variantes de gradiente:** paso fijo y backtracking (Armijo), las dos de las notas de clase. Ambas proyectan sobre el dominio para buscar en la misma región que los heurísticos.
> **Hiperparámetros de los heurísticos:** valores estándar de la literatura, sin sintonizar por función. La paciencia de parada (100 generaciones) sí la fijé con una prueba piloto.
> **GIF:** uno por función en 2D, con los cinco métodos avanzando al mismo ritmo de evaluaciones equivalentes. Es la corrida 0, sin escogerla.

### Prompt B

> «Pasame las citas y referencias que hayas usado para realizar esto en normas apa 7»

### Prompt C

> «laas que dicen Doi.org no llevan a ninguna parte»

### Prompt D

> «en base a lo que hiciste del archivo parte 1 , resuelve esto:» seguido del enunciado del punto 5.

### Hallazgo 1 — Bibliografía: una cita que no respalda la afirmación

**El prompt utilizado:** Prompt A para el informe y Prompt B para la lista de referencias.

**La parte relevante de la respuesta de la IA:** En README_P1.md, línea 13, la IA escribió: «Un solo mínimo para n = 2 y 3 (Kok y Sandrock, 2009), en el fondo de un valle curvo y estrecho: mal condicionada». Cuando le pedimos las referencias en APA, repitió: «(Kok y Sandrock, 2009) Que Rosenbrock tiene un solo mínimo para n = 2 y n = 3». En esa misma respuesta reconoció: «Las cinco publicadas las cité de memoria, sin abrir los documentos».

**Cómo sospechamos que había un error:**
Primero creímos que los enlaces DOI no funcionaban y se lo dijimos a la IA (Prompt C). La IA comprobó que los cinco DOI sí resolvían y que los metadatos coincidían. Un DOI válido solo prueba que el artículo existe, no que diga lo que se le atribuye. Además, la IA había admitido que citó de memoria. Por eso, al pedirle la cacería de alucinaciones (Prompt D), la IA comparó cada afirmación con el resumen de su fuente. Así apareció este error.

**La evidencia que lo demuestra:**
El resumen de Kok y Sandrock (2009), que la IA consultó en el repositorio de la Universidad de Pretoria, habla de dos variantes de la función. De la primera dice que «is shown to possess a single stationary point, the global minimum». De la segunda, que «has numerous stationary points for high dimensionality» y que «For dimensions up to 100, only two local minimizers are found». No menciona n = 3.

**La corrección:**
Quitamos la atribución a Kok y Sandrock, respaldamos la afirmación con el experimento y citamos a Shang y Qiu (2006) solo para el segundo mínimo en n = 4 a 30. (Propuesta; todavía no está aplicada en el README.)

**La lección aprendida:** Aprendimos que verificar una cita tiene dos pasos: que el DOI resuelva y que la fuente diga lo que se le atribuye, al menos en el resumen. Cuando una propiedad se puede comprobar con un experimento barato, ese experimento es mejor respaldo que una cita de memoria.

---

- Agregué un experimento que no pediste: 10 000 corridas cortas de gradiente en Rastrigin para medir la probabilidad de caer en la cuenca global (0.94 % en 2D, 0.09 % en 3D). Lo hice porque 0 éxitos en 30 corridas no permite distinguir entre "improbable" e "imposible".
- Instalé pyyaml en tu Python, porque el main.py lo necesita igual que el de la Parte 2.
- La verificación de los gradientes analíticos contra diferencias centrales la hice en un script temporal que no quedó en el repositorio; el informe la menciona. No hice commit: Parte1/ aparece como carpeta nueva sin seguimiento en git.

---

## Parte 2: Optimización combinatoria

### Problema

Un vendedor debe recorrer las 47 capitales de provincia de la España peninsular (se excluyen Palma, Las Palmas de Gran Canaria y Santa Cruz de Tenerife por ser insulares, y Ceuta y Melilla por no ser provincias ni accesibles por carretera desde la península). El recorrido es cerrado: el vendedor vuelve a la ciudad de salida.

### Retos

- Encuentren el orden óptimo del recorrido usando colonias de hormigas y algoritmos genéticos.
- El costo de desplazamiento, teniendo en cuenta: valor de la hora, costo de peajes y costo del combustible.

Esta parte del trabajo compara dos metaheurísticas de naturaleza muy distinta sobre el mismo problema combinatorio: colonia de hormigas (ACO) y algoritmo genético (GA).

### Fundamentación Matemática Parte 2

Sea un conjunto de N = 47 ciudades, indexadas por i = 1, ..., N. El objetivo es encontrar el recorrido cerrado que visita cada ciudad exactamente una vez y regresa a la ciudad de origen, minimizando el costo total del viaje.

Una solución se representa como una permutación $\pi = (\pi_0, \pi_1, \ldots, \pi_{N-1})$, donde $\pi_i \in \{1, \ldots, N\}$.

El costo de ir de la ciudad $i$ a la ciudad $j$ es:

$$C_{i,j} = \underbrace{v_h \cdot t_{i,j}}_{\text{tiempo}} + \underbrace{p_{i,j}}_{\text{peaje}} + \underbrace{\frac{d_{ij}}{100} \cdot \gamma \cdot \rho_c}_{\text{combustible}}$$

- $v_h$: valor de la hora del vendedor (EUR/h).
- $t_{ij}$: tiempo de viaje por carretera entre i y j (h).
- $p_{ij}$: costo de peaje en el trayecto por carretera entre i y j (EUR).
- $d_{ij}$: distancia por carretera entre i y j (km).
- $\gamma$ (gamma): consumo homologado del vehículo elegido (L/100 km).
- $\rho_c$: precio del combustible (EUR/L).

#### Metodología: colonia de hormigas (ACO)

Ant System clásico. En cada iteración, cada una de las hormigas construye una ruta completa visitando las 47 ciudades sin repetir, eligiendo la siguiente ciudad de forma probabilística.

El atractivo de ir de la ciudad i a la j es el inverso de su costo:

$$\eta_{i,j} = \frac{1}{C_{i,j}}$$

**Probabilidad de transición.** Estando en la ciudad i, la hormiga k elige la siguiente ciudad j (no visitada) con probabilidad:

$$p_{i,j}^{k} = \frac{\tau_{ij}^{\alpha} \cdot \eta_{ij}^{\beta}}{\sum_{l \in \text{no visitadas}} \tau_{il}^{\alpha} \cdot \eta_{il}^{\beta}}$$

donde $\tau_{ij}$ es la feromona acumulada en el arco (i, j), $\alpha$ pondera la importancia de la feromona y $\beta$ la del atractivo.

**Actualización de feromona:**

$$\tau_{i,j} \leftarrow (1 - \rho)\,\tau_{i,j} + \sum_{k=1}^{m} \Delta\tau_{i,j}^{k}, \qquad \Delta\tau_{i,j}^{k} = \frac{Q}{L_K}$$

donde $\rho$ es la tasa de evaporación, $L_K$ es el costo total de la ruta de la hormiga k, y Q es una constante de depósito.

Hiperparámetros: ver Tabla 2.2.

#### Metodología: algoritmo genético (GA)

**1. Planteamiento del Problema**

El objetivo es encontrar la ruta cerrada de costo mínimo que visite exactamente una vez N = 47 ciudades (capitales provinciales peninsulares) y regrese al punto de origen. Dado que es un problema simétrico ($c_{ij} = c_{ji}$ y $c_{ii} = 0$), el número de rutas posibles es $(N-1)!$, lo cual resulta inviable de resolver mediante fuerza bruta.

**Función de Costo**

El costo de ir de la ciudad i a la ciudad j combina tres variables: tiempo de viaje, peajes y consumo de combustible:

$$C_{i,j} = v_h \cdot t_{i,j} + p_{i,j} + \frac{d_{ij}}{100} \cdot \gamma \cdot \rho_c$$

Donde:

- `t_ij`: Tiempo de viaje entre i y j en horas.
- `p_ij`: Costo de peajes entre i y j en euros.
- `d_ij`: Distancia por carretera entre i y j en kilómetros.
- `valor_hora`: Costo de oportunidad del tiempo en EUR/hora.
- `consumo`: Litros consumidos por cada 100 km.
- `precio_combustible`: Costo del combustible en EUR/litro.

**Costo Total del Recorrido π**

$$C(\pi) = \sum_{i=0}^{N-2} c_{\pi_i,\,\pi_{i+1}} + c_{\pi_{N-1},\,\pi_0}$$

**Algoritmo Genético (GA)**

*Concepto.* Mantiene una población de soluciones representadas como permutaciones de longitud N. Aplica operadores adaptados que garantizan recorridos válidos sin repetir ni omitir ciudades.

*Componentes principales:*

- **Codificación y Evaluación:** Representación mediante permutaciones de las 47 ciudades. La aptitud del individuo es su costo total C(π).
- **Selección:** Selección por torneo (k individuos al azar), escogiendo la ruta con menor costo para actuar como progenitor.
- **Cruzamiento (Order Crossover - OX):** Copia un segmento del primer padre y completa las posiciones restantes con las ciudades faltantes del segundo padre, respetando su orden original.
- **Mutación (Swap):** Intercambia aleatoriamente la posición de dos ciudades con probabilidad $p_m$.
- **Elitismo:** Los $e$ mejores individuos pasan directamente a la siguiente generación para evitar la pérdida de la mejor ruta encontrada.

**4. Criterio de Parada y Reporte**

Ambos algoritmos ejecutan un número prefijado de iteraciones o generaciones $n_{iter}/n_{generaciones}$. La solución reportada corresponde al mejor recorrido global registrado durante el proceso, desglosado en:

- Costo total C(π).
- Desglose individual de costos por tiempo, peajes y combustible.

### Fuentes de información

Detalla las fuentes y APIs utilizadas para construir la matriz de costos del problema de las 47 capitales de provincia.

**Tabla 2.1.** *Fuentes de información utilizadas para la construcción de la matriz de costos.*

| Datos | Fuentes | Fecha de consulta |
|---|---|---|
| Coordenadas de las 47 capitales | Nominatim (OpenStreetMap) | 10 de octubre de 2026 |
| Tiempo y distancia por carretera entre ciudades | OSRM Table API (router.project-osrm.org) | 11 de octubre de 2026 |
| Costo de peajes por trayecto | HERE Routing API v8 | 11 de octubre de 2026 |

*Fuente: Elaboración propia a partir de las APIs de Nominatim, OSRM y HERE Routing.*

Se apoyó en herramientas estandarizadas y oficiales para garantizar precisión geoespacial y de enrutamiento: Nominatim (OpenStreetMap) para coordenadas, OSRM Table API para tiempos y distancias por carretera, y HERE Routing API v8 para el cálculo de peajes.

**Tabla 2.2.** *Hiperparámetros configurados para el algoritmo de Colonia de Hormigas (ACO).*

| Parámetro | Símbolo | Valor |
|---|---|---|
| Número de hormigas | m | 47 |
| Iteraciones | — | 300 |
| Peso de feromona | α | 1 |
| Peso de atractivo | β | 2 |
| Tasa de evaporación | ρ | 0,1 |
| Constante de depósito | Q | 1 |
| Feromona inicial | τ₀ | 1 |
| Semilla | — | 1983 |

*Fuente: Elaboración propia.*

En la tabla 2.2 se muestra la configuración de los parámetros clave para el algoritmo basado en hormigas. Se estableció una cantidad de hormigas equivalente al número de ciudades y una evaporación moderada para equilibrar la exploración y la explotación de las rutas.

**Tabla 2.3.** *Hiperparámetros configurados para el Algoritmo Genético (GA).*

| Parámetro | Valor |
|---|---|
| Tamaño de población | 150 |
| Generaciones | 400 |
| Probabilidad de cruce | 0,9 |
| Probabilidad de mutación | 0,2 |
| Fracción de élite | 0,1 |
| Tamaño de torneo | 3 |
| Semilla | 1983 |

*Fuente: Elaboración propia.*

En la tabla 2.3 se presentan los parámetros configurados para el algoritmo genético. Se otorgó un mayor presupuesto de generaciones (400 frente a 300 de ACO).

### Resultados

**Tabla 2.4.** *Resultados comparativos de optimización para los algoritmos ACO y GA (semilla 1983).*

| Método | Costo total (EUR) | Horas totales | Costo tiempo (EUR) | Costo peajes (EUR) | Km totales | Costo combustible (EUR) |
|---|---|---|---|---|---|---|
| ACO | 2491,51 | 83,6 | 1672 | 61,46 | 6664,2 | 758,05 |
| GA | 3274,27 | 106,1 | 2121,8 | 167,9 | 8655,6 | 984,57 |

*Fuente: Elaboración propia a partir de las corridas del algoritmo.*

En la tabla 2.4 se compara el desempeño final de ambos algoritmos evaluando costo total, horas, peajes, kilómetros y costo de combustible. Como se observa, ACO superó claramente al GA, encontrando una ruta óptima significativamente más económica (2.491,51 EUR frente a 3.274,27 EUR de GA), recorriendo menos distancia (6.664,2 km frente a 8.655,6 km) y requiriendo menor tiempo de viaje (83,6 horas frente a 106,1 horas).

**Figura 2.1.** *Curvas de convergencia del costo mínimo encontrado por iteración o generación para ACO y GA.*

> *[Figura: "Convergencia de los algoritmos" — mejor costo encontrado (EUR) frente a iteración/generación. ACO parte cerca de 4 200 EUR y se estabiliza en torno a 2 500 EUR hacia la iteración 130; GA parte cerca de 7 500 EUR y se estabiliza en torno a 3 300 EUR.]*

*Fuente: Elaboración propia.*

Las curvas del mejor costo encontrado por iteración/generación para cada algoritmo, sobre las mismas 47 ciudades. Como se puede observar, ACO converge más rápido y a un valor más bajo, mientras que GA mejora de forma más gradual y se estanca alrededor de la generación 270 sin alcanzar el nivel de ACO ni con el doble de iteraciones disponibles (400 vs 300).

**Figura 2.2.** *Mejor ruta obtenida mediante la colonia de hormigas (ACO) a través de las capitales peninsulares de España.*

> *[Figura: "Mejor ruta - ACO (costo = 2,491.5 EUR)" — recorrido cerrado en el plano longitud–latitud.]*

*Fuente: Elaboración propia.*

En la anterior figura se muestra el recorrido cerrado sobre las 47 capitales graficado en el plano longitud–latitud, con el costo total. La ruta se ve razonablemente compacta y sin cruces entre regiones lejanas; los segmentos siguen principalmente capitales vecinas, lo que es consistente.

**Figura 2.3.** *Mejor ruta obtenida mediante el algoritmo genético (GA) a través de las capitales peninsulares de España.*

> *[Figura: "Mejor ruta - GA (costo = 3,274.3 EUR)" — recorrido cerrado en el plano longitud–latitud.]*

*Fuente: Elaboración propia.*

En esta figura se observa la ruta óptima hallada por el algoritmo genético. A diferencia de la ruta de ACO, aquí se notan varios cruces de líneas; un segmento que se cruza a sí mismo casi siempre se puede acortar intercambiando el orden de visita. Esto respalda la idea de que al GA le falta para alcanzar al desempeño del ACO.

### Enlaces a los gifs

- Gif de la evolución ACO
- Gif de la evolución GA

### Enlace al repositorio Git

<https://github.com/jucifuenteso/Redes-neuronales-yalgoritmos-bioinspirados/tree/main/Parte2>

### Enlace al video

<https://drive.google.com/file/d/1NhwNrMmYwBg5_-X95LWAWTkSCEgs7s7U/view?usp=sharing>

---

## Uso de Inteligencia Artificial y Cacería de la Alucinación P2

**Prompt**

> Hola voy a realizar un trabajo de redes neuronales bioinspirados, te voy a compartir archivos de jupyter y pdfs del tema, aparte la rubruca del trabajo, necesito desarrllar unicamente la parte dos, explora que partes del material serian utiles y antes de generar codigo, exolicame como se haría y estructura, para ponernos de acuerdo.

Empezó a hacer una lista con coordenadas pero por alguna razon empezó a hacerla con 37 capitales, no 47 y empezó a plantear una estructura que implementaba partes modulares redundantes.

**Prompt**

> Hola por favor dame paginas con la informacion de las coordenadas de las ciudades de españa, si son de sitios oficiales del gobierno, mucho mejor

Mandó a sitios de datos abiertos del gobierno español pero dentro no había ninguna base de datos que en realidad contuviese lo que se necesitaba.

**Prompt**

> Necesito los datos que se piden en el trabajo, con links asociados y fuentes reales, para mayor precision en la solucion del problema

Responde: 'No voy a darte un precio por litro inventado ni presentar un precio actual como si fuera el promedio de otra fecha. El valor definitivo debe salir de la descarga del MITECO y quedar acompañado de la fecha y del método de cálculo del promedio.'

**Prompt:**

> Volvi a empezar el pryecto, por ahora ya tengo los datos con los que se va a hacer el ejercicio, te voy a pasar los archivos, quiero que los leas, estos archivos no se van a tocar, ahora quiero que me digas que archivos se deben crear para continuar con el proyecto y dame una descripcion de que harian esos archivos, antes de generar codigo como tal lo estoy trabajando en vscode. recuerda que para el proceso de colonia de hormigas y el resto debe estar basado lo mas que se pueda en el material que te compartí antes

Empezó a proponer tres archivos de validación, uno para si ya existían las tablas, otro para si sí había ahora sí las 47 capitales, imagino que por el reclamo anterior de que no habían 47 capitales y un archivo de experimentos que hacía barridos con los parámetros.

**Prompt:**

> Vamos a trabajar las distancias con latitud y longitud cierto? quiero que tanto los tiempos de carretera como el costo de pasajes, sean hechos manualmente, osea yo relleno eso en un excel, despues lo transformo en .csv. Entonces ahora genera los codigos por favor. voy a usarlo en google colab. dame como debe ser la estructura del excel para que al transformarlo a csv sea compatible con el codigo.

> Explicame paso a paso como lo abro desde colab por favor o Podrias hacer tu las tablas? basado los datos abiertos mas recientes que haya?

En este punto lo que hizo fue generar un zip de todos los archivos que él creía que necesitaba. Me entregó un zip, supe que estaba mal porque había mucho código, alguno redundante y cosas que eran desproporcionadamente robustas pensando en hacer todo lo más modular que pudiese. Entonces también habían varios problemas en el tratamiento de datos.

---

## Referencias

Clerc, M., y Kennedy, J. (2002). The particle swarm—Explosion, stability, and convergence in a multidimensional complex space. *IEEE Transactions on Evolutionary Computation, 6*(1), 58–73.

shelman, L. J., y Schaffer, J. D. (1993). Real-coded genetic algorithms and interval-schemata. En L. D. Whitley (Ed.), *Foundations of genetic algorithms* (Vol. 2, pp. 187–202). Morgan Kaufmann.

Griewank, A., y Walther, A. (2008). *Evaluating derivatives: Principles and techniques of algorithmic differentiation* (2.ª ed.). Society for Industrial and Applied Mathematics.

Kok, S., y Sandrock, C. (2009). Locating and characterizing the stationary points of the extended Rosenbrock function. *Evolutionary Computation, 17*(3), 437–453.

Ospina Arango, J. D., y Zapata Rincón, A. M. (2026b). *Optimización por enjambre de partículas (PSO)* [Diapositivas]. Universidad Nacional de Colombia, Facultad de Minas.

Storn, R., y Price, K. (1997). Differential evolution – A simple and efficient heuristic for global optimization over continuous spaces. *Journal of Global Optimization, 11*(4), 341–359.

GitHub - johnberroa/Ant-Colony-Optimization: A Python implementation of the Ant Colony Optimization algorithm for generating solutions to such problems as the Traveling Salesman Problem. (s. f.-b). GitHub. <https://github.com/johnberroa/Ant-Colony-Optimization>

Dorigo, M., Maniezzo, V., & Colorni, A. (1996). Ant system: Optimization by a colony of cooperating agents. *IEEE Transactions on Systems, Man, and Cybernetics, Part B (Cybernetics), 26*(1), 29–41. <https://doi.org/10.1109/3477.484436>

Gad, A. F. (2021). *PyGad: An intuitive genetic algorithm Python library*. arXiv. <https://arxiv.org/abs/2106.06158>

HERE Technologies. (2026). *HERE Routing API* [Documentación de API]. <https://developer.here.com>

Ospina Arango, J. D. (2026). *Ant_colony.ipynb: Ejemplo de problema del vendedor viajero con colonias de hormigas* [Material de clase no publicado]. Universidad Nacional de Colombia, Departamento de Ciencias de la Computación y de la Decisión.

Ospina Arango, J. D. (2026). *Prep_Algoritmos_Evolutivos.ipynb: Introducción a los algoritmos evolutivos* [Material de clase no publicado]. Universidad Nacional de Colombia, Departamento de Ciencias de la Computación y de la Decisión.

Ospina Arango, J. D. (2020). *Ant Colony Optimization* [Notas de clase manuscritas no publicadas]. Universidad Nacional de Colombia, Departamento de Ciencias de la Computación y de la Decisión.

Project OSRM. (s. f.). *Open Source Routing Machine* [Software]. <https://project-osrm.org>

OpenStreetMap contributors. (2026). *Nominatim* [Software de geocodificación]. <https://nominatim.openstreetmap.org>
