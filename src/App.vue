<template>
    <div class="app">
       <ContainerForm />
    </div>
</template>

<script>
import ContainerForm from '@/components/ContainerForm'
export default {
    components: {
        ContainerForm
    },
    data() {
        return {
        }
    },
    methods: {
    }
}
</script>

<style scoped>
    * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
    }

    .app {
        padding: 20px;
    }

</style>
