# Álgebra de cuaterniones sobre cuerpos finitos

Repositorio académico dedicado al estudio algebraico y computacional de los **cuaterniones con coeficientes en los cuerpos finitos** $\mathbb{Z}_p$, donde $p$ es un número primo. Contiene algoritmos implementados en Python para calcular productos, explorar elementos invertibles y examinar propiedades de la estructura algebraica, así como una suite de pruebas automatizadas.

## Contenido

- [Descripción del proyecto](#descripción-del-proyecto)
- [Fundamento matemático](#fundamento-matemático)
- [Organización del repositorio](#organización-del-repositorio)
- [Descripción de las carpetas](#descripción-de-las-carpetas)
- [Pruebas automatizadas](#pruebas-automatizadas)
- [Requisitos](#requisitos)
- [Instalación y ejecución](#instalación-y-ejecución)
- [Resultados](#resultados)
- [Alcance y consideraciones](#alcance-y-consideraciones)

## Descripción del proyecto

El proyecto estudia operaciones y propiedades de los cuaterniones sobre $\mathbb{Z}_p$ mediante algoritmos desarrollados en Python. Los cuaterniones se representan mediante cuatro componentes y se realizan recorridos computacionales sobre los elementos del conjunto para examinar su comportamiento bajo el producto de Hamilton.

El repositorio se organiza en cuatro carpetas:

1. **Producto:** calcula el producto de pares de cuaterniones.
2. **Invertibles:** busca pares de cuaterniones cuyo producto es la identidad y almacena los resultados.
3. **Estructura:** explora computacionalmente cuántos elementos admiten un elemento que los multiplique para obtener la identidad.
4. **Test_Automatizado:** contiene pruebas unitarias para verificar propiedades algebraicas seleccionadas.

## Fundamento matemático

Un cuaternión se representa como

$$
q=a+bi+cj+dk,
$$

donde $a,b,c,d\in\mathbb{Z}_p$. Sus unidades imaginarias satisfacen las relaciones de Hamilton:

$$
i^2=j^2=k^2=-1,
$$

$$
ij=k,\qquad jk=i,\qquad ki=j,
$$

$$
ji=-k,\qquad kj=-i,\qquad ik=-j.
$$

En los programas, cada cuaternión se representa mediante una cuádrupla:

$$
q=(a,b,c,d)\in\mathbb{Z}_p^4.
$$

Para dos cuaterniones

$$
q_1=(a_1,b_1,c_1,d_1),\qquad
q_2=(a_2,b_2,c_2,d_2),
$$

el producto de Hamilton, reducido módulo $p$, se calcula como

$$
\begin{aligned}
q_1q_2=\big(&
a_1a_2-b_1b_2-c_1c_2-d_1d_2,\\
& a_1b_2+b_1a_2+c_1d_2-d_1c_2,\\
& a_1c_2-b_1d_2+c_1a_2+d_1b_2,\\
& a_1d_2+b_1c_2-c_1b_2+d_1a_2
\big)\pmod p.
\end{aligned}
$$

El elemento identidad multiplicativa es

$$
e=(1,0,0,0).
$$

El producto no es conmutativo en general. Por ejemplo,

$$
ij=k,\qquad ji=-k.
$$

### Norma e invertibilidad

Para $q=(a,b,c,d)$, la norma reducida utilizada en la suite de pruebas es

$$
N(q)=a^2+b^2+c^2+d^2\pmod p.
$$

Cuando $N(q)\neq 0$ en $\mathbb{Z}_p$, la implementación calcula el inverso multiplicativo mediante

$$
q^{-1}=\overline{q}\,N(q)^{-1},
$$

donde el conjugado está dado por

$$
\overline{q}=(a,-b,-c,-d),
$$

y $N(q)^{-1}$ representa el inverso multiplicativo de la norma módulo $p$. En el código de pruebas, si la norma es congruente con cero módulo $p$, la función `quat_inv` devuelve `None`.

El conjunto de cuádruplas tiene $p^4$ elementos. Para los primos impares considerados en la suite, el conteo teórico de elementos invertibles que se utiliza como referencia es

$$
\left|H(\mathbb{Z}_p)^\times\right|
=(p^2-1)(p^2-p)
=p^4-p^3-p^2+p.
$$

## Organización del repositorio

La estructura general del repositorio es la siguiente:

```text
.
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
├── Test_Automatizado/
│   ├── test_cuaterniones.py
│   └── test_cuaterniones.pdf
│
└── README.md
```

> **Nota:** el árbol utiliza los nombres de archivos informados para el repositorio. GitHub distingue entre mayúsculas y minúsculas en los nombres. Se conserva `esctructura.py` tal como fue escrito en la lista de archivos; si el archivo tiene otro nombre en GitHub, actualiza esta referencia. Verifica también si `ProgramaInvertibles` es un archivo adicional y cuál es su extensión.

Los documentos `.docx` describen los algoritmos. Los archivos `.txt` contienen versiones de los programas o resultados de ejecución. Los archivos `.py` son los scripts de Python y los archivos `.xlsx` almacenan resultados tabulados.

## Descripción de las carpetas

### 1. `Producto`

**Propósito:** calcular el producto de dos cuaterniones para los pares generados a partir de los coeficientes de $\mathbb{Z}_p$.

El programa:

- Construye dos cuaterniones $x$ e $y$, recorriendo sus cuatro componentes en $\{0,1,\ldots,p-1\}$.
- Calcula el producto de Hamilton $z=x\cdot y$.
- Reduce cada componente del resultado módulo $p$.
- Imprime los factores y el producto obtenido.

Los archivos de resultados permiten consultar las salidas generadas para los valores de $p$ incluidos en la carpeta.

### 2. `Invertibles`

**Propósito:** identificar pares de cuaterniones cuyo producto es la identidad multiplicativa.

El programa recorre pares ordenados $x,y\in\mathbb{Z}_p^4$, calcula $z=x\cdot y$ y selecciona los casos en los que

$$
x\cdot y=(1,0,0,0).
$$

Los pares encontrados se organizan en una tabla con las componentes de $x$, $y$ y $z$. El script utiliza `pandas` para exportar los resultados a archivos de Excel (`.xlsx`).

Esta búsqueda permite explorar computacionalmente las relaciones de invertibilidad. La verificación explícita de la existencia de inversos por ambos lados también se incluye en la suite de pruebas automatizadas.

### 3. `Estructura`

**Propósito:** explorar computacionalmente cuántos elementos admiten un elemento que, al multiplicarlos, produce la identidad.

Para cada elemento $x$, el programa recorre posibles elementos $y$ y calcula el producto. Identifica los casos en los que

$$
x\cdot y=(1,0,0,0).
$$

El algoritmo cuenta los elementos para los cuales encuentra al menos un candidato y presenta el conteo `Dav`, junto con el valor $p^4$. La comparación se utiliza para examinar si todos los elementos no nulos cuentan con un candidato a inverso por el lado evaluado por el programa.

Esta exploración computacional es un criterio de análisis, pero por sí sola no demuestra que la estructura sea un anillo de división. Para establecer esa conclusión se requiere verificar los axiomas de anillo y la existencia de inversos multiplicativos bilaterales para todos los elementos no nulos.

### 4. `Test_Automatizado`

**Propósito:** validar mediante pruebas unitarias propiedades algebraicas de los cuaterniones sobre $\mathbb{Z}_p$.

El documento `test_cuaterniones.pdf` describe una suite implementada con `pytest`. Las pruebas se organizan en seis bloques:

1. **Identidad multiplicativa:** comprueba que $e\cdot q=q\cdot e=q$.
2. **Cuadrados de las unidades imaginarias:** verifica que $i^2=j^2=k^2=-1\pmod p$.
3. **No conmutatividad:** comprueba las relaciones de Hamilton, incluyendo $ij=k$ y $ji=-k$.
4. **Asociatividad del producto:** verifica que $x\cdot(q\cdot h)=(x\cdot q)\cdot h$.
5. **Distributividad por izquierda y derecha:** comprueba las dos igualdades
   $$
   x\cdot(q+h)=x\cdot q+x\cdot h,
   $$
   $$
   (q+h)\cdot x=q\cdot x+h\cdot x.
   $$
6. **Invertibilidad bilateral y conteo de unidades:** verifica que $q\cdot u=e=u\cdot q$ y compara el número de elementos invertibles con
   $$
   p^4-p^3-p^2+p.
   $$

La estrategia descrita en el documento combina recorridos exhaustivos y muestreo determinista:

- Para algunas propiedades se recorren todos los elementos de $\mathbb{Z}_p^4$.
- Para $p=3$, la asociatividad y la distributividad se verifican exhaustivamente sobre las ternas.
- Para primos mayores, se utilizan muestras deterministas de ternas para limitar el costo computacional.
- Se emplean semillas fijas para que las muestras sean reproducibles.

El PDF documenta las funciones implementadas, las propiedades verificadas, los primos utilizados y el código de la suite.

## Requisitos

- Python 3.10 o superior para la sintaxis de anotaciones de tipos utilizada en la suite documentada.
- `pandas` para exportar los resultados de invertibilidad a Excel.
- `openpyxl` como motor de escritura de archivos `.xlsx`.
- `pytest` para ejecutar las pruebas automatizadas.

Instala las dependencias con:

```bash
python -m pip install pandas openpyxl pytest
```

Los programas de producto y estructura utilizan las operaciones básicas de Python y no requieren `pandas` para los cálculos descritos.

## Instalación y ejecución

Clona el repositorio:

```bash
git clone URL_DEL_REPOSITORIO
```

Ingresa a la carpeta del repositorio:

```bash
cd NOMBRE_DEL_REPOSITORIO
```

Ejecuta cada programa desde su carpeta correspondiente. Por ejemplo:

```bash
cd Producto
python producto.py
```

Para ejecutar el programa de invertibilidad:

```bash
cd Invertibles
python invertibles.py
```

Para ejecutar el programa de estructura:

```bash
cd Estructura
python esctructura.py
```

**Importante:** los programas compartidos definen la función `simulacionZp(p)`. Si el script no contiene una llamada a esa función, debe invocarse desde el intérprete de Python o agregarse una llamada al final del archivo. Por ejemplo, para ejecutar el caso $p=3$:

```python
simulacionZp(3)
```

Para ejecutar las pruebas automatizadas desde la raíz del repositorio:

```bash
python -m pytest -v Test_Automatizado/test_cuaterniones.py
```

Si el archivo conserva el bloque de ejecución directa `if __name__ == "__main__":` descrito en el PDF, también se puede ejecutar mediante:

```bash
python Test_Automatizado/test_cuaterniones.py
```

Los programas que recorren todos los pares de cuaterniones realizan $p^8$ combinaciones. Por tanto, el costo computacional aumenta rápidamente a medida que crece $p$.

## Resultados

El repositorio incluye resultados de ejecuciones para distintos valores de $p$:

- **Producto:** archivos de texto con productos de cuaterniones.
- **Invertibles:** archivos Excel con los pares cuyo producto es la identidad.
- **Estructura:** archivos de texto con los conteos obtenidos en la exploración de invertibilidad.
- **Pruebas automatizadas:** documentación del procedimiento de validación de propiedades algebraicas.

Estos archivos permiten consultar y contrastar las salidas de los algoritmos. Para reproducir los resultados, ejecuta el programa correspondiente con el valor de $p$ indicado en el nombre del archivo.

## Alcance y consideraciones

Este repositorio combina experimentación computacional y pruebas automatizadas para estudiar operaciones y propiedades de los cuaterniones sobre cuerpos finitos.

Es importante considerar que:

- Una simulación permite verificar los casos efectivamente recorridos, pero no reemplaza por sí sola una demostración matemática general.
- Las pruebas exhaustivas cubren todos los elementos o combinaciones indicados para los valores de $p$ especificados.
- Las pruebas basadas en muestreo determinista verifican las muestras generadas, no todas las ternas posibles.
- Encontrar un elemento $y$ tal que $x\cdot y=e$ no debe confundirse, sin una verificación adicional, con comprobar que $y\cdot x=e$.
- Para concluir que una estructura es un anillo de división se deben verificar los axiomas de anillo y la existencia de inversos bilaterales para todos los elementos no nulos.

## Autoría y referencias

**Autoría:** completar con los nombres de las personas responsables del proyecto.

**Referencias bibliográficas:** agregar los libros, artículos o materiales utilizados para fundamentar el álgebra de cuaterniones, los cuerpos finitos y las pruebas algebraicas.

---

Repositorio desarrollado con fines académicos y de experimentación computacional en álgebra abstracta.


**Autoría:** completar con los nombres de las personas responsables del
proyecto.

**Referencias:** agregar aquí los libros, artículos o materiales
matemáticos utilizados para fundamentar el álgebra de cuaterniones, los
cuerpos finitos y las pruebas algebraicas.
