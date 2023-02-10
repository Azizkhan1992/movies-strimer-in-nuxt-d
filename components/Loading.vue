<template>
    <div class="loading">
        <span></span>
    </div>
</template>
<script>

</script>
<style lang="scss">
.loading{
    height: 100vh;
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;

    @keyframes circleLoader {
        to {
            transform: rotateZ(360deg);
        }
    }

    span{
        display: block;
        width: 70px;
        height: 70px;
        border-radius: 50%;
        border: 2px solid transparent;
        border-top-color: #fff;
        animation: circleLoader 1s infinite;
    }
}
</style>