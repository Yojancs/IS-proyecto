## Práctica de código legible aplicadas

* No colocar JS ni CSS en las plantillas Blade y no coloques HTML en clases de PHP
* Convención de Laravel para los nombres, organización de archivos y carpetas:
  Se siguen los [estándares PSR](http://www.php-fig.org/psr/psr-2/), también, sigue la convención aceptada por la comunidad. Para la organización de archivos y caroetas , técnicamente, se podría escribir el código de una aplicación completo dentro de un solo archivo. Pero eso resultaría en una pesadilla para leer y mantener. Es por ello que siguiendo el MVC se organizaron las carpetas y archivos. 



Qué | Cómo | Bueno | Malo
------------ | ------------- | ------------- | -------------
Controlador | singular | ControladorArticulo | ~~ControladorArticulos~~
Relaciones hasOne o belongsTo | singular | comentarioArticulo | ~~comentariosArticulo, comentario_articulo~~
Propiedad de modelo | snake_case | $model->created_at | ~~$model->createdAt~~
Método | camelCase | traerTodo | ~~traer_todo~~
Vistas | kebab-case | show-filtered.blade.php | ~~showFiltered.blade.php, show_filtered.blade.php~~

A continuación se mostrará la organización de carpetas y archivos, así como el nombramiento de los modelos, controladores, y vistas:
  <p align="center">
    <img src="/imagenesINGSoft/bp-1.png">
    <img src="/imagenesINGSoft/bp-2.png">
    <img src="/imagenesINGSoft/bp-3.png">
  </p>
 Convención de nombres para relaciones de hasOne o belongsTo

 ```php
 public function sesionEvento()
 {
    return $this->hasOne(SesionEvento::class, 'id_evento');
 }
 ```
Convención de nombres para los métodos
 ```php
    // Muestra opciones para el tipo fe evento que se quiera crear
    public function showCreateOptions($id_sesion)
    {
        return view('evento.show-create-options', compact('id_sesion'));
    }

    // Muestra eventos en específico que pertenecen a una sesión
    public function show(sesion $sesion)
    {
        return view('evento.show', compact('sesion'));
    }

    //Función para eliminar un vento
    public function destroy(evento $evento)
    {
        $evento->delete();
        return redirect()->route('home.index');
    }
```

Convención de nombre para las propiedades de modelo
```php
$evento->concurso->update(request()->only(['num_participantes', 'requisitos', 'ganadores', 'moderador']));
$evento->sesionEvento->where('id_evento', $evento->id)->update(request()->only(['hora_inicio']));
```
* Utiliza sintaxis cortas y legibles siempre que sea posible

Sintaxis común | Sintaxis corta y legible
------------ | -------------
`Session::get('cart')` | `session('cart')`
`$request->session()->get('cart')` | `session('cart')`
`Session::put('cart', $data)` | `session(['cart' => $data])`
`$request->input('name'), Request::get('name')` | `$request->name, request('name')`
`return view('index')->with('title', $title)->with('client', $client)` | `return view('index', compact('title', 'client'))`

Ejemplos: 
```php
if(Administrador::find(Auth::user()->dni) != NULL)
  session(['isAdmin' => true]);
else
  session(['isAdmin' => false]);

return redirect('home');

```

```php
public function show(sesion $sesion)
{
  return view('evento.show', compact('sesion'));
}

```

* No ejecutar consultas en las plantillas Blade y utiliza el cargado prematuro (Problema N + 1)

Malo (Para 100 ponentes, se ejecutarán 101 consultas):

```php
@foreach (Ponente::all() as $ponente)
    {{ $ponente->profile->name }}
@endforeach
```

Bueno:
```php
@foreach ($ponentes as $ponente)
    <h3><a href="speaker-details.html">{{ $ponente->nombre}}</a></h3>
    <p>{{ $ponente->grado_academico }}</p>
    <p><strong>Especialidad: </strong>{{ $ponente->especialidad }}</p>
    <div class="social">
        <a href="{{ route('ponente.mostrar',$ponente->dni) }}" class="btn btn-outline-light">Ver más</a>
     </div>
@endforeach
```
* No colocar ningún tipo de lógica en los archivos de rutas.
```php
// Rutas de Concurso
Route::get('/concurso/crear/{id_sesion}', [ConcursoController::class, 'create'])->name('concurso.crear');
Route::post('/concurso/guardar/{sesion}', [ConcursoController::class, 'store'])->name('concurso.guardar');
Route::get('/concurso/editar/{evento}', [ConcursoController::class, 'edit'])->name('concurso.editar');
Route::put('/concurso/{evento}', [ConcursoController::class, 'update'])->name('concurso.actualizar');

// Rutas de ponencia
Route::get('/ponencia/crear/{id_sesion}', [PonenciaController::class, 'create'])->name('ponencia.crear');
Route::post('/ponencia/guardar/{sesion}', [PonenciaController::class, 'store'])->name('ponencia.guardar');
Route::get('/ponencia/editar/{evento}', [PonenciaController::class, 'edit'])->name('ponencia.editar');
Route::put('/ponencia/{evento}', [PonenciaController::class, 'update'])->name('ponencia.actualizar');


```

* Identación consistente:

   Siempre sera una buena praxis mantener una indentación ordenada. No hay un estilo ’mejor’ que todos deberían seguir. En realidad, el mejor estilo, es un estilo consistente. Si se es parte de un equipo o si se está contribuyendo con código a un proyecto, se debe seguir el estilo existente que se está utilizando en ese proyecto.También vale la pena señalar que es una buena idea mantener su estilo de identación de una manera coherente.
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Models\Administrador;
use Auth;

class LoginController extends Controller
{
    public function ingresar()
    {
        $credenciales = request()->only(['email', 'password']);
        if (Auth::attempt($credenciales))
        {
            request()->session()->regenerate();

            if(Administrador::find(Auth::user()->dni) != NULL)
                session(['isAdmin' => true]);
            else
                session(['isAdmin' => false]);

            return redirect('home');
        }
        else
        {
            return redirect()->route('login');
        }    
    }

    public function salir()
    {
        Auth::logout();
        request()->session()->invalidate();
        request()->session()->regenerate();
        return redirect('home');
    }
}
```

   
* Agrupación de código:

   Casi siempre ciertas tareas requieren unas pocas líneas de código. Es una buena idea mantener estas tareas dentro de bloques separados de código, con algunos espacios entre ellos.
```php
    /**
     * Muestra opciones para el tipo fe evento que se quiera crear
     */
    public function showCreateOptions($id_sesion)
    {
        return view('evento.show-create-options', compact('id_sesion'));
    }

    /**
     * Muestra eventos en específico que pertenecen a una sesión
     */
    public function show(sesion $sesion)
    {
        return view('evento.show', compact('sesion'));
    }

    
    /**
     * Función para eliminar un vento
     */
    public function destroy(evento $evento)
    {
        $evento->delete();
        return redirect()->route('home.index');
    }
```

* Longitud límite de línea:

   Es menos cansado leer 5 filas de amplitud corta que un largo texto a ras de hoja, por ello es una buena prática evitar escribir líneas largas y horizontales.

## Estilos de Programación aplicados
* Programacion Orientada a Objetos: 
El estilo de programación utilizado fue siguiendo el paradigma orientado a objetos, basándonos en el concepto de clases y objetos. Este tipo de programación se utiliza para estructurar un programa de software en piezas simples y reutilizables de planos de código (clases) para crear instancias individuales de objetos. 

* 	Código mantenible: 
El sistema está diseñado de forma que pueda ser actualizado cada cierto tiempo, con independencia entre sus funciones y clases, logrando así el programa perdure.
 
* Composición de funciones:
  Esta sección muestra como se conectan las llamadas a funciones. Se utiliza un estilo de paso de continuación, donde a cada función se le da también la siguiente función que debe ser llamada.
  <p align="center">
      <img src="/imagenesINGSoft/estilos.jpeg" >
      <img src="/imagenesINGSoft/estilos1.jpeg" >
      </p>
