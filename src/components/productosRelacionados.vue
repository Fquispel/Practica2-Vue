<template>
    <div class="container ">

        <div class="row">
            <h4>Productos relacionados</h4>
        </div>
        <div class="row">
            <template v-for="productosRel of productosRelacionados" :key="productosRel.id">
                <div class="col">
                    <div class="card" style="width: 18rem;cursor:pointer;" v-on:click="seleccionado(productosRel.id)">
                        <div class="card-body">
                            <h5 class="card-title">{{productosRel.nombre}}</h5>
                            <img :src="productosRel.imagen" alt="" :style="`width:${dimension}`">
                            <p class="card-text">{{productosRel.descripcion}}.</p>
                            <div class="producto-relacionado-precio" v-bind:style="precioEstilos">Precio: {{idProductoRelacionado}}
                                {{productosRel.precio}} BOB</div>
                            <div>
                                <div>
                                    <div class="color-box" v-for="color in productosRel.colores" :key="color.id" :style="`background: ${color}`"></div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </template>
        </div>
    </div>
</template>

<script>
    import axios from "axios";
    export default {
        props: {
            idProductoRelacionado: {
                type: Number,
                default: 1
            }
        },
        name: 'productosRelacionados',
        data(){
            return {
                productos: [2,3,4],
                productosRelacionados: [],
                dimension: '100%',
                idproductosRelacionados: this.idProductoRelacionado ,
            }
        },
        async created(){
            const res = await axios.get(`http://localhost:3333/productos`, {
                    params: {
                        'id': this.productos
                    },
                    headers:{
                        'content-type': 'application/json',
                        'accept': 'application/json'
                    }
            });
            this.productosRelacionados = res.data;
        },
        methods: {
            seleccionado: async function(id){
                this.productos.push(Number(this.idproductosRelacionados));
                this.productos=this.productos.filter(function(item){
                    return item !== id
                });
                this.idproductosRelacionados = id;
                const res = await axios.get(`http://localhost:3333/productos`, {
                        params: {
                            'id': this.productos
                        },
                        headers:{
                            'content-type': 'application/json',
                            'accept': 'application/json'
                        }
                });
                this.productosRelacionados = res.data;
                this.$emit('enlarge-text', id);

            },
        },
        computed: {
        },
        components: {
        },
    }
</script>

<style></style>
