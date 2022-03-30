<template>
  <q-page padding>
    <div class="row justify-center">
      <div class="col-12 col-md-8">
        <q-card>
          <q-card-section class="bg-primary text-white">
            <div class="text-h6">Calculadora</div>

          </q-card-section>
          <q-card-section >
              <div class="text-h5 text-grey-5 text-right">
                  {{acumulador + actual}}
              </div>
              <div class="text-h3 text-right"> 
                {{resultado}}
              </div>

          </q-card-section>
          <q-card-section class="bg-grey-4">
            <div class="row q-col-gutter-sm">
              <div class="col-3" v-for="(btn, index) in botones" :key="index">
                <q-btn class="full-width text-h6" 
                  :color="noEsNumero(btn) ? 'indigo' : 'grey-2'"
                  :text-color="noEsNumero(btn) ? 'white' : 'grey-8'"
                  @click="btnAccion(btn)"
                  >
                    {{btn}}
                </q-btn>
              </div>
              <div class="col-6">
                  <q-btn class="full-width text-h6" color="indigo" @click="btnReiniciar">
                    Reset
                  </q-btn>
              </div>

              <div class="col-6">
                  <q-btn class="full-width text-h6" color="orange" @click="btnResultado">
                    =
                  </q-btn>
              </div>
            </div>

          </q-card-section>
        </q-card>
        <pre >{{arrayResultado}}</pre>
      </div>
    </div>
  </q-page>
</template>

<script >
import {
  evaluate, round, sqrt
} from 'mathjs'

import {ref} from 'vue'
export default {
  setup() {
    const botones = [7, 8, 9, "%", 4, 5, 6, "+", 1, 2, 3, "-", ".", 0, "/", "*"];
    const noEsNumero = valor => isNaN(valor)
    const actual = ref('')
    const acumulador = ref('')
    const resultado = ref ('')
    const operadorClick = ref (true)
    const arrayResultado = ref ([])
    const btnAccion = valor => {
      if(!noEsNumero(valor)){
        if(operadorClick.value){
            actual.value=""
            operadorClick.value = false
        }

        actual.value =  `${actual.value}${valor}`
      }else{
        ejecutarOperacion(valor)
      }
    }

    const ejecutarOperacion = valor =>{
      if(valor === "."){
        if(actual.value.indexOf('.') === -1){
          actual.value =  `${actual.value}${valor}`
        }
        return
      }
       if(valor === "%"){
        if(actual.value !== ''){
          actual.value =  `${parseFloat(actual.value)/100}`
        }
        return
      }

      agregarOperador(valor)
    }
    const agregarOperador = valor =>{
      if(!operadorClick.value){
         acumulador.value += `${actual.value} ${valor} `
      actual.value =""
      operadorClick.value = true
      }
     
    }

    const btnReiniciar =() =>{
      actual.value =""
      acumulador.value =""
      resultado.value =""
      operadorClick.value = true

    }
    
    const btnResultado = () =>{
      if(!operadorClick.value){
        resultado.value = evaluate(acumulador.value + actual.value)
        arrayResultado.value.push( `${acumulador.value} + ${actual.value} = ${resultado.value}`)
      }
      else{
        resultado.value = 'Error!'
      }
    }
    return{
      botones, noEsNumero, btnAccion, actual, acumulador, btnReiniciar, btnResultado, resultado, arrayResultado
    }
  },
  
}
</script>

<style scoped>
.text-h5{
  height: 23px;
}
.txt-h3{
  height: 50px;
}
</style>
