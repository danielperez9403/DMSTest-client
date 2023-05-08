<template>
    <header>
        <nav class="navbar navbar-expand-sm navbar-toggleable-sm bg-dark border-bottom shadow-sm mb-3">
            <div class="container-fluid mx-2">
                <a class="navbar-brand" href="#">
                    <img src="../../assets/dms_logo.png" alt="DominionDMS Logo" height="30 ">
                </a>

                <div class="d-sm-inline-flex d-none justify-content-between align-items-center">
                    <span class="navbar-nav flex-grow-1 text-light">
                        Web Developer Test
                    </span>
                </div>
            </div>
        </nav>
    </header>
</template>

<script lang="js">
    import { defineComponent } from 'vue';

    export default defineComponent({
        name: "HeaderDMS",
    });
</script>