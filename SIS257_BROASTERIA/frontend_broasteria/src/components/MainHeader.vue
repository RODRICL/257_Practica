<script setup lang="ts">
    import { useAuthStore } from '@/stores';
    import { useRoute } from 'vue-router';

    const authStore = useAuthStore()
    const location = useRoute()

</script>
<template>

<nav class="navbar navbar-expand-lg" :style="'background-color: ' + (location.path != '/' ? 'black' : '')">
        <div class="container">
            <div class="row">
                <!-- Brand and toggle get grouped for better mobile display -->
                <div class="navbar-header">
                    <button type="button" class="navbar-toggle collapsed" data-toggle="collapse"
                        data-target="#bs-example-navbar-collapse-1">
                        <span class="sr-only">Toggle navigation</span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                    </button>
                    <a class="navbar-brand" href="#">Restaurant</a>
                </div>

                <!-- Collect the nav links, forms, and other content for toggling -->
                <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
                    <ul class="nav navbar-nav main-nav  clear navbar-right ">
                        <li>
                            <RouterLink to="/" class="nav-link click-scroll">Inicio</RouterLink>
                        </li>
                        <slot v-if="authStore.token">
                            <li><a class="navactive color_animation" href="#top">HOME</a></li>
                            <li>
                                <RouterLink to="/clientes" class="color_animation" href="#story">CLIENTES</RouterLink>
                            </li>
                            <li>
                                <RouterLink to="/productos" class="color_animation" href="#story">PRODUCTOS</RouterLink>
                            </li>
                            <li>
                                <RouterLink to="/productoventa" class="color_animation" href="#story">PRODUCTOS DISPONIBLES</RouterLink>
                            </li>
                            <li>
                                <RouterLink to="/carrito" class="color_animation" href="#story">CARRITO</RouterLink>
                            </li>
                            
                       
                        </slot>
                    </ul>
                    <a  @click="authStore.logout()" class="btn custom-btn d-lg-block d-none">Salir</a>
                </div><!-- /.navbar-collapse -->
            </div>
        </div><!-- /.container-fluid -->
    </nav>

    <div id="top" class="starter_container bg">
        <div class="follow_container">
            <div class="col-md-6 col-md-offset-3">





            </div>
        </div>
    </div>

</template>


<style scoped></style>