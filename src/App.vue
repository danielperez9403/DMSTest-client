<template>
    <HeaderDMS />

    <FooterDMS />
</template>

<script>
    import HeaderDMS from './components/headers/HeaderDMS.vue'
    import FooterDMS from './components/footers/FooterDMS.vue'

    export default {
        name: 'App',
        components: {
            HeaderDMS,
            FooterDMS
        }
    }
</script>

<style>
    html {
        font-size: 14px;
    }

    @media (min-width: 768px) {
        html {
            font-size: 16px;
        }
    }

    html {
        position: relative !important;
        min-height: 100% !important;
    }

    body {
        margin-bottom: 6rem !important;
    }
</style>
