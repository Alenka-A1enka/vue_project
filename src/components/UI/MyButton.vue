<template>
    <button class="btn">
        <slot></slot>
    </button>

</template>

<script>

</script>

<style scoped>
    .btn {
        border: 1px solid #999;
        font-size: 18px;
        background-color: white;
        border-radius: 1px;
        margin-top: 30px;
        padding: 10px 15px;
        cursor: pointer;
        position: relative;
        left: 50%;
        transform: translate(-50%, 0);
    }
    .btn:hover {
        box-shadow: 8px 10px 5px 2px #C0C0C0;

    }
</style>