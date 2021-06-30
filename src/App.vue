<template>
  <div id="app">
    <form @submit.prevent="guardar">
      <input required type="text" placeholder="nombre" v-model="producto.nombre">
      <input required type="text" placeholder="precio" v-model="producto.precio">
      <input required type="text" placeholder="cantidad" v-model="producto.cantidad">
      <button  type="submit">Agregar</button>
    </form>
    <table style="width: 100%" border="1">
      <tr>
        <th>#</th>
        <th>Nombre</th>
        <th>Precio</th>
        <th>Cantidad</th>
        <th>Eliminar</th>
      </tr>
      <tr v-for="(i,index) in productos" :key="index">
        <td>{{index+1}}</td>
        <td>{{i.nombre}}</td>
        <td>{{i.precio}}</td>
        <td>{{i.cantidad}}</td>
        <td>
          <button @click="modificar(i)">Update</button>
          <button @click="eliminar(i)">Eliminar</button>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  data(){
    return {
      productos:[],
      producto:{},
      booleanCrear:true
    }
  },
  components: {
    // HelloWorld
    // productos:[]
  },
  created() {
    this.misdatos();
  },
  methods:{
    guardar(){
      if (this.booleanCrear){
        this.axios.post('http://localhost:8000/api/producto',this.producto)
            .then(res=> {
              console.log(res.data)
              this.misdatos();
              this.producto={};
            });
      }else{
        this.axios.put('http://localhost:8000/api/producto/'+this.producto.id,this.producto)
            .then(res=> {
              console.log(res.data)
              this.misdatos();
              this.producto={};
              this.booleanCrear=true;
            });
      }

    },
    misdatos(){
      this.axios.get('http://localhost:8000/api/producto')
          .then(res=>{
            // console.log(res.data);
            this.productos=res.data;
          });
    },
    eliminar(i){
      // console.log(i)
      this.axios.delete('http://localhost:8000/api/producto/'+i.id)
          .then(res=> {
            console.log(res.data)
            this.misdatos();
          });
    },
    modificar(i){
      // console.log(i);
      this.producto=i;
      this.booleanCrear=false;
    }

  }
}
</script>
