## Principios SOLID aplicados
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

* Inversión de dependencia: 

  El principio de inversión de dependencia busca reducir el acople entre sistemas de software. 
  En nuestro caso, el sistema independiza la tecnología de bases de datos, ya que podríamos migrar con facilidad a otro sistema de bases de datos, pues utilizamos el framework  Laravel, donde existe una abstracción que nos permite cambiar el tipo de BD utilizada mediante modificar algunos datos de forma sencilla. Esto nos brinda libertad para la implementación y cambiar de tecnología de BD afectando mínimamente las partes del sistema

   <p align="center">
      <img src="/imagenesINGSoft/si.jpeg" >
   </p>
   No dependemos de la tecnología que empleamos en la base de datos, ya que la comunicación entre los componentes del sistema es siempre mediante interfaces, y esto nos permite tener libertad a la hora de decidir las implementaciones concretas de cada elemento. Por ejemplo, podríamos cambiar la conexión a la base de datos de mysql a mongodb o postgresql sin afectar a ninguna parte del sistema.
