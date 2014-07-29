CRUD-con-PDO-y-redbean
======================

CRUD para el proyecto con PDO y redbean
CREATE TABLE IF NOT EXISTS Aspirantes(
id_aspirantes int(11) NOT NULL AUTO_INCREMENT,
cedula VARCHAR(100) NOT NULL,
nombre VARCHAR(100) NOT NULL,
apellido VARCHAR(100) NOT NULL,
direccion VARCHAR(100) NOT NULL,
telefono VARCHAR(100) NOT NULL,
mail VARCHAR(100) NOT NULL,
equipo_procedencia VARCHAR(500) NOT NULL,
posicion_preferida VARCHAR(100) NOT NULL,
fecha_nacimiento VARCHAR(100) NOT NULL, 
nacionalidad VARCHAR(100) NOT NULL,
partidos_jugados int(100) NOT NULL,
cantidad_lesiones int(10) NOT NULL,

PRIMARY KEY (id_aspirantes));



<?php

class Conexion
{
    public function conectar() {
        $root = 'root';
        $password = 'password';
        $host = 'localhost';
        $dbname = 'futbolclub';
        
        return $conexion = new PDO("mysql:host=$host;dbname=$dbname;", $root, $password); 
    }
    
    
    
    
}



<?php

class Crud{
    public $insertInto;
    public $insertColumns;
    public $insertValues;
    public $mensaje;
    public $select;
    public $from;
    public $condition;
    public $rows;


    public function Create(){
    $model=new Conexion;
   
    $conexion = $model->conectar();
    $insertInto = $this->insertInto;
    $insertColumns = $this->insertColumns;
    $insertValues = $this->insertValues;
    $sql = "INSERT INTO $insertInto ($insertColumns) VALUES ($insertValues)";
    $consulta = $conexion->prepare{$sql};
    if (!$consulta) 
    {
        $this->mensaje = "Error al crear el registro";
    }
    else
    {
        $this->execute;
        $this->mensaje = "Registro creado satisfactoriamente";
    }
        
    }
    
    public function Read() {
        $model = new Conexion;
        $conexion = $model->conectar();
        $select=  $this->select;
        $from = $this->from;
        $condition = $this->condition;
        if ($condition != '') {
            $conition= " WHERE ".$condition;
        }
        $sql ="SELECT $select FROM $from $condition";
        $consulta = $conexion->prepare($sql);
        $consulta=execute;
        while ($filas=$consulta->fetch())
                {
                $this->rows[]= $filas;
                }
        
    }
}


<?php
require 'Crud/Conexion.php';
require 'Crud/Crud.php';

$mensaje = null;
if (isset($_POST["create"])) {
 
    $cedula = htmlspecialchars($_POST['cedula']);
    $nombre = htmlspecialchars($_POST['nombre']);
    $apellido = htmlspecialchars($_POST['apellido']);
    $direccion = htmlspecialchars($_POST['direccion']);
    $telefono = htmlspecialchars($_POST['telefono']);
    $mail = htmlspecialchars($_POST['mail']);
    $equipo_procedencia = htmlspecialchars($_POST['equipo_procedencia']);
    $posicion_preferida = htmlspecialchars($_POST['posicion_preferida']);
    $fecha_nacimiento = htmlspecialchars($_POST['fecha_nacimiento']);
    $nacionalidad= htmlspecialchars($_POST['nacionalidad']);
    $partidos_jugados= htmlspecialchars($_POST['partidos_jugados']);
    $cantidad_lesiones = htmlspecialchars($_POST['cantidad_lesiones']);
    
    if (!is_numeric($cantidad_lesiones)) {
        $mensaje="Error, el campo cantidad de lesiones debe ser numerico";
    }
    else
    {
        $model = new Crud;
        $model->insertInto = 'Aspirantes';
        $model->insertColumns='cedula, nombre, apellido, direccion, telefono, mail, equipo_procedencia, posicion_preferida,'
                . 'fecha_nacimiento, nacionalidad, partidos_jugados, cantidad_lesiones';
        $model->insertValues="'$cedula','$nombre','$apellido','$direccion','$telefono','$mail','$equipo_procedencia','$posicion_preferida',"
                . "'$fecha_nacimiento','$nacionalidad','$partidos_jugados','$cantidad_lesiones'";
        $model->Create();
        $model->$model->mensaje;
        
    }
}
?>

<!DOCTYPE HTML>
<html>
<head>
	<title>Sistema de registro "Martita F.C."</title>
    
</head>

<body background="./imagenes/balonderecha.jpg">
<center><h1>FORMULARIO DE REGISTRO</h1></center>
<strong><?php echo $mensaje; ?>
<!--<center><nav>
            <a href="principal.html">Inicio</a>
            <a href="registro.html">Registro</a> 
            <a href="contactos.html">Contactos</a>
            <a href="acerca.html">Acerca de</a>
        </nav></center>-->
    <div style="margin-top: 30px">
        <form id="myForm" method="POST" action="<?php echo $_SERVER['PHP_SELF'];?>">  
            <pre>Cedula      <input type="text" name ="cedula" onchange="validarnumero(this.value)"></pre>
            <pre>Nombre     <input type="text" name ="nombre" onchange="validartexto(this.value)">    Apellido <input type="text" name="apellido" onchange="validartexto(this.value)"></pre>
            <pre>Direccion     <input type="text" name ="direccion" onchange="validartexto(this.value)"></pre>
            <pre>Telefono        <input type="text" name ="telefono" onchange="validarnumero(this.value)"></pre>
            <pre>Mail     <input type="text" name ="mail" onchange="validartexto(this.value)"></pre>
            <pre>Equipo_procedencia     <input type="text" name ="equipo_procedencia" onchange="validartexto(this.value)"></pre>
            <pre>Posicion_preferida     <input type="text" name ="posicion_preferida" onchange="validartexto(this.value)"></pre>
            <pre>fecha_nacimiento     <input type="text" name ="fecha_nacimiento" onchange="validartexto(this.value)"></pre>
            <pre>nacionalidad     <input type="text" name ="nacionalidad" onchange="validartexto(this.value)"></pre>
            <pre>partidos_jugados     <input type="text" name ="partidos_jugados" onchange="validarnumero(this.value)"></pre>
            <pre>cantidad_lesiones     <input type="text" name ="cantidad_lesiones" onchange="validarnumero(this.value)"></pre>

            <input type="hidden" value="create"> 
            <input type="submit" value="Enviar">   
    </form>
    </div>
    
</body>
</html>
<script>
function validartexto(texto)
{   
  var expresion = /^[a-zA-Z]*$/;
    if(texto.search(expresion))
    {
      alert("Ingrese solo letras");  
    }    
 
}
function validarnumero(texto)
{   
  var expresion = /^[a-zA-Z]*$/;
    if(!texto.search(expresion))
    {
      alert("Ingrese solo numeros");  
    }    
 
}
function limpiar()
{
   document.getElementById("myForm").reset();
   alert("Datos registrados");
   window.location="principal.html";
  
}

</script>   




<?php

/* 
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
 require 'rb.php' ;
 class Jugador
{
     private $id;
     private $Nombre;
     private $Apellido;
     private $Telefono;
     
     public function __construct($id,$nombre, $apellido, $telefono) {
         $this->id=$id;
         $this->Nombre=$nombre;
         $this->Apellido=$apellido;
         $this->Telefono=$telefono;
         R::setup('mysql:host=localhost; dbname=futbolclub', 'root','');
    }
     
     
     public function getNombre()
     {
         return $this->Nombre;
     }
     public function setNombre($valor)
     {
         $this->Nombre=$valor;
     }
      public function getApellido()
     {
         return $this->Apellido;
     }
     public function setApellido($valor)
     {
         $this->Apellido=$valor;
     }
      public function getTelefono()
     {
         return $this->Telefono;
     }
     public function setTelefono($valor)
     {
         $this->Telefono=$valor;
     }
     
     public function crear(){
        $jugadores= R::dispense('aspirantes');
              
                $jugadores->Nombre=  $this->getNombre(); 
                $jugadores->Apellido=$this->getApellido();
                $jugadores->Telefono=$this->getTelefono(); 
                $insertado=R::store($jugadores); 
     }
     
     public function eliminar(){
        $jugadores = R::load('aspirantes', $this->id);
                R::trash( $jugadores ); 
     }
     
     public function actualizar(){
          $jugadores = R::load('aspirantes', $id);
            
                $jugadores->Nombre= $this->getNombre();
                $jugadores->Apellido=$this->getApellido();
                $jugadores->Telefono=$this->getTelefono(); 
                R::store($jugadores);
     }
     
     public function buscar(){
          $jugadores = R::load('aspirantes', $id);
     }
}


?>



        <?php
        require 'rb.php' ;
        R::setup('mysql:host=localhost; dbname=futbolclub', 'root',''); 
         
        if (isset($_POST['boton']))    // isset permite ver si la variable existe
        {
            $boton=$_POST["boton"];    //recuperamos el boton 
            $busca=$_POST["busca"]; 
            if($boton=="Buscar")
            {
                //$dat=R::getRow("SELECT * FROM alumnos2 WHERE Id= :busca", array(':busca' => 1) );
                //print_r($dat);
                $jugadores = R::load('aspirantes', $busca);
            }
            if($boton=="Actualizar")
            {
                
                $jugadores = R::load('aspirantes', $busca);
                //$alumnos->Id= $_POST["id"];
                $jugadores->Nombre= $_POST["nombre"];
                $jugadores->Apellido=$_POST["apellido"]; 
                $jugadores->Telefono=$_POST["telefono"]; 
                $jugadores->Sexo=$_POST["sexo"];
                R::store($jugadores);
            }
            if($boton=="Eliminar")
            {
                $jugadores = R::load('aspirantes', $busca);
                R::trash( $jugadores );
                
            }
               if($boton=="Agregar")
            {
                $jugadores= R::dispense('aspirantes');
                //$alumnos->Id=$_POST["id"]; 
                $jugadores->Nombre=$_POST["nombre"]; 
                $jugadores->Apellido=$_POST["apellido"]; 
                $jugadores->Telefono=$_POST["telefono"]; 
                $jugadores->Sexo=$_POST["sexo"];
                $insertado=R::store($jugadores);
               

            }
        }
?>

<!DOCTYPE html>
<!--
To change this license header, choose License Headers in Project Properties.
To change this template file, choose Tools | Templates
and open the template in the editor.
-->

<html>
    <head>
        <meta charset="UTF-8">
        <title></title>
            <form action="capadat.php" method="post" name="form">
             <input type="submit" value="Buscar" id="buscar" name="boton">
             <input type="text"   id="txtbusca" name="busca" value= "<?php if (isset($jugadores->Id))  echo $jugadores->Id;   ?>" >
             <input type="submit" value="Buscar" id="buscar" name="boton"> </br>
             <input type="button" value="Id" id="Nombre" name="id"> 
             <input type="text"  id="txtid" name="id" value= "<?php if (isset($jugadores->Id))  echo $jugadores->Id;   ?>"> </br>
             <input type="button" value="Nombre" id="Nombre" name="nombre"> 
             <input type="text"   id="txtnombre" name="nombre" value= "<?php if (isset($jugadores->Nombre))  echo $jugadores->Nombre;   ?>" > </br>
             <input type="button" value="Apellido" id="Apellido" name="apellido"> 
             <input type="text"   id="txtapellido" name="apellido" value="<?php if (isset($jugadores->Apellido))  echo $jugadores->Apellido;?>"> </br>
             <input type="button" value="Telefono" id="Telefono" name="telefono"> 
             <input type="text"   id="txttelefono" name="telefono" value="<?php if (isset($jugadores->Telefono))  echo $jugadores->Telefono;?>">  </br>
             <input type="button" value="Sexo" id="sexo" name="sexo">
             <select name="sexo" id="sexo" value="<?php if (isset($jugadores->Sexo)) echo $jugadores->Sexo;?>"> 
            <option value="1"> Femenino
            <option value="2"> Masculino
            </select></br>
             <input type="submit" value="Actualizar" id="Actualizar" name="boton">
             <input type="submit" value="Eliminar" id="Eliminar" name="boton">
             <input type="submit" value="Agregar" id="Agregar" name="boton">
         </form>
    </head>
    <body>
  
    </body>
</html>
