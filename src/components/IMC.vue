<template>
  <div class="page-content">
    <div class="title">
      Calcule o IMC, Gabriele <br>
      <span class="span">Para Gabriele saber se está magrinha ou gordinha.</span>
    </div>

    <b-field label="Altura" message="(ex.: 1,75)">
      <b-input value="" v-model="altura"></b-input>
    </b-field>

    <b-field label="Peso" message="(ex.: 65,5)">
      <b-input value="" v-model="peso"></b-input>
    </b-field>

    <div class="buttons">
      <b-button @click="calcularIMC" type="is-primary" expanded>Calcular</b-button>
    </div>



    <div v-show="calcular">
      <p>
        <b-skeleton height="150px"></b-skeleton>
      </p>
    </div>


    <div v-show="mostrarResultado" class="notification">
        <div class="content">
            <h4>
                Resultado
            </h4>
            <p>
                Pontuação do seu IMC: <b>{{ imc }}</b>  <br>
                Sua classificação é: <b>{{ classificacao }}</b>
            </p>
            <section>
              <b-button
                  label="Ver a tabela do IMC"
                  @click="mostrarInformcoes = !mostrarInformcoes" 
                  type="is-primary"/>

              <br>
              <br>
              <b-message 
                  title="IMC (Índice de Massa Corporal)" 
                  v-model="mostrarInformcoes"
                  type="is-primary"
                  aria-close-label="Close message"
              >

                <b-table :data="data" :columns="columns"></b-table>
              </b-message>
          </section>
        </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IMC',
  data() {
    return {
      altura: '',
      peso: '',
      imc: '-',
      mostrarResultado: false,
      calcular: false,
      mostrarInformcoes: false,
      columns: [
        {
          field: 'class',
          label: 'Classificação',
        },
        { 
          field: 'pont',
          label: 'Pontuação',
        }
      ],
      data: [
        { class: 'Abaixo do Peso', pont: 'Menor que 18,5' },
        { class: 'Peso Normal', pont: 'Entre 18,5 e 24,9' },
        { class: 'Sobrepeso', pont: 'Entre 25 e 29,9' },
        { class: 'Obesidade Grau I', pont: 'Entre 30 e 34,9' },
        { class: 'Obesidade Grau II', pont: 'Entre 35 e 39,9' },
        { class: 'Obesidade Grau III', pont: 'Maior que 40' }
      ]
    }
  },
  methods: {
    calcularIMC() {

      if (this.altura == '' || this.peso == '') {
        this.mostrarResultado = false;
        this.calcular = false;
        this.toast('Preencha todos os campos!');
        return;
      }
      let peso = this.formatNumber(this.peso);
      let altura = this.formatNumber(this.altura);

      if(isNaN(altura) || isNaN(peso)) {
        this.mostrarResultado = false;
        this.calcular = false;
        this.toast('Altura e Peso devem ser números!');
        return;
      }

      this.calcular = true;
      this.mostrarResultado = false;
      this.mostrarInformcoes = false;
      
      let imc = peso / (altura * altura);
      this.imc = imc.toFixed(2);

      setTimeout(() => {
        this.mostrarResultado = true;
        this.calcular = false;
      }, 1500);
    },
    formatNumber(number){
      return number.replace(',', '.');
    },
    toast(message) {
      this.$buefy.toast.open({
          duration: 2000,
          message,
          type: 'is-danger'
      })
    },
  },
  computed: {
    classificacao() {

      if(this.imc == '-') {
        return '-';
      }

      if (this.imc < 18.5) {
        return 'Abaixo do Peso';
      } else if (this.imc >= 18.5 && this.imc <= 24.9) {
        return 'Peso Normal';
      } else if (this.imc >= 25 && this.imc <= 29.9) {
        return 'Sobrepeso';
      } else if (this.imc >= 30 && this.imc <= 34.9) {
        return 'Obesidade Grau I';
      } else if (this.imc >= 35 && this.imc <= 39.9) {
        return 'Obesidade Grau II';
      } else {
        return 'Obesidade Grau III';
      }
    }
  }
}
</script>

<style scoped>
.page-content {
  margin: 3% auto;
  margin-top: 7%;
  max-width: 450px;
  width: 90%;
}

.title{
  font-size: 1.5rem;
  font-weight: bold;
}
.span {
  font-size: 0.9rem;
  font-weight: 400;
}
</style>
