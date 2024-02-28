<template>
  <div>
    <header>
      <h1>TALONARIO</h1>
    </header>

    <!-- div datos iniciales __________________-->
    <div id="datosIniciales" v-show="mostrarInformacionInicial">
      <h3 id="alerta" v-if="error != ''">{{ error }}</h3>
      <h2>CONFIGURA TU TALONARIO</h2>
      <input type="text" v-model="premio" placeholder="premio"> <br>
      <input type="text" v-model="valorBoleta" placeholder="valor Boleta"><br>
      <select class="loteria" id="loteria" v-model="loteria">
        <option value="" disabled selected>Seleccione la loteria </option>
        <option value="cundinamarca">Cundinamarca</option>
        <option value="cruzRoja">Cruz Roja </option>
        <option value="manizales">Manizales </option>
        <option value="Bogota">Bogota</option>
        <option value="santander">Santander </option>
        <option value="boyaca">Boyaca </option>
        <option value="culona">Culona</option>
      </select> <br>
      <select class="cantidadBoletas" v-model="cantidadBoletas">
        <option value="" disabled selected>Cantidad de Boletas</option>
        <option value="100">0-100</option>
        <option value="1000">0-1000 </option>
        <option value="10000">0-10000</option>
      </select> <br>
      <input type="date" v-model="fechaSorteo" placeholder="Fecha sorteo">
      <br><input type="button" @click="validarInformacionInicial()" value="Guardar">
    </div>
    <div class=" principal">
      <!-- informacion -->
      <div class="informacion">

        <h2>INFORMACIÓN</h2>
        <div class="informacion2">
          <P>🏆{{ premio }}</P>
          <P>💰{{ valorBoleta }}</P>
          <P>🎰{{ loteria }}</P>
          <P>🗓️{{ fechaSorteo }}</P>
          <input type="button" value="Editar✒️" @click="editarInformacionInicial()">
        </div>
      </div>



      <div class="tarjetas">
        <div v-for="objetoRifa in arrayRifa" :key="objetoRifa.id" class="card" @click="mostrarEdicion(objetoRifa)">

          <div v-if="!objetoRifa.editando">

            <span :class="'estado-' + objetoRifa.estado.toLowerCase()" @click="mostrarEdicion(objetoRifa)">{{
              objetoRifa.id
            }}</span>
          </div>

        </div>
      </div>
      <!-- mostrar informacion tarjeta -->
      <div class="informacionComprador" v-if="tarjetaSeleccionada">
        <div class="informacionCompradorContendor">
          <label class="tituloInformacionComprador">
            <p>Información Comprador</p>
          </label>

          <p>Numero de Boleta: {{ tarjetaSeleccionada.id }}</p><br>
          <label for="nombreEdit">Nombre:</label>
          <input type="text" id="nombreEdit" v-model="tarjetaSeleccionada.nombre"><br>
          <label for="direccionEdit">Dirección:</label>
          <input type="text" id="direccionEdit" v-model="tarjetaSeleccionada.direccion"><br>
          <label for="telefonoEdit">Teléfono:</label>
          <input type="text" id="telefonoEdit" v-model="tarjetaSeleccionada.telefono"><br>
          <label>Pagada:</label>
          <div>
            <input type="radio" id="pagadaSi" value="Si" v-model="tarjetaSeleccionada.pagada">
            <label for="pagadaSi">Sí</label>
            <input type="radio" id="pagadaNo" value="No" v-model="tarjetaSeleccionada.pagada">
            <label for="pagadaNo">No</label>
          </div>
          <button class="botonGuardarInformacion" @click="guardarEdicion(tarjetaSeleccionada)">Guardar</button>



        </div>
      </div>

      <!-- acciones -->

      <div class="acciones">

        <h2>ACCIONES</h2>
        <div class="acciones2">
          <input type="button" value="ESTADO"><br>
          <input type="button" value="LISTAR BOLETAS"><br>
          <input type="button" value="PERSONALIZAR TALONARIO"><br>
          <input type="button" value="GENERAR ARCHIVO DE DATOS">


        </div>
      </div>
    </div>



    <footer>
      <p>&copy; 2023. Todos los derechos reservados.</p>

    </footer>


  </div>
</template>

<script setup>
import { ref } from "vue"
let premio = ref("")
let valorBoleta = ref("")
let loteria = ref("")
let cantidadBoletas = ref("")
let fechaSorteo = ref("")
let error = ref("")
let mostrarInformacionInicial = ref("false")
let arrayInformacionInical = ref([])

let tarjetaSeleccionada = ref(null);

let arrayRifa = ref([]);

function guardarInformacionInicial() {
  const objetoInformacionInical = {
    premio: premio.value,
    valorBoleta: valorBoleta.value,
    loteria: loteria.value,
    fechaSorteo: fechaSorteo.value
  }
  arrayInformacionInical.value.push(objetoInformacionInical)
  mostrarInformacionInicial.value = false;
  console.log(arrayInformacionInical)
}

function validarInformacionInicial() {
  const fechaActual = new Date();
  const fechaSeleccionada = new Date(fechaSorteo.value);
  if (premio.value == "") {
    error.value = "Digite el valor del premio"
    setTimeout(() => {
      error.value = ""
    }, 3000)
  }
  else if (valorBoleta.value == "") {
    error.value = "Digite el valor de la boleta"
    setTimeout(() => {
      error.value = ""
    }, 3000)
  }
  else if (loteria.value == "") {
    error.value = "seleccione una loteria"
    setTimeout(() => {
      error.value = ""
    }, 3000)
  }
  else if (fechaSorteo.value == "") {
    error.value = "Debes seleccionar una fecha"
    setTimeout(() => {
      error.value = ""
    }, 3000)
  }
  else if (fechaActual >= fechaSeleccionada) {
    error.value = 'fecha incorrecta'
    setTimeout(() => {
      error.value = ""
    }, 3000);
  }
  else {
    guardarInformacionInicial()
  }

}
function editarInformacionInicial() {

}

let arrayEstadoRifa = ["disponible", "pagada", "porPagar", "ganador"]

// array 
for (let i = 0; i < 100; i++) {
  const objetoRifa = {
    id: i < 10 ? `0${i}` : `${i}`,
    estado: arrayEstadoRifa[0],
    nombre: "",
    direccion: "",
    telefono: "",
    pagada: "",
    mostrarInfo: false

  };
  arrayRifa.value.push(objetoRifa);



}
console.log(arrayRifa)


// const mostrarEdicion = (objetoRifa) => {
//   objetoRifa.editando = true;

// };

const mostrarEdicion = (objetoRifa) => {
  tarjetaSeleccionada.value = objetoRifa;
};
const guardarEdicion = (objetoRifa) => {
  objetoRifa.editando = false;
  tarjetaSeleccionada.value = null;

  const index = arrayRifa.value.findIndex(item => item.id === objetoRifa.id);
  if (index !== -1) {
    arrayRifa.value[index] = { ...objetoRifa };

    objetoRifa.estado = arrayEstadoRifa.find(estado => estado.toLowerCase() === objetoRifa.pagada.toLowerCase()) || arrayEstadoRifa[0];

    arrayRifa.value = [...arrayRifa.value];
  }
};


</script>

<style>
* {
  padding: 0;
  margin: 0;
  font-family: Georgia, 'Times New Roman', Times, serif;

}

body {
  position: relative;
  min-height: 100vh;
  padding-bottom: 5%;
}

header {
  width: 100%;
  height: auto;
  background-color: #0467fc;
  text-align: center;
  color: #ffffff;
  /* position: fixed; */


}

header h1 {
  font-size: 300%;
  padding: 1%;
}

/* configuracion tu talonario*/


#alerta {
  width: 30%;
}

h3 {
  width: 100%;
  height: 50px;
  padding: 2%;
  text-align: center;
  border-radius: 10px;
  background-color: red;
  color: #ffffff;
  position: fixed;
}

#datosIniciales {
  background-color: #dee2e2e3;
  width: 40%;
  height: auto;
  margin-left: 28%;
  position: fixed;
  padding: 1%;
  margin-top: 1%;
  border-radius: 10px;
  text-align: center;
  /* display: none; */

}

#datosIniciales h2 {
  background-color: #0467fc;
  color: #ffffff;
  padding: 2%;
  border-radius: 5px;

}



input[type="text"] {
  width: 80%;
  height: 40px;
  border-radius: 6px;
  border: none;
  margin: 2%;
  padding-left: 2%;
  font-size: 100%;
}

input[type="date"] {
  width: 80%;
  height: 40px;
  border-radius: 6px;
  border: none;
  margin: 2%;
  padding-left: 2%;
  font-size: 100%;
}

#datosIniciales input[type="button"] {
  background-color: #0467fc;
  font-size: 150%;
  padding: 2%;
  border: none;
  border-radius: 5px;
  color: #ffffff;
}

#datosIniciales input:hover {
  background-color: #8eafe0;
  color: #000000;
}



::placeholder {
  color: #313131;

}


select {
  width: 80%;
  height: 40px;
  border-radius: 6px;
  border: none;
  margin: 2%;
  padding-left: 2%;
  font-size: 100%;
}
/* principal */
.principal{
  width: 100%;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

/* informacion  */
.informacion {
  background-color: #dee2e2;
  width: 90%;
  margin: 1%;
  padding: 2%;
  border-radius: 10px;
  text-align: center;

}

.informacion2 {
  background-color: #ffffff;
  padding: 3%;
  border-radius: 10px;
  margin-top: 3%;

}

.informacion2 P {
  margin: 1%;
  font-size: 200%;
  text-align: left;

}

.informacion2 input[type="button"] {
  background-color: #0467fc;
  font-size: 120%;
  padding: 3%;
  border: none;
  border-radius: 5px;
  color: #ffffff;
}

.informacion2 input:hover {
  background-color: #8eafe0;
  color: #000000;
}


/* Acciones */
.acciones {
  background-color: #dee2e2;
  width: 90%;
  margin: 1%;
  padding: 2%;
  border-radius: 10px;
  text-align: center;

}

.acciones2 {
  background-color: #ffffff;
  padding: 3%;
  border-radius: 10px;
  margin-top: 3%;

}

.acciones2 input {
  width: 80%;
  /* font-size: 100%; */
  padding: 3%;
  margin: 2%;
  border-radius: 5px;
  border: none;
  background-color: #0467fc;
  color: #ffffff;
}

/* Talonario  */
.card {
  margin: 1%;
  width: 80px;
  height: 80px;
  border-radius: 50%;
  font-size: 150%;
  text-align: center;
  display: inline-block;
  vertical-align: top;
  cursor: pointer;
  background-color: #0467fc;
  color: #ffffff;
  overflow: hidden;
}

.card span {
  line-height: 80px;
}

.card:hover {
  background-color: #8eafe0;
  color: #000000;
}

.informacionComprador {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #dee2e2;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 0 4px 8px #00000049;
  z-index: 1000;


}

.informacionCompradorContendor {
  width: 80%;
  max-width: 600px;
  margin: auto;
}

.tituloInformacionComprador p {
  background-color: #0467fc;
  color: #ffffff;
  width: 100%;
  padding: 3%;
  border-radius: 5px;
  text-align: center;
  font-size: 110%;

}

.botonGuardarInformacion {
  background-color: #0467fc;
  color: #ffffff;
  width: 80%;
  padding: 3%;
  border-radius: 5px;
  text-align: center;
  font-size: 110%;
  border: none;
}

.botonGuardarInformacion:hover {
  background-color: #8eafe0;
  color: #000000;
}

.estadoPagada {
  background-color: green;
}

.estadoPorPagar {
  background-color: red;
}

.tarjetas {
  display: grid;
  grid-template-columns: repeat(10, 1fr);
}

/* footer */

footer {
  width: 100%;
  height: auto;
  background-color: #0467fc;
  text-align: center;
  color: #ffffff;
  position: absolute;
  bottom: 0;
}

footer p {
  padding: 1%;
}
</style> 