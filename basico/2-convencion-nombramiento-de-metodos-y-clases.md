## Convención de nombramiento de métodos y clases.

Tanto las clases y metodos o inclusive las funciones deben tener nombres que respeten la primera
[convención](1-convencion-nombramiento-de-variables.md) pero tambien los nombres que se les ponga deben
expresar exactamente lo que se quiere comunicar.

*Siempre usar CamelCase para el nombre de clases e interfases*

### Nombre de interfases

Las interfases en programación definen un **contrato** a la que podemos agregarle a clases para que respeten
e implementen, para luego ser usados en ciertas condiciones. Es importante tenerlos ya sea en un directorio dedicado,
por ejemplo `Contracts` o si nuestro sistema esta organizado por módulos entonces esta bien ponerlos directo en la
raíz del módulo.

Nombres aceptables para interfaces son:

```php
<?php

interface Notifiable {}

interface NotifiableContract {}

interface HasPosts {}

interface HasPermissions {}

interface CanCreatePermissions {}

interface ShouldQueue {}

interface SantillanaTokenService {}
```

### Nombre de métodos de las interfases

Los métodos en las interfaces además que luego en las mejores practicas de nivel intermedio nos explicarán, estás no debén
tener muchos métodos que implementar y sus nombres tanto los tipos de retorno deben ser concisos.

```php
<?php

interface Notifiable
{
    public function notify(): void;
}

interface NotifiableContract
{
    public function notify(Notification $notification): void;
}

interface HasPosts
{
    public function posts(): HasMany;
}

interface HasPermissions
{
    /** @return Collection<int, Permission> */
    public function permissions(): Collection;
}

interface CanCreatePermissions
{
    /**
     * @param string $permission
     * @param array<int, string> $abilities
     * @return Permission
     */
    public function createPermission(string $permission, array $abilities): Permission;
}

interface ShouldQueue
{
    // Nothing, only used to validate that the job should be pushed to the queue.
}

interface SantillanaTokenService
{
    public function token(): Token;
}
```

### Nombre de clases

Las clases deben expresar el modelo o entidad a la que quieren "modelar" o bien el funcionamiento que representan
explicitamente hay casos especiales como las acciones que simplemente pueden describir la acción en el nombre.

Si las clases tienen alguna implementación o forma muy especifica podemos usar interfases y es recomendable su uso.

```php
<?php

class Person {}

class People implements \Arrayable {}

class CreateUserRequest {}

class CreateUserCommand {}

class Editor extends Person implements Notifiable, HasPosts {}

class Token {}

class SantillanaSIFTokenService implements SantillanaTokenService {}

class SantillanaUNOiTokenService implements SantillanaTokenService {}
```

### Nombre de métodos de las clases

Es importante seguir el principio de [Tell don`t ask](https://martinfowler.com/bliki/TellDontAsk.html). Una de los redflags
en el código es tener getter y setters ser más especificos usando un lenguaje más cerca al contexto del negocio son mejores.

Ejemplos de nombres de métodos más adecuados al negocio y no a la programación:

```php
<?php

class Editor extends Person implements Notifiable, HasPosts
{
    protected string $name;

    public function notify(): void
    {
        //...
    }

    public function posts(): HasMany
    {
        //...
    }

    // En ocasiones cuando tenemos métodos con muchos parámetros de entrada es mejor
    // extraerlos en clases [DTOs](https://martinfowler.com/eaaCatalog/dataTransferObject.html)
    // los constructores son uno de los metodos que puedes no usar esta recomendación
    public static function register(CreateUserRequest $request): static
    {
        // ...
    }

    // En vez de tener un getName() podemos simplemente obtener el nombre así:
    public function name(): string
    {
        return $this->name;
    }

    // En vez de tener un setName(string $name) es mejor describir que se hace, en este caso renombrar
    // por eso el npmbre del metodo queda así:
    public function rename(string $name): void
    {
        $this->name = $name;
    }

    // Usar el lenguaje del negocio por desconocimiento algunos usarian savePost, appendPost y dado que
    // en nuestro lenguaje cualquier post de un editor se publican con solo tenerlo entones publishPost es más
    // adecuado.
    public function publishPost(Post $post): void
    {
        $this->posts()->save($post);
    }
}
```
