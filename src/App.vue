<template>
    <Navbar />
        <router-view />
    <Footer />
</template>

<script>
import Navbar from "./components/Layout/Navbar.vue";
import Footer from "./components/Layout/Footer.vue";

export default {
    name: "App",
    components: {
    Navbar,
    Footer,
    },
    methods: {
        formatPrice(value) {
            return value.toString().replace(/(\d)(?=(\d{3})+(?!\d))/g, '$1,');
        },
    }
}; 
</script>

<style>
    @import url("https://fonts.googleapis.com/css2?family=Kanit:wght@400&display=swap");
    * {
        font-family: "Kanit", sans-serif;
    }
    #app {
        background-color: rgb(240, 240, 240);
    }
    .card-product:hover {
        border: 1px solid black;
    }
    .product-card-img {
        max-width: 100%;
        height: 15.5rem;
        margin-bottom: 10px;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .product-card-name {
        overflow: hidden;
        font-size: 25px;
        font-size: 2.5rem;
        max-height: 10rem;
        height: 5rem;
        font-weight: 700;
    }
    .middle {
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .product-img {
        max-width: 70%;
        height: auto;
    }

    .product-img-modal {
        max-height: 100%;
        width: auto;
    }

    .pointer {
        cursor: pointer;
    }

    .h-80vh {
        min-height: 80vh;
    }

    @media screen and (max-width: 1200px) {
        .product-card-img {
            height: auto;
        }
    }

    @media screen and (max-width: 990px) {
        .btn-login {
            margin: 2px 0;
        }
        .width-100 {
            width: 100%;
            margin-bottom: 3px;
        }
    }
</style>