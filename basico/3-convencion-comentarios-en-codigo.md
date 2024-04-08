## Convención comantarios en Código

Los comentarios en código son nocivos, ya que al pedirle al desarrollador siempre ponerlos solo crean comentario
cin valor alguno y solo por obligación, es más práctico tener código lo suficientemente legible y que con tan solo
verlo entender que sucede, claro en ocaciones no es posible y es ahí cuando un comentario de la intención del siguiente
pedazo de código es importante ya sea para especificar que el sigiente código es de vital importancia o si algo falta
para mejorar su implementación.

```php
class CheckCorrectUserLoginHash
{
    public function handle(Request $request, Closure $next): Response
    {
        $user = $request->user('user');

        if ($user && $request->session()->get('session_login_hash') !== $user->session_login_hash) {
            /** Login hash is different so the user logged out before with the openid client */
            if ($request->hasSession()) {
                $request->session()->invalidate();
                $request->session()->regenerateToken();
            }

            return redirect()->route('santillana.home');
        }

        return $next($request);
    }
}
```

Tambien código donde uno crea que un colaborador le cuente trabajo entender el por qué de ciertas desiciones.

```php
final class FindQuestionnaireByCode
{
    public function __invoke(string $code): Questionnaire
    {
        /**
         * This action was made to cache the questionnaire so we can hit less the database.
         */
        return cache()->remember(
            sprintf('questionnaire_%s', $code),
            now()->addDay(),
            fn () => Questionnaire::with(['questions'])->where('code', $code)->firstOrFail(),
        );
    }
}
```