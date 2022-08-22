## Proyecyo de Software - Biblioteca Digital

* Integrantes


-Arodi Carhuas Heredia


-Alvaro Panibra Choctaya


-Cristhian Junior Mendoza



## Estilos de Programación aplicados
* Programacion Orientada a Objetos: 
El estilo de programación utilizado fue siguiendo el paradigma orientado a objetos, basándonos en el concepto de clases y objetos. Este tipo de programación se utiliza para estructurar un programa de software en piezas simples y reutilizables de planos de código (clases) para crear instancias individuales de objetos. 
<p align="center">
    <img src="/imagenesINGSoft/apersonaestilos.png" >
      </p>  
 
* Composición de funciones:
  Esta sección muestra como se conectan las llamadas a funciones. Se utiliza un estilo de paso de continuación, donde a cada función se le da también la siguiente función que debe ser llamada.
  <p align="center">
      <img src="/imagenesINGSoft/aestilos2.png" >
      </p>
      
* 	Código mantenible: 
El sistema está diseñado de forma que pueda ser actualizado cada cierto tiempo, con independencia entre sus funciones y clases, logrando así el programa perdure.
 <p align="center">
      <img src="/imagenesINGSoft/aestilos3.png" >
      </p>
      
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

 
## Conceptos DDD aplicados

* Modules:
 
  Cada parte del proyecto tiene un dominio que aísla los códigos formados por módulos de clases relacionadas con una funcionalidad de la aplicación.

   <p align="center">
    <img src="/imagenesINGSoft/dd1(1).png" >
      </p>

*  Ubiquitous Language: 

   Es un concepto de gran importancia porque, además de servir de vehículo de entendimiento en el negocio y entre el negocio e IT, también sirve para identificar las particiones del Domain, que darán lugar a soluciones modulares

   <p align="center">
  <img src="/imagenesINGSoft/alenguajeubicuo1.png" >
    <img src="/imagenesINGSoft/alenguajeubicuo2.png" >
      </p>

* Entities: 

  Existen diferentes entidades en el Sistema que desarrollamos, entre ellas podríamos mencionar las entidades Ponente, Participante , Evento , persona , participante , sesion , administrador, ya que son objeto del dominio que  mantienen un estado y comportamiento más allá de la ejecución de la aplicación. A continuación se muestra la entidad Evento, que posee una identificación única mediante su ID.

   <p align="center">
      <img src="/imagenesINGSoft/eventoid.png" >
       </p>
      
*  Aggregates: 
   Los agregados representan el límite lógico de un conjunto de datos, permiten modelar el sistema en pequeños subconjuntos. Para acceder a los elementos de un agregado debemos  acceder mediante una entidad principal, que le sirve a modo de entrada. 

   Existe un agregado “Sesión” ya que esta entidad posee relaciones con otras a nivel de negocio y su acceso es mediante la entidad Programa.
    
    <p align="center">
  <img src="/imagenesINGSoft/aagregate.png" >
      </p>

*  Value Objects:

   Los Value Objects (VO) son solo valores, no entidades, por si solos no significan nada, tienen que estar acompañados de una entidad para que signifiquen algo o ser interpretados. 
   En nuestro sistema identificamos algunos Value Objects, como el siguiente: 
    
    <p align="center">
       <img src="/imagenesINGSoft/dd3(1).png" >
      </p>
   La clase SesionEvento si bien aparece como un Entity en realidad solo es un dato que conecta la relacion N-M de las entidades Sesión y Evento y almacena el dato de la hora de Inicio. Sin estas otras entidades perdería sentido por sí misma. Es un Value Object.

* Repository:
 
  En nuestro proyecto tenemos clases, o más famosamente llamados controladores (MVC), que tienen la función de repositorio, ya que, son los que controlan, dirigen, a la aplicación   por dónde debe ir dependiendo de lo que el usuario requiera al interactuar con dicha aplicación.

     <p align="center">
       <img src="/imagenesINGSoft/arepositorio.png" >
     </p>

  Un claro ejemplo sería en controlador de Sesión, que guarda diferentes funciones en donde consultamos dichos datos a la base de datos para que luego sean mostrados al usuario.
  
 
## Principios SOLID aplicados
* Inversión de dependencia: 

  El principio de inversión de dependencia busca reducir el acople entre sistemas de software. 
  En nuestro caso, el sistema independiza la tecnología de bases de datos, ya que podríamos migrar con facilidad a otro sistema de bases de datos, pues utilizamos el framework  Laravel, donde existe una abstracción que nos permite cambiar el tipo de BD utilizada mediante modificar algunos datos de forma sencilla. Esto nos brinda libertad para la implementación y cambiar de tecnología de BD afectando mínimamente las partes del sistema

   <p align="center">
      <img src="/imagenesINGSoft/asolid1.png" >
   </p>
   No dependemos de la tecnología que empleamos en la base de datos, ya que la comunicación entre los componentes del sistema es siempre mediante interfaces, y esto nos permite tener libertad a la hora de decidir las implementaciones concretas de cada elemento. Por ejemplo, podríamos cambiar la conexión a la base de datos de mysql a mMngodb o Postgresql sin afectar a ninguna parte del sistema.
   
   
* Single-responsability : 
  El principio de responsabilidad única ( SRP ) es un principio de programación de computadoras que establece que cada módulo , clase o función en un programa de computadora debe   tener responsabilidad sobre una sola parte de la funcionalidad de ese programa , y debe encapsular esa parte. Todo eso de la función módulo, clase o servicios deben estar alineados estrechamente con esa responsabilidad.
  - Si una Clase tiene muchas responsabilidades, aumenta la posibilidad de errores porque hacer cambios en una de sus responsabilidades podría afectar a las otras sin que usted     lo sepa.
  -  "Una clase debe tener solo una razón para cambiar"
  Se eligió esta clase porque cumple con las características de este, es decir, la Clase User se encarga únicamente de recopilar la información de una persona como nombre, email,  password.

 <p align="center">
      <img src="/imagenesINGSoft/asolid2.png" >
   </p>
  Se busca que el código quede encapsulado y exista independencia entre las clases, sus funcionalidades. Al utilizar clases hemos procurado   cumplir con este criterio, ya que encapsulamos la funcionalidad de cada una para que realicen una única función. 

   <p align="center">
      <img src="/imagenesINGSoft/asolid22.png" >
      </p>

   Por ejemplo, en la imagen se ve que se han independizado las funciones, entre ellas actualidzar la sesiosn, y otras. Esto también ayuda a la reutilización del código en caso de cambios o mantenimiento. 

*  Segregación de la Interfaz: 

   Según este principio es mejor tener una clase pequeña y especializada que una muy grande, para poder hacer un mejor objetivo hacia las necesidades del sistema. 
   En nuestro trabajo, hemos procurado seguir esta norma al no sobrecargar las funcionalidades de las clases sin más de lo que se necesite. Por ejemplo en la captura se ve que      cada función está dirigida a un único fin, aunque pertenezcan al mismo sistema, estan especializados. 

   <p align="center">
     <img src="/imagenesINGSoft/asolid3.png" >
      </p>

