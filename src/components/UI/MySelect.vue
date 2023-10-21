<template>
    <select value="default" @change="changeParametrs" class="box">
        <option disabled value="default">{{ title }}</option>
        <option v-for="elem in list" v-bind:value="elem.id" :key="elem.id">{{ elem.name }}</option>
    </select>
</template>

<script>
export default {
    props: {
        // Список для вывода значений в select. 
        list: {
            type: Array, 
            required: true,
        }, 
        // Дефолтное значение (название select-а). 
        title: {
            type: String, 
            required: true,
        }
    },
    data() {
        return {
            
        }
    },
    methods: {
        // Вызов слушателя родителя при изменении выбранного элемента. 
        changeParametrs(event) {
            this.$emit('change_id', event.target.value);
        }
    }
}
</script>

<style scoped>
    .box {
        width: 100%;
        height: 30px;
        border: 1px solid #999;
        font-size: 18px;
        background-color: white;
        border-radius: 5px;
        margin-top: 15px;
      }
</style>