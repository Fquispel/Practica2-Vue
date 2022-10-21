<template>
  <div>
    <menuCabecera></menuCabecera>
    <!-- <producto :idProducto="datos"></producto> -->
    <Productos :productoDetalle="{
        id: this.id,
        nombre: this.nombre,
        imagen: this.imagen,
        descripcion: this.descripcion,
        precio: this.precio,
        colores: this.colores
    }">
  </Productos>
    <listaProductos v-on:enlarge-text="onEnlargeText"></listaProductos>
    <menuFooter></menuFooter>
  </div>
</template>

<script>
// IMPORTANDO COMPONENTES
  import menuCabecera from '@/components/menuCabecera.vue';
  import menuFooter from '@/components/menuFooter.vue';
  import Productos from '@/components/Productos.vue';
  import listaProductos from '@/components/productosRelacionados.vue';
  import axios from "axios";
  export default {
    name: 'app',
    data(){
      return {
        //datos: 1
        producto: [],
        id: 0,
        nombre: '',
        imagen: '',
        descripcion: '',
        precio: 0,
        colores: [],
      }
    },
    methods: {
      onEnlargeText: async function (enlargeAmount) {
            try {
                const res = await axios.get(`http://localhost:3333/productos`, {
                    params: {
                        'id': enlargeAmount
                    },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
                });
                this.producto = res.data;
                this.id = this.producto[0].id;
                this.nombre = this.producto[0].nombre;
                this.imagen = this.producto[0].imagen;
                this.descripcion = this.producto[0].descripcion;
                this.precio = this.producto[0].precio;
                this.colores = this.producto[0].colores;
            }
            catch(error){
              console.log(error);
            }
      }
    },

    components: {
      menuCabecera,
      menuFooter,
      Productos,
      listaProductos
    },
  };
</script>

<style>
  .color-box {
        width: 40px;
        height: 40px;
        border-radius: 50%;
        margin: 7px;
        display: inline-block;
    }

    .clic{
        cursor: pointer;
    }

    .quantity button{
        border-radius: 50%;
        display: inline-block;
        width: 30px;
    }
    .quantity div{
        text-align: center;
        min-width: 30px;
        display: inline-block;
        font-weight: bold;
    }
    .buy-box{
        margin: 20px;
    }
    footer {

        text-align: center;
        padding: 30px 10px;
        margin-top: 50px;
        min-height: 100px;
    }
    .container{
        margin-top: 50px;
    }
    .producto-relacionado-precio{
        background: orangered;
        color: white;
        text-align: center;
        padding: 10px;
    }
</style>
