## Conceptos DDD aplicados
* Modules:
 
  Cada parte del proyecto tiene un dominio que aísla los códigos formados por módulos de clases relacionadas con una funcionalidad de la aplicación.

   <p align="center">
    <img src="/imagenesINGSoft/dd1(1).png" >
      </p>

* Entities: 

  Existen diferentes entidades en el Sistema que desarrollamos, entre ellas podríamos mencionar las entidades Ponente, Participante , Evento , persona , participante , sesion , administrador, ya que son objeto del dominio que  mantienen un estado y comportamiento más allá de la ejecución de la aplicación. A continuación se muestra la entidad Evento, que posee una identificación única mediante su ID.

   <p align="center">
      <img src="/imagenesINGSoft/eventoid.png" >
       </p>
      
*  Value Objects:

   Los Value Objects (VO) son solo valores, no entidades, por si solos no significan nada, tienen que estar acompañados de una entidad para que signifiquen algo o ser interpretados. 
   En nuestro sistema identificamos algunos Value Objects, como el siguiente: 
    
    <p align="center">
       <img src="/imagenesINGSoft/dd3(1).png" >
      </p>
   La clase SesionEvento si bien aparece como un Entity en realidad solo es un dato que conecta la relacion N-M de las entidades Sesión y Evento y almacena el dato de la hora de Inicio. Sin estas otras entidades perdería sentido por sí misma. Es un Value Object.
 
*  Ubiquitous Language: 

   Es un concepto de gran importancia porque, además de servir de vehículo de entendimiento en el negocio y entre el negocio e IT, también sirve para identificar las particiones del Domain, que darán lugar a soluciones modulares

   <p align="center">
  <img src="/imagenesINGSoft/dd4(1).png" >
    <img src="/imagenesINGSoft/dd5(1).png" >
      </p>

*  Aggregates: 
   Los agregados representan el límite lógico de un conjunto de datos, permiten modelar el sistema en pequeños subconjuntos. Para acceder a los elementos de un agregado debemos  acceder mediante una entidad principal, que le sirve a modo de entrada. 

   Existe un agregado “Sesión” ya que esta entidad posee relaciones con otras a nivel de negocio y su acceso es mediante la entidad Programa.
    
    <p align="center">
  <img src="/imagenesINGSoft/dd6(1).png" >
      <img src="/imagenesINGSoft/dd7(1).png">
      </p>

* Repository:
 
  En nuestro proyecto tenemos clases, o más famosamente llamados controladores (MVC), que tienen la función de repositorio, ya que, son los que controlan, dirigen, a la aplicación   por dónde debe ir dependiendo de lo que el usuario requiera al interactuar con dicha aplicación.

     <p align="center">
       <img src="/imagenesINGSoft/dd8.png" >
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

 
 ```php
<?php

namespace App\Models;

use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Foundation\Auth\User as Authenticatable;
use Illuminate\Notifications\Notifiable;

class User extends Authenticatable
{
    use HasFactory, Notifiable;

    protected $table = 'persona';
    public $timestamps = false;
    protected $primaryKey = 'dni';
    /**
     * The attributes that are mass assignable.
     */
    protected $fillable = [
        'nombre',
        'email',
        'password',
    ];

    /**
     * The attributes that should be hidden for arrays.
     */
    protected $hidden = [
        'password',
    ];

    /**
     * The attributes that should be cast to native types.
     */
    protected $casts = [
        'email_verified_at' => 'datetime',
    ];
}


```
   El principio de responsabilidad única busca que el código quede encapsulado y exista independencia entre las clases, sus funcionalidades. Al utilizar clases hemos procurado   cumplir con este criterio, ya que encapsulamos la funcionalidad de cada una para que realicen una única función. 

   <p align="center">
      <img src="/imagenesINGSoft/respUnica4.png" >
      </p>

   Por ejemplo, en la imagen se ve que se han independizado las funciones, entre ellas editar la sesión, y otras. Esto también ayuda a la reutilización del código en caso de cambios o mantenimiento. 

*  Segregación de la Interfaz: 

   Según este principio es mejor tener una clase pequeña y especializada que una muy grande, para poder hacer un mejor objetivo hacia las necesidades del sistema. 
   En nuestro trabajo, hemos procurado seguir esta norma al no sobrecargar las funcionalidades de las clases sin más de lo que se necesite. Por ejemplo en la captura se ve que      cada función está dirigida a un único fin, aunque pertenezcan al mismo sistema, estan especializados. 

   <p align="center">
     <img src="/imagenesINGSoft/segI.png" >
      </p>

