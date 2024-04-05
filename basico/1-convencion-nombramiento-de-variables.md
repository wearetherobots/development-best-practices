## Convención de nombramiento de variables.

Es importante siempre tener buenas prácicas en nombrar variables tanto de lo que hacen
igual de la forma de escribirlos ya sea en `snakecase`, `camelcase`, `pascalcase`,
`kebabcase` o algún otro.

Esto principalmente para que nuestro código no parezca escrito por diferentes personas y
que las variables debén tener nombres que representen de forma rápida lo que hacen. tener nombres
descriptivos es más importante tener nombres cortos y que para saber que es lo que hacen uno tenga
que revisar el código para ver que dato esta alojando la variable.

Ejemplos de malas prácticas en el nombramiento de variables:

```php
<?php

class personStudent
{
    public $studentID;
    public $name;
    public $first_Name;
    public $last_name;
    public $Age;

    public function __construct($ID, $name, $first, $Last, $age)
    {
        $this->studentID = $ID;
        $this->name = $name;
        $this->first_Name = $first;
        $this->last_name = $Last;
        $this->Age = $age;
    }
}

$s = new personStudent('abc123', 'José', 'Pérez', 'López', 16);

$adulto = $s->Age >= 18;
```

Cosas que podemos aprender de este mál código es que aunque es funcional no tiene buenos nombres de
variables porque está en diferentes formatos, la variable `$s` a simple vista no inferimos a que
se refiere y la variable `$adulto` pareciera que se refiere a una persona mayor pero su objetivo
contener si la persona es mayor o no.

Una forma de limpiar el código es decidiendo que forma escribiremos las variables o nombres de
clase luego, esto no significa que todo debé ser igual si no para ciertas cosas como los atributos
de los objetos podemos usar una forma y las variables dentro de funciones, métodos o en general de
otra. Por ejemplo:

```php
<?php

class Student
{
    public $id;
    public $name;
    public $firstName;
    public $lastName;
    public $age;

    public function __construct($id, $name, $first, $last, $age)
    {
        $this->id = $id;
        $this->name = $name;
        $this->firstName = $first;
        $this->lastName = $last;
        $this->age = $age;
    }
}

$student = new Student('abc123', 'José', 'Pérez', 'López', 16);

$isAdult = $student->age >= 18;
```

En esta iteración mejoramos con tan solo en renombrar variables, y el código es más legible. Pero
podemos ir un poco más allá y tener una convención de tener los atributos de clase en snake_case
para aprovechar cosas nuevas que salen en los lenguajes de programación y que sean más bellos cuando
en conjunto de otras cosas como arreglos podamos tener un código más limpio.

```php
<?php

class Student
{
    public function __construct(
        public $id,
        public $name,
        public $first_name,
        public $last_name,
        public $age,
    ) {
        //
    }
}

$studentData = [
    'id' => 'abc123',
    'name' => 'José',
    'first_name' => 'Pérez',
    'last_name' => 'López',
    'age' => 16,
];

/** Si los datos vienen de una base de datos podemos inclusove pasar así los datos */
$student = new Student(...$studentData);

$isAdult = $student->age >= 18;

/** Si queremos ser más explicitos versiones recientes nos dejan hacer cosas así */
$secondStudent = new Student(
    id: 'abc124',
    name: 'María',
    first_name: 'Moreno',
    last_name: 'Madrigal',
    age: 15,
);
```
