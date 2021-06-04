<template>
  <div id="app">
    <div style="display: flex; flex-direction: row">
      <div style="flex: 50%;height: 300px;">
        <h2>Rango de horas</h2>
        <div>
          <label for="inicial">Hora inicial</label><br>
          <input id="inicial" v-model="horaInicial" style="margin-right: 10px" type="time"><input v-model="fechaInicial" type="date">
        </div>
        <div>
          <label for="final">Hora final</label><br>
          <input  v-model="horaFinal" id="final" style="margin-right: 10px" type="time"><input  v-model="fechaFinal" type="date">
        </div>
      </div>
      <div style="flex: 50%;height: 300px;">
        <h2>Participantes</h2>
        <ul>
            <li v-for="(participante,index) in participantes" v-bind:key="participante">{{participante}} <button @click="borrarParticipante(index)">Eliminar</button></li>
        </ul>
        <br>
        <label for="">Añadir participante</label><br>
        <input v-model="nuevoParticipante" type="text"><button @click="agregarParticipante()">Añadir</button>
      </div>
    </div>
    <div style="text-align: center">
      <button @click="generar()">Generar</button>
    </div>
    <div v-if="results.length">
      <table>
        <tr>
          <th>
            Hora
          </th>
          <th>
            Participante
          </th>
        </tr>
        <tr v-for="result in results" v-bind:key="result">
          <td>
            {{result.fecha}}
          </td>
          <td>
            {{result.participante}}
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data:  ()=> {
    return {
      participantes: [],
      fechaInicial: formatDate(new Date()) ,
      horaInicial: null,
      fechaFinal: formatDate(new Date()),
      horaFinal: null,
      nuevoParticipante: "",
      results: []
    }
  },
  methods: {
    agregarParticipante: function() {
      if(this.nuevoParticipante){
        this.participantes.push(this.nuevoParticipante)
        this.nuevoParticipante = ""
      }
    },
    borrarParticipante: function(index){
      this.participantes.splice(index,1)
    },
    generar: function() {
      this.results = []
      let participantesDesordenados = shuffle([...this.participantes])
      let fechaInicial = new Date(this.fechaInicial + " " + this.horaInicial)
      let fechaFinal = new Date(this.fechaFinal + " " + this.horaFinal)
      do{
        this.results.push({
          fecha: fechaInicial.getHours() + ':' + fechaInicial.getMinutes()+ "    " + fechaInicial.getDate() + '/' + fechaInicial.getMonth() + '/' + fechaInicial.getFullYear()
        })
        fechaInicial = fechaInicial.addHours(1)
      }while(fechaInicial < fechaFinal)
      let i = 0
      for(let result of this.results){
        result.participante = participantesDesordenados[i]
        i++
        if(i >= this.participantes.length) i = 0
      }
    }
  }
}

Date.prototype.addHours = function(h) {
  this.setTime(this.getTime() + (h*60*60*1000));
  return this;
}

function formatDate(date) {
    var d = new Date(date),
        month = '' + (d.getMonth() + 1),
        day = '' + d.getDate(),
        year = d.getFullYear();

    if (month.length < 2) 
        month = '0' + month;
    if (day.length < 2) 
        day = '0' + day;

    return [year, month, day].join('-');
}

function shuffle (array) {
    var currentIndex = array.length,  randomIndex;

    // While there remain elements to shuffle...
    while (0 !== currentIndex) {

      // Pick a remaining element...
      randomIndex = Math.floor(Math.random() * currentIndex);
      currentIndex--;

      // And swap it with the current element.
      [array[currentIndex], array[randomIndex]] = [
        array[randomIndex], array[currentIndex]];
    }

    return array;
  }
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
