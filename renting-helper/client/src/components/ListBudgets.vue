<template>
  <div class>
  <p class="text-center font-weight-bold">Histórico de Serviços</p>
    <div class="bg-white">
        <label for="services">Orçamentos Pendentes:</label>   
        <div class="d-flex flex-row justify-content-between bg-light p-4 rounded border mb-4" v-for="budget in budgets" :key="budget._id"> 
        <div class="d-flex flex-column">   
            <span><b>Contratante: </b>{{budget.contratante.name}}</span>
            <span><b>Serviço: </b>{{budget.service}}</span>
            <span><b>Estado:</b> {{budget.contratante.state}}</span>
            <span><b>Data: </b>{{new Date(budget.date.replace("Z", "")).toLocaleString().split(" ")[0]}}</span>
        </div>
        <div class="d-flex flex-column">  
            <input v-model="form.value" min="0" type="number" id="budget-value" class="form-control mb-4"/>
            <button @click="updateBudget(budget._id, 1)" type="submit" class="btn btn-success form-group">Enviar Orçamento</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  created(){
    this.$store.dispatch("retrieveBudgets");
  } ,
  props: ["profile"],
  data() {
    return {
      searchData: this.$store.state.search,
      form: {
        type: "contratado",
        services: this.$store.state.services,
        value: 0
      },
      services: this.$store.state.services
    };
  },
  methods: {
    search() {
      this.$store.dispatch("searchUsers");
    },
    requestBudget(id){
      var selectedServices = this.form.services
      this.$store.dispatch("requestBudget", {id, selectedServices});
    },
    updateBudget(id, newStatus){
       const newValue = this.form.value
       this.$store.dispatch("updateBudget", {id, newStatus, newValue});      
    }
  },
  computed: {
    image() {
      return this.$store.getters.currentUser.image;
    },
    budgets(){
      return this.$store.state.budgets.filter(budget => {
        return budget.status == 0
      })
    }     
  },
};
</script>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>
<style scoped>
.flex__container {
  display: flex;
}
.fullwidth {
  width: 100%;
}
.form-control{
  width: 160px;
}
</style>