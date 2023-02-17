<template>
  <div id="app">
    
    <label for="pesos">Valor en pesos:</label>
    
    <input type="number" id="pesos" v-model="pesos"> 
    
    <br>
    <label for="Dolar">Cotización del dolar:</label>
    <input type="number" id="cotizacion" v-model="cotizacion" :disabled="autoUpdate"> 
    <button @click="updateDolar" v-if="!autoUpdate">Actualizar</button>
    <br>

    <label for="resultado">Resultado:</label>
    <span id="resultado">{{dolarConvertido}}</span>
    <br>

    <label for="autoUpdate">Actualizar automáticamente: </label>
    <input type="checkbox" id="autoUpdate" v-model="autoUpdate">
    <br>

    <span v-if="autoUpdate">Última actualización: {{lastUpdate}}</span> 

    <h2><i>RESPUESTAS</i></h2>
        <div class="table responsive">
          <table class="table table-dark">
            <tr>
              <th>Pregunta</th>
              <th>Respuesta</th>
            </tr>
            <tr>
             <th>1</th>
             <th>C</th>
            </tr>
            <tr>
             <th>2</th>
             <th>B</th>
            </tr>
            <tr>
             <th>3</th>
             <th>C</th>
            </tr>
            <tr>
             <th>4</th>
             <th>A</th>
            </tr>
            <tr>
             <th>5</th>
             <th>B</th>
            </tr>
            </table>
   
  </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',
  components: {
  },
  data() {
    return {
      url: 'https://www.dolarsi.com/api/api.php?type=valoresprincipales',
      pesos:0,
      cotizacion:1,
      autoUpdate: false,
      lastUpdate:'',
    };
  },
  mounted(){
    setInterval(()=>{
      if(this.autoUpdate){
        this.updateDolar();
      }
    },2000);

  },
  computed: {
    dolarConvertido() {
      return (this.pesos / this.cotizacion).toFixed(2);
    }
  },
  methods: {
    async updateDolar() {
      try {
        let { data } = await axios.get(this.url);
        this.cotizacion = parseFloat(data[0].casa.venta.replace(',', '.'));
        this.lastUpdate = new Date().toLocaleString();
      } catch(error) {
        console.error('Error Axios', error);
      }   
    }
  }
}
</script>


<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
