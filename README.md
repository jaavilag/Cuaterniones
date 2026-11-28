# Álgebra de cuaterniones sobre cuerpos finitos

Repositorio dedicado al estudio algebraico y computacional de los
**cuaterniones con coeficientes en los cuerpos finitos**
(`\mathbb{Z}`{=tex}\_p), donde (p) es un número primo. Contiene
programas en Python para explorar el producto de cuaterniones,
identificar elementos invertibles y examinar computacionalmente
propiedades relacionadas con la estructura algebraica, además de una
suite de pruebas automatizadas.

## Contenido

-   [Descripción del proyecto](#descripción-del-proyecto)
-   [Fundamento matemático](#fundamento-matemático)
-   [Organización del repositorio](#organización-del-repositorio)
-   [Descripción de los programas](#descripción-de-los-programas)
-   [Pruebas automatizadas](#pruebas-automatizadas)
-   [Requisitos](#requisitos)
-   [Instalación y ejecución](#instalación-y-ejecución)
-   [Resultados](#resultados)
-   [Alcance y consideraciones](#alcance-y-consideraciones)

## Descripción del proyecto

El proyecto estudia operaciones y propiedades de los cuaterniones sobre
(`\mathbb{Z}`{=tex}\_p) mediante algoritmos implementados en Python. Se
consideran cuaterniones representados por cuatro componentes y se
realizan recorridos computacionales sobre los elementos del conjunto
para estudiar su comportamiento bajo el producto de Hamilton.

El repositorio se organiza en cuatro componentes:

1.  **Producto:** calcula el producto de pares de cuaterniones.
2.  **Invertibles:** busca pares de cuaterniones cuyo producto sea la
    identidad y almacena los resultados.
3.  **Estructura:** realiza una exploración computacional orientada a
    determinar si los elementos no nulos admiten inversos, como parte
    del análisis de la posible estructura de anillo de división.
4.  **Test automatizado:** implementa pruebas unitarias para verificar
    propiedades algebraicas seleccionadas.

## Fundamento matemático

Un cuaternión se representa como

\[ q=a+bi+cj+dk, \]

donde (a,b,c,d`\in`{=tex}`\mathbb{Z}`{=tex}\_p), y las unidades
imaginarias satisfacen las relaciones de Hamilton:

\[
i^2=j^2=k\^2=-1,`\qquad `{=tex}ij=k,`\quad `{=tex}jk=i,`\quad `{=tex}ki=j,
\]

\[ ji=-k,`\quad `{=tex}kj=-i,`\quad `{=tex}ik=-j. \]

Los elementos se representan computacionalmente mediante cuádruplas:

\[ q=(a,b,c,d)`\in`{=tex}`\mathbb{Z}`{=tex}\_p\^4. \]

Para dos cuaterniones

\[ q_1=(a_1,b_1,c_1,d_1),`\qquad`{=tex} q_2=(a_2,b_2,c_2,d_2), \]

el producto de Hamilton, reducido módulo (p), es

\[
```{=tex}
\begin{aligned}
q_1q_2=\big(&
a_1a_2-b_1b_2-c_1c_2-d_1d_2,\\
& a_1b_2+b_1a_2+c_1d_2-d_1c_2,\\
& a_1c_2-b_1d_2+c_1a_2+d_1b_2,\\
& a_1d_2+b_1c_2-c_1b_2+d_1a_2
\big)\pmod p.
\end{aligned}
```
\]

El elemento identidad multiplicativa es

\[ e=(1,0,0,0). \]

La operación no es conmutativa en general: por ejemplo, (ij=k), mientras
que (ji=-k).

### Norma e invertibilidad

Para (q=(a,b,c,d)), el programa de pruebas utiliza la norma reducida

\[ N(q)=a^2+b^2+c^2+d^2`\pmod `{=tex}p. \]

Cuando (N(q)`\neq 0`{=tex}) en (`\mathbb{Z}`{=tex}\_p), se puede
calcular el inverso multiplicativo mediante

\[ q^{-1}=`\overline `{=tex}q,N(q)^{-1}, \]

donde

\[ `\overline `{=tex}q=(a,-b,-c,-d) \]

es el conjugado y (N(q)\^{-1}) es el inverso modular de la norma. En la
implementación, si la norma es cero, la función de inversión devuelve
`None`.

El conjunto de cuaterniones tiene (p\^4) elementos. Para los primos
impares considerados en las pruebas, el conteo teórico de elementos
invertibles utilizado como referencia es

\[
`\left`{=tex}\|H(`\mathbb{Z}`{=tex}\_p)\^`\times`{=tex}`\right`{=tex}\|
=(p^2-1)(p^2-p) =p^4-p^3-p\^2+p. \]

## Organización del repositorio

La estructura general del repositorio es la siguiente:

``` text
.
├── Invertibles/
│   ├── AlgoritmoInvertibles.docx
│   ├── ProgramaInvertibles
│   ├── ProgramaInvertibles.txt
│   ├── invertibles.py
│   ├── resultados_invertiblesZp_3.xlsx
│   ├── resultados_invertiblesZp_5.xlsx
│   ├── resultados_invertiblesZp_7.xlsx
│   ├── resultados_invertiblesZp_11.xlsx
│   └── resultados_invertiblesZp_13.xlsx
│
├── Producto/
│   ├── AlgoritmoProducto.docx
│   ├── ProgramaProducto.txt
│   ├── producto.py
│   ├── ResultadosProductoZp_2.txt
│   └── ResultadosProductoZp_3.txt
│
├── Estructura/
│   ├── AlgoritmoEstructura.docx
│   ├── ProgramaEstructura.txt
│   ├── esctructura.py
│   ├── ResultadosEstructuraZp_3.txt
│   ├── ResultadosEstructuraZp_5.txt
│   ├── ResultadosEstructuraZp_7.txt
│   ├── ResultadosEstructuraZp_11.txt
│   └── ResultadosEstructuraZp_13.txt
│
└── Test_Automatizado/
    ├── test_cuaterniones.py
    └── test_cuaterniones.pdf
```

**Nota:** el árbol anterior recoge los nombres de archivos comunicados
para el repositorio. Conviene comprobar que coincidan exactamente con
los nombres y las mayúsculas/minúsculas de GitHub. En particular, se
conserva `esctructura.py` tal como fue informado; si el archivo se
renombra, debe actualizarse también este árbol.

Los documentos `.docx` contienen la descripción de los algoritmos. Los
archivos `.txt` presentan versiones de los programas o resultados de
ejecución, mientras que los archivos `.py` corresponden a los scripts
ejecutables. Los resultados de invertibilidad se exportan a Excel.

## Descripción de los programas

### 1. Carpeta `Producto`

**Propósito:** calcular el producto de dos cuaterniones para todos los
pares de elementos generados a partir de los coeficientes de
(`\mathbb{Z}`{=tex}\_p).

El programa:

-   Construye dos cuaterniones (x) e (y), recorriendo sus cuatro
    componentes en ({0,1,`\ldots`{=tex},p-1}).
-   Calcula el producto de Hamilton (z=x`\cdot `{=tex}y).
-   Reduce cada componente del resultado módulo (p).
-   Imprime los factores y el producto obtenido.

Los resultados disponibles corresponden a los valores de (p) indicados
en los archivos de la carpeta.

### 2. Carpeta `Invertibles`

**Propósito:** identificar los pares de cuaterniones cuyo producto es la
identidad multiplicativa.

El programa recorre los pares ordenados
(x,y`\in`{=tex}`\mathbb{Z}`{=tex}\_p\^4), calcula (z=x`\cdot `{=tex}y) y
selecciona aquellos casos en los que

\[ x`\cdot `{=tex}y=(1,0,0,0). \]

Los pares encontrados se organizan en una tabla con las componentes de
(x), (y) y (z). El programa utiliza `pandas` para exportar los
resultados a archivos Excel (`.xlsx`).

Esta búsqueda permite examinar computacionalmente las relaciones de
inversibilidad. La comprobación bilateral de que un elemento es
invertible se aborda explícitamente en la suite de pruebas
automatizadas.

### 3. Carpeta `Estructura`

**Propósito:** explorar computacionalmente si los elementos no nulos
poseen inversos multiplicativos, como criterio relacionado con la
posibilidad de que la estructura sea un anillo de división.

El programa recorre los pares de cuaterniones, calcula sus productos y
cuenta, para cada elemento (x), si encuentra al menos un elemento (y)
tal que

\[ x`\cdot `{=tex}y=(1,0,0,0). \]

Al finalizar, presenta el conteo obtenido (`Dav`) y lo compara con
(p\^4). La interpretación de esta exploración debe considerar que un
anillo de división exige que todo elemento no nulo tenga inverso
multiplicativo; para establecer la propiedad de manera rigurosa también
deben verificarse los axiomas de anillo y la existencia de inversos
bilaterales.

### 4. Carpeta `Test_Automatizado`

**Propósito:** validar mediante pruebas unitarias propiedades
algebraicas de los cuaterniones sobre (`\mathbb{Z}`{=tex}\_p).

La documentación `test_cuaterniones.pdf` describe una suite implementada
con `pytest`. Las pruebas se organizan alrededor de seis propiedades:

1.  **Identidad multiplicativa:** comprueba que
    (e`\cdot `{=tex}q=q`\cdot `{=tex}e=q).
2.  **Cuadrados de las unidades imaginarias:** verifica que
    (i^2=j^2=k\^2=-1`\pmod `{=tex}p).
3.  **No conmutatividad:** comprueba las relaciones de Hamilton,
    incluyendo (ij=k) y (ji=-k).
4.  **Asociatividad del producto:** verifica que (x(qh)=(xq)h).
5.  **Distributividad por izquierda y derecha:** comprueba \[
    x(q+h)=xq+xh,`\qquad `{=tex}(q+h)x=qx+hx. \]
6.  **Invertibilidad bilateral y conteo de unidades:** comprueba
    (qu=e=uq) y compara el número de unidades con \[ p^4-p^3-p\^2+p. \]

La estrategia descrita combina recorridos exhaustivos y muestreo
determinista:

-   Para algunas propiedades se recorren todos los elementos de
    (`\mathbb{Z}`{=tex}\_p\^4).
-   Para (p=3), la asociatividad y la distributividad se verifican
    exhaustivamente sobre las ternas.
-   Para primos mayores, se utilizan muestras deterministas de ternas
    para limitar el costo computacional.
-   Se emplean semillas fijas para que las muestras sean reproducibles.

El PDF contiene la explicación de las funciones, las propiedades
verificadas, los primos utilizados y el código de la suite.

## Requisitos

-   Python 3.10 o superior para utilizar la sintaxis de anotaciones de
    tipos presente en la suite documentada.
-   `pandas` para la exportación de resultados a Excel en el programa de
    invertibilidad.
-   `openpyxl` como motor de escritura de archivos `.xlsx` utilizado
    habitualmente por `pandas`.
-   `pytest` para ejecutar las pruebas automatizadas.

Instalación de dependencias:

``` bash
python -m pip install pandas openpyxl pytest
```

Los programas de producto y estructura utilizan funciones de la
biblioteca estándar de Python y no requieren `pandas` para las
operaciones que se muestran en los archivos de programa suministrados.

## Instalación y ejecución

Clona el repositorio:

``` bash
git clone URL_DEL_REPOSITORIO
```

Entra en la carpeta del proyecto:

``` bash
cd NOMBRE_DEL_REPOSITORIO
```

Ejecuta cada programa desde su carpeta correspondiente. Por ejemplo:

``` bash
python producto.py
python invertibles.py
python esctructura.py
```

**Importante:** en los programas compartidos, la función principal está
definida como `simulacionZp(p)`. Si el archivo no contiene una llamada a
esa función, debe invocarse desde el intérprete de Python o agregarse
una llamada al final del script. Por ejemplo, para ejecutar un caso con
(p=3):

``` python
simulacionZp(3)
```

Para ejecutar las pruebas automatizadas, desde la raíz del repositorio:

``` bash
python -m pytest -v Test_Automatizado/test_cuaterniones.py
```

También se puede ejecutar el archivo directamente, si conserva el bloque
de ejecución `if __name__ == "__main__":` documentado:

``` bash
python Test_Automatizado/test_cuaterniones.py
```

Los resultados y tiempos de ejecución dependen del valor de (p). Los
programas que recorren todos los pares de cuaterniones realizan (p\^8)
combinaciones, por lo que el costo computacional crece rápidamente.

## Resultados

El repositorio incluye resultados de las ejecuciones realizadas para
distintos valores de (p):

-   **Producto:** archivos de texto con productos de cuaterniones.
-   **Invertibles:** archivos Excel con los pares cuyo producto es la
    identidad.
-   **Estructura:** archivos de texto con los conteos obtenidos en la
    exploración de invertibilidad.
-   **Pruebas automatizadas:** documentación del procedimiento de
    validación de las propiedades algebraicas.

Los archivos de resultados permiten consultar y contrastar las salidas
de los algoritmos. Para reproducirlos, se debe ejecutar el programa
correspondiente con el mismo valor de (p).

## Alcance y consideraciones

Este repositorio combina experimentación computacional y verificación
automatizada para estudiar propiedades del álgebra de cuaterniones sobre
cuerpos finitos.

Es importante tener presentes las siguientes consideraciones:

-   Una simulación sobre un conjunto finito permite verificar los casos
    efectivamente recorridos, pero no reemplaza por sí sola una
    demostración teórica general.
-   Las pruebas exhaustivas cubren todos los elementos o combinaciones
    indicados para los valores de (p) especificados.
-   Las pruebas basadas en muestreo determinista verifican las muestras
    generadas, no todas las ternas posibles.
-   La búsqueda de pares con (xy=e) no debe confundirse, sin una
    verificación adicional, con la comprobación de que (yx=e).
-   La conclusión sobre una estructura de anillo de división requiere
    considerar conjuntamente los axiomas de anillo y la existencia de
    inversos para todos los elementos no nulos.

## Autoría y referencias

**Autoría:** completar con los nombres de las personas responsables del
proyecto.

**Referencias:** agregar aquí los libros, artículos o materiales
matemáticos utilizados para fundamentar el álgebra de cuaterniones, los
cuerpos finitos y las pruebas algebraicas.
