<template>
    <div class="container">
            <div class="row">
                <h3>{{productoDetalle.id==0?this.nombre:productoDetalle.nombre}}</h3>
            </div>
            <div class="row">
                <div class="col-12 col-sm-6 col-md-4 ">
                    <img :src="productoDetalle.id==0?this.imagen:productoDetalle.imagen" alt="" :style="`width:${dimensiones}`">
                </div>
                <div class="col-12 col-sm-6  col-md-8">
                    <h6 v-html="productoDetalle.id==0?this.descripcion:productoDetalle.descripcion"></h6>
                    <div class="p-3 mb-2 text-white" v-bind:style="precioEstilos">
                        <!-- Precio: {{this.precio}} BOB -->
                        Precio: {{productoDetalle.id==0?this.precio:productoDetalle.precio}} BOB

                    </div>
                    <h5>Color</h5>
                    <div>
                        <div class="color-box clic" v-for="color in productoDetalle.id==0?this.colores:productoDetalle.colores" :key="color.id" :style="`background: ${color}; ${color==selector?estilo:''}`" v-on:click="seleccionarcolor(color)"></div>
                    </div>
                    <h5>Cantidad</h5>
                    <div class="quantity">
                        <button v-on:click="restar()">-</button>
                        <div>{{cantidad}}</div> <button v-on:click="incrementar()">+</button>
                    </div>
                    <div class="buy-box">
                        <button type="button" class="btn btn-primary" :disabled="habilitado"
                            v-on:click="comprar()">Comprar</button>
                    </div>
                </div>
            </div>
        </div>
</template>

<script>
    import axios from "axios";

    export default {
        props: {
            idProducto: {
                type: Number,
                default: 1
            },
            productoDetalle: {
                type: []
            }
        },
        name: 'productos',
        data(){
            return {
                precioEstilos: "background: orangered; color: white; font-weight: bold",
                cantidad: 0,
                color: '',
                habilitado: true,
                selector: '',
                estilo: 'border: 3px solid; color: #33F9FF;',
                mensaje: this.idProducto,
                dimensiones: '100%',
                producto: [],

                id: 0,
                nombre: '',
                imagen: '',
                descripcion: '',
                precio: 0,
                colores: [],
                pedido: {
                    id: 0,
                    cantidad: 0,
                    color: ''
                }
            }
        },
        async created(){
            try {
                const res = await axios.get(`http://localhost:3333/productos`, {
                    params: {
                        'id': this.idProducto
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
                this.dimensiones = '100%';
            }catch(error){
                console.log(error);
            }
        },
        methods: {
            incrementar: function () {
                this.cantidad += 1;
                if (this.color != '') {
                    this.habilitado = false;
                }
            },
            restar: function () {
                this.habilitado = true;
                if ((this.cantidad - 1) <= 0) {
                    this.cantidad = 0;
                } else {
                    this.cantidad -= 1;
                    if (this.color != '') {
                        this.habilitado = false;
                    }
                }
            },
            seleccionarcolor: function (color) {
                this.color = color;
                this.selector = color;
                if (this.cantidad > 0) {
                    this.habilitado = false;
                }
                alert('Eligio el color: ' + color);
            },
            comprar: function () {
                this.pedido.id = this.productoDetalle.id==0?this.id:this.productoDetalle.id;
                this.pedido.cantidad = this.cantidad;
                this.pedido.color = this.color;

                alert(JSON.stringify(this.pedido));

                this.cantidad = 0;
                this.selector ='';
                this.habilitado = true;
            },
        },
    }
</script>
<style></style>
