# Estados ligados en un pozo de potencial finito

Proyecto de física electrónica que calcula numéricamente los estados ligados de un electrón en un **pozo cuadrado finito unidimensional**. El sistema estudiado tiene una longitud característica de $a=3\,\text{Å}$ y una barrera de potencial de $V_0=3\,\text{eV}$.

El notebook resuelve las ecuaciones trascendentes de los estados pares e impares mediante el método de Newton–Raphson. Después estudia cómo cambian los niveles de energía al variar la masa efectiva del electrón y la profundidad del pozo.

## Contenido

```text
.
├── Electronic_physic_final_project.ipynb
└── README.md
```

## Objetivos

El proyecto se divide en tres apartados:

1. Representar las ecuaciones del pozo finito y obtener los dos primeros niveles de energía permitidos.
2. Estudiar el efecto de una masa efectiva $m^*=x\,m_e$ sobre dichos niveles.
3. Analizar la relación entre las energías permitidas y la profundidad $V_0$ del pozo.

## Modelo físico

Se considera un pozo de potencial finito y simétrico. Los estados ligados cumplen:

$$
0<E<V_0
$$

Dentro del pozo, la función de onda es oscilatoria. Fuera de él, decrece exponencialmente. Al exigir continuidad de la función de onda y de su derivada en las fronteras aparecen dos ecuaciones trascendentes: una para los estados pares y otra para los impares.

El notebook utiliza las variables adimensionales:

$$
z=ka
$$

y:

$$
z_0=\frac{a}{\hbar c}\sqrt{2(m_ec^2)V_0}
$$

donde se emplean unidades de energía para la masa y el valor de $\hbar c$:

$$
\hbar c=197.3\times10^{-9}\ \text{eV·m},
\qquad
m_ec^2=5.11\times10^5\ \text{eV}
$$

Se define además:

$$
f(z)=\sqrt{\left(\frac{z_0}{z}\right)^2-1}
$$

## Ecuaciones de los estados ligados

### Estados pares

Los estados de paridad par satisfacen:

$$
\tan(z)=f(z)
$$

o, de forma equivalente:

$$
g_{\mathrm{par}}(z)=\tan(z)-f(z)=0
$$

### Estados impares

Los estados de paridad impar satisfacen:

$$
-\cot(z)=f(z)
$$

es decir:

$$
g_{\mathrm{impar}}(z)=-\frac{1}{\tan(z)}-f(z)=0
$$

Una vez obtenida una raíz válida $z_n$, la energía se calcula mediante:

$$
E_n=\frac{1}{2m_ec^2}\left(\frac{\hbar c\,z_n}{a}\right)^2
$$

Esta expresión es dimensionalmente equivalente a $E_n=\hbar^2k_n^2/(2m_e)$.

## Método numérico

### Búsqueda de valores iniciales

Las funciones trigonométricas presentan discontinuidades. Para localizar posibles raíces, el código:

1. Divide el intervalo permitido $0<z<z_0$ en miles de puntos.
2. Evalúa la función correspondiente a cada paridad.
3. Detecta cambios de signo entre puntos consecutivos.
4. Descarta cambios demasiado bruscos para evitar confundir una asíntota con una raíz.
5. Utiliza el punto medio de cada intervalo como valor inicial.

### Newton–Raphson

Cada raíz se refina con 50 iteraciones del método:

$$
z_{j+1}=z_j-\frac{g(z_j)}{g'(z_j)}
$$

La derivada auxiliar utilizada para $f(z)$ es:

$$
f'(z)=-\frac{z_0^2}{z^3f(z)}
$$

El programa conserva únicamente las soluciones que cumplen:

$$
0<z<z_0
$$

## Parámetros del problema

| Parámetro | Valor | Descripción |
|---|---:|---|
| $a$ | $3\times10^{-10}\,\text{m}$ | Longitud característica del pozo |
| $V_0$ | $3\,\text{eV}$ | Altura o profundidad del pozo |
| $\hbar c$ | $197.3\times10^{-9}\,\text{eV·m}$ | Constante empleada en unidades relativistas |
| $m_ec^2$ | $5.11\times10^5\,\text{eV}$ | Energía en reposo del electrón |
| Iteraciones | 50 | Iteraciones de Newton–Raphson |

## Resultados principales

Para los valores iniciales del problema se encuentran dos estados ligados:

| Estado | Paridad | Energía |
|---|---|---:|
| Primer estado | Par | $0.5421\,\text{eV}$ |
| Segundo estado | Impar | $2.0138\,\text{eV}$ |

Ambas energías son menores que $V_0=3\,\text{eV}$, por lo que corresponden a estados ligados.

La primera gráfica muestra las intersecciones entre $f(z)$ y las funciones $\tan(z)$ y $-\cot(z)$. Los puntos de intersección representan las soluciones permitidas.

## Estudio de la masa efectiva

En un semiconductor, el movimiento de los portadores dentro de la estructura cristalina puede describirse mediante una masa efectiva:

$$
m^*=x\,m_e
$$

El notebook estudia valores comprendidos aproximadamente entre:

$$
0.1\leq \frac{m^*}{m_e}<2
$$

Para cada valor se recalculan $z_0$ y los primeros estados par e impar. La gráfica resultante muestra la variación de $E_1$ y $E_2$ con $m^*/m_e$.

Al aumentar la masa efectiva, las energías de confinamiento tienden a disminuir, ya que el término cinético es inversamente proporcional a la masa. Además, el valor de $z_0$ cambia y puede modificar el número de estados ligados disponibles.

Las energías objetivo del notebook se calculan inicialmente usando $m^*=m_e$. Por construcción, las líneas de referencia se recuperan alrededor de:

$$
\frac{m^*}{m_e}=1
$$

Por tanto, esta parte muestra la sensibilidad de las energías a la masa efectiva; no determina una masa desconocida a partir de datos experimentales independientes.

## Dependencia con la profundidad del pozo

El notebook repite el cálculo para:

$$
0.1\,\text{eV}\leq V_0\leq10\,\text{eV}
$$

y representa tanto las energías absolutas como los cocientes $E_n/V_0$.

Los resultados permiten observar que:

- Al aumentar $V_0$, las energías ligadas aumentan en valor absoluto respecto al fondo del pozo.
- Las energías crecen más lentamente que la profundidad, por lo que $E_n/V_0$ disminuye.
- Un pozo más profundo puede contener un número mayor de estados ligados.
- Todo estado ligado debe permanecer por debajo de la barrera: $E_n<V_0$.

La línea $E=V_0$ incluida en la gráfica marca el límite entre los estados ligados y los estados no confinados.

## Gráficas generadas

El notebook produce cuatro representaciones principales:

1. Ecuaciones trascendentes y raíces de los estados pares e impares.
2. Primeros niveles de energía en función de $m^*/m_e$.
3. Energías $E_1$ y $E_2$ en función de $V_0$.
4. Energías relativas $E_1/V_0$ y $E_2/V_0$ en función de $V_0$.

## Requisitos

- Python 3.
- NumPy.
- Matplotlib.
- Jupyter Notebook, JupyterLab o Visual Studio Code con soporte para notebooks.

Instalación de las dependencias:

```bash
python -m pip install numpy matplotlib notebook
```

El módulo `math` utilizado en el notebook forma parte de la biblioteca estándar de Python.

## Ejecución

Desde la carpeta del proyecto, ejecuta:

```bash
jupyter notebook
```

Después abre `Electronic_physic_final_project.ipynb` y ejecuta las celdas en orden.

También puede utilizarse JupyterLab o Visual Studio Code.

## Funciones principales del código

| Función | Finalidad |
|---|---|
| `f(z, z0)` | Evalúa el término asociado al decaimiento fuera del pozo |
| `derivative(z, z0)` | Calcula la derivada de `f` |
| `Newton_Raphson(...)` | Refina una raíz par o impar |
| `seed(...)` | Localiza valores iniciales mediante cambios de signo |
| `get_energy(...)` | Devuelve la primera energía válida de una paridad |

## Parámetros que pueden modificarse

- `a`: tamaño característico del pozo.
- `V0`: altura o profundidad de la barrera.
- `me`: masa o energía equivalente de la partícula.
- `masses`: intervalo de masas efectivas estudiado.
- `V0_values`: intervalo de profundidades analizado.
- `divisions`: resolución utilizada para encontrar valores iniciales.
- `iteraciones`: número de pasos de Newton–Raphson.

## Limitaciones y consideraciones

- Las ecuaciones implementadas corresponden a la convención en la que $a$ aparece en $z=ka$. Según cómo se defina geométricamente el pozo, $a$ puede representar su semianchura. Es importante mantener la misma convención al comparar con otras fuentes.
- La detección de raíces se basa en cambios de signo y en un umbral empírico; cerca de las asíntotas puede perder soluciones o detectar candidatos incorrectos.
- Newton–Raphson no garantiza la convergencia para cualquier valor inicial.
- `get_energy` devuelve únicamente la primera raíz válida de cada paridad.
- Cuando no existe un estado impar ligado, el programa devuelve `NaN` para su energía.
- El análisis de masa efectiva utiliza como objetivos energías generadas por el propio modelo con $m^*=m_e$.
- El modelo supone una barrera simétrica, abrupta y unidimensional, e ignora detalles de la estructura cristalina y de las interfaces reales.

## Posibles mejoras

- Sustituir Newton–Raphson por un método acotado como bisección o `scipy.optimize.brentq`.
- Separar los intervalos entre asíntotas antes de buscar raíces.
- Eliminar automáticamente raíces duplicadas.
- Calcular todos los estados ligados para cada masa y profundidad.
- Representar el perfil del potencial y las funciones de onda.
- Determinar la masa efectiva a partir de energías experimentales independientes.
- Añadir una tabla con el número de estados ligados frente a $V_0$.
- Unificar el idioma de textos, comentarios, títulos y etiquetas.

## Licencia

Proyecto educativo de física electrónica. No se ha especificado una licencia de distribución.
