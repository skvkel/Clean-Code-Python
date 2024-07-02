# Clean Code - Python
En este repositorio se nombran y explican los principio del libro Clean
Code, de Robert C. Martin, aplicados al lenguaje de programación Python.
Esta recopilación la he basado en los años que llevo trabajando con 
este lenguaje en congruencia con PEP8. Cabe destacar que, por supuesto, no es
una guía definitiva y algunos desarrolladores de software pueden no estar
de acuerdo con ciertos principios.

Como comento, esta guía complementa a la guía de estilos de Python PEP 8 (Python 
Enhancement Proposal) y está escrita desde un punto de vista objetivo.

## Nombres con sentido
___

**Los nombres deben revelar la intención**.

Si se necesita un comentario, el nombre es incorrecto. En el siguiente ejemplo, necesitamos un comentario para
explicar qué es s, que es b y qué es t. 
````python
# MAL
# s es el salario base, b es el bono, t es el salario total
s = 50000
b = 5000
t = s + b

# BIEN
salario = 50000
base = 5000
salario_total = salario + base
````
___
**Evitar los números mágicos**. 

Cuando utilizamos un número, este número tiene que "tener un nombre". En el 
siguiente ejemplo, ¿qué es 3.14, 7 y 7? Estos son números mágicos. El primero 
es el número PI y el siguiente es el radio.
````python
# MAL
area = 3.14159 * 7 * 7

# BIEN
pi_value = 3.14159
radius = 7
area = pi_value * radius * radius
````
___
**Se deben usar nombres pronunciables**. 

Esto es muy importante a la hora de tener una comunicación efectiva, un code 
review o simplemente una explicación. En el ejemplo, pronunciar el nombre de 
la variable es muy difícil, pudiendo ser mucho mejor first_name y last_name
````python
# MAL
f_n = 'John'
l_n = 'Doe'

# BIEN
first_name = 'John'
last_name = 'Doe'
````
___
**Se deben usar nombres que se puedan buscar**.

Esto se refiere a que se deben
usar nombres que se puedan buscar de forma unívoca. Por ejemplo, si necesitamos
buscar rule_1 (refiriéndonos a la primera), aparecerán todas rule_ que 
tengan un "1" después del guión bajo. Encontrarás rule_11, rule_112412, etc.
````python
# MAL
name_rule = 'rule_1'

# Bien
name_rule = 'rule_001'
````
___
**Evitar la notación húngara**. 

Esto es, no codificar como nombre de una variable el tipo de la misma. Esto 
es una práctica extendida por un programador húngaro que defendía incluir un 
prefijo para denotar el tipo. Sin embargo, esto va en contra del pilar 
fundamental de la POO abstracción. Además, al ser Python un lenguaje de tipado 
dinámico, podemos llamar a una variable con un sufijo de tipo y durante una 
refactorización cambiar su tipo y dejar de ser congruente con el nombre de la 
variable.
**No incluir "I" en las interfaces**. 
Es mejor incluir "Imp" en sus implementaciones (si fuese necesario).
```
IVehiculo --> Coche(IVehiculo)   ERROR
Vehiculo  --> CocheImp(Vehiculo) MEJOR
```
___
**El nombre de una clase no debe ser un verbo**.
````python
# MAL
class Calculate:    # El nombre es un verbo
    def add(self, a, b):
        return a + b

# BIEN
class Calculator:   # El nombre es un sustantivo
    def add(self, a, b):
        return a + b
````
___
**El nombre de un **método** debe ser un verbo**.

Los métodos hacen cosas y es por ello que deben ser verbos.
___

**Utilizar Properties y Setter en vez de un método set_ y get_**

En python, se utiliza el decorador property para implementar un getter. Se 
utiliza el setter para el modificador del atributo. También existe el protocolo
Descriptor, que se utiliza cuando queremos mayor flexibilidad y mejorar la reutilización
entre clases.
````python
class Person:
    def __init__(self, name):
        self._name = name

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        if not value:
            raise ValueError("Name cannot be empty")
        self._name = value
````
___
**Utilizar misma funcionalidad para mismo nombre de función en distintas clases**

Esto quiere decir que si hay un método en una clase con nombre *get_nombre* 
y en otra clase existe un método con ese mismo nombre, los dos deben hacer lo
mismo.

___
**Utilizar nombres técnicos cuando sea posible**

Si utilizamos un patrón, por ejemplo, es mejor incluir el nombre del patrón. Esto
facilita la comprensión, la mantenibilidad, etc. Ayudamos al lector a comprender
lo que queremos hacer.
```
Para patrón VISITOR --> AccountVISITOR()
```
___
**No se debe incluir contexto innecesario**
Esto nos dice que si la clase se define como Coche, en los atributos no se debe
incluir coche_ruedas, por ejemplo. Son atributos de un coche, por lo que es
redundante.
````python
# MAL
class Animal:
    cantidad_patas_animal: str
    cantidad_ojos_animal: int
    
# BIEN
class Animal:
    cantidad_patas: str
    cantidad_ojos: int
````
___
***TIP: si durante el refactor se quejan, no importa. Cambiar a mejor siempre 
es bueno, a corto, medio y largo plazo.***


# Funciones
___
**Las funciones cuentan historias**

Los métodos cuentan historias y deben ser cortas (idealmente 20 líneas). Es un
principio muy difícil de cumplir, pero debemos ajustarnos lo máximo posible a él.
Tanto el nombre como la funcionalidad tienen que contar la historia, por lo que
todo tiene que estar en consonancia, tanto el nombre de la historia (nombre del
método) como la historia (el cuerpo del método).
___
**Dos niveles máximo de anidación por método**

Los métodos deben hacer una única cosa, y la deben hacer bien. Se crearon los 
métodos para descomponer algo de forma atómica. En el momento que una función
no funciona de manera atómica, deja de tener sentido. Este principio también es
difícil de aplicar, pero debemos de ajustarlo lo máximo posible a él.
___
**Un nivel de abstracción por método**

No se debería mezclar, por ejemplo, get_html() y parser.render('test') en el 
mismo método.
___
**La cantidad ideal de argumentos de una función es 0**

La cantidad máxima es 3. En Python, la cantidad máxima sería de 15 antes de que
un lintern nos avise. Además, una cantidad elevada de parámetros puede ser un
***smell code***, ya que podría indicar que el método hace más de una cosa. 
De igual forma, un método con muchos parámetros es un método mucho más difícil 
de testear.
___
**Tratar excepciones fuera del método**

En el caso de Python, podemos utilizar los decoradores para extraer esa captura
externamente a la función. El trato de excepciones es una cosa aparte de la 
funcionalidad de un método, por lo que si lo incluimos en el método, estaría 
haciendo dos cosas.
````python
# MAL
class FileProcessor:
    # Función que hace más de una cosa
    def process_file(self, file_path):
        try:
            with open(file_path, 'r') as file:
                data = file.read()
            return data.upper()
        except FileNotFoundError:
            print(f"Error: El archivo {file_path} no fue encontrado")
            return None
        except IOError as e:
            print(f"Error de E/S: {e}")
            return None

# BIEN
def handle_file_exceptions(func):
    def wrapper(*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except FileNotFoundError:
            print(f"Error: El archivo {args[1]} no fue encontrado")
            return None
        except IOError as e:
            print(f"Error de E/S: {e}")
            return None
    return wrapper

class FileProcessor:
    @handle_file_exceptions
    def process_file(self, file_path):
        with open(file_path, 'r') as file:
            data = file.read()
        return data.upper()
````
___
**Posibilidad de varios return, break o continue**

Dijkstra argumentaba que un único punto de retorno hace que el flujo de
control de un método sea más claro y fácil de seguir. Al usar un único retorno,
se evitan estructuras complejas como múltiples sentencias return, break o 
continue dispersas dentro de la función. Esto reduce la posibilidad de 
errores y hace que el código sea más predecible. Con un solo punto de retorno, 
el proceso de depuración y mantenimiento se simplifica. Los 
desarrolladores pueden concentrarse en un único lugar para verificar y 
modificar la salida del método.

Sin embargo, en Python, el uso de múltiples return, break o continue puede 
hacer que el código sea más legible y expresivo, especialmente en funciones 
cortas y simples. Desde mi punto de vista, escogería la mejor opción en vista a
mejorar la legibilidad y legibilidad del método. Además, al ser los métodos 
pequeños, muy pocas veces necesitaremos una lógica "***compleja***" que necesite
de varios return, break o continues.
___
**Evitar funciones con condición negativa**
Esto nos llevará gran cantidad de veces a confusión, ya que es más dificil 
interpretar una negación de una negación (afirmativo) que una negación simple
o una afirmación simple.
````python
# MAL
def is_not_elegible() -> bool:
    pass

# BIEN
def is_elegible() -> bool:
    pass

````

# Comentarios
**Los comentarios son (casi siempre) un error**. Son provocados por la falta de
expresión en el código. Siempre que podamos reescribir algo para que no haga 
falta el comentario, será bien.

Además, el código cambia, pero comúnmente el comentario se mantiene. Esto ocurre
muy a menudo en los docstring de Python, por lo que hay que cerciorarse de que
si cambia un parámetro, cambiarlo en el docstring.
___
**Los comentarios no compensan código incorrecto o desorganizado**

En el siguiente ejemplo, podremos ver un ejemplo de mala codificación y su 
solución, con la que podemos ver de un vistazo cuál es la intención:

````python
# MAL. Comentario insertado para intentar compensar mal código
# Comprobar si el empleado tiene derecho
if (employee.flags and 
    employee.is_active and 
    employee.age > 65):

# BIEN
if employee.is_eligible():
````
___

**Los comentarios suelen ser útiles para recalcar una advertencia o amplificamos
la importancia de algo**.
___
**Docstring en Python**

El autor Robert C. Martin indica que los docstring no deben existir. Sin embargo,
los docstring son ua herramienta muy útil que codificada correctamente siguiendo
las buenas prácticas, es totalmente válido y muy útil.
___
**El código comentado es un completo error SIEMPRE**

___
**Los comentarios, en el caso de que se deban incluir, deben ser cortos, claros y
concisos**.

___
**Eliminar código muerto**

Puede parecer obvio, pero son muchos los desarrolladores que dejan el código
que no se usa (código muerto) y no lo eliminan. Esto, además de empeorar la 
mantenibilidad del código, lo hace más sucio y menos legible, pudiendo llevar
a confusiones.

# Formato

**Mejor módulos más pequeños**

Es mejor dividir módulos grandes en módulos más pequeños por funcionalidad por
ejemplo.
___
**Importante el docstring del inicio del módulo, explicando su finalidad**

Este comentario es muy importante, ya que nos define qué es el módulo y para 
qué sirve. Es como el titular del periódico. Si estamos buscando algo, leyendo
el "titular" es suficiente para saber si es ahí donde debemos buscar o no.
___
````python
""" This module define......"""


class MyClass:
.....
````
___

**La disposición de los métodos y atributos es:**
1. Inicializador (__init__)
2. Métodos de clase
3. Métodos estáticos
4. Métodos privados
5. Métodos públicos

````python
class Test:
    
    def __init__(self):
        pass
    
    @classmethod
    def class_method(cls):
        pass
    
    @staticmethod
    def static_method():
        pass
    
    def _private_method(self):
        pass
    
    def public_method(self):
        pass
````
___
**Las contantes deben ir declaradas las primeras y en mayúscula**
````python

MY_CONST = 'test'

class MyClass:
....
````
___
**Conceptos o funciones afines deben estar cerca**
Esto nos dice que si varias funciones son afines o hacen cosas comunes o incluso
son "helpers" de otras funciones, deben estar declaradas juntas y no una al 
final y otra al inicio.
# Pruebas de unidad
Las pruebas de unidad son **indispensables**. Un código no debería ser promocionado
a un entorno productivo sin tener una prueba de unidad, como mínimo.
___
**Los tests deben ser limpios**
Las pruebas descuidadas producen una baja mantenibilidad y esto produce grandes
costos a refactorizaciones o cambios en funcionalidades.
___
**Las pruebas posibilidad nuevas features**
Las pruebas nos darán la tranquilidad de poder modificar sin romper algo que 
funciona.
___
**Una prueba por concepto**
En cada uno de los test unitarios, se debe testear un solo concepto.
___
**Los test deben cumplir los principios FIRST:**
1. Rapidez: los test deben ser rápidos. Se deben poder ejecutar a gran velocidad.
2. Independencia: cada uno de los test debe ser totalmente independiente al resto.
3. Repetición: Se debe poder ejecutar en cualquier entorno.
4. Validación automática: las pruebas deben retornar como resultado True o False
5. Puntualidad: los test deben estar antes de promocionar código a producción

# Objetos y estructuras de datos
La ley de Demeter nos dice que un método solo debe invocar a los métodos de:
1. Su clase
2. Un objeto pasado como argumento
3. Un objeto creado por la función
4. Un objeto en una variable de instancia
Esto quiere decir que un método no puede ejecutar métodos de un objeto retornado.
````python
def mi_funcion():
    output = a.get_options().get_last_option()
````
En el ejemplo anterior, hemos utilizado el *get_last_option()* y esto es un error.
___
**Acceso a atributos mediante Descriptores o property y setter**
````python
# MAL
class Developer:
    def __init__(self, age = 0):
        self._age = age
    
    def get_age(self):
        return self._age
    
    def set_age(self, age):
        self._age = age

john = Developer()
john.set_age(21)

# BIEN
class Developer:
    def __init__(self, age = 0):
        self._age = age
    
    @property
    def age(self):
        return self._age
    
    @age.setter
    def age(self, age):
        self._age = age

john = Developer()
john.age = 21
````

# Clases
Las clases se deben organizar de la siguiente manera:
1. Variables de clase públicas
2. Variables de clase privadas
3. Variables de instancia privadas (en __init__ o __new__)
4. Variables de instancia públicas (en __init__ o __new__)
````python
class Ejemplo:
    # 1. Variables de clase públicas
    variable_clase_publica = "Soy una variable de clase pública"
    
    # 2. Variables de clase privadas
    __variable_clase_privada = "Soy una variable de clase privada"

    def __init__(self, valor_publico, valor_privado):
        # 3. Variables de instancia privadas
        self.__variable_instancia_privada = valor_privado
        
        # 4. Variables de instancia públicas
        self.variable_instancia_publica = valor_publico
````
___
# Tamaño reducido
Para nombrar las clases, si el nombre es ambiguo puede ser indicativo de 
*smell code*. Es mejor renombrar a algo más específico y separar en varias
clases.
___
# Tus clases deben cumplir con los principios SOLID
Esta sección es muy extensa, que está documentada al detalle en el repositorio
https://github.com/skvkel/SOLID-Principles


