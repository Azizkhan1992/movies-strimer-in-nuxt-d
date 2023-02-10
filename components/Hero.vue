<template>
    <div class="hero">
        <img src="../assets/imgs/profile.jpg" alt="">   
        <div class="text-container">
            <div class="text">
                <span class="miniT">Now Streaming</span>
                <h1><span>Now</span> Streaming</h1>
                <a href="#movie-grid" class="button">View Movies</a>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: 'test-hero',
    
}
</script>
<style lang="scss">
.hero{
    height: 600px;
    position: relative;
     @media screen and (min-width: 750px) {
        height: 800px;
     }

     &::after{
        content: "";
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.6);
     }

     img{
        width: 100%;
        height: 100%;
        object-fit: cover;
     }

     .text-container{
        z-index: 99;
        position: absolute;
        top: 0;
        left: 10%;
        margin: auto;
        width: 80%;
        height: 100%;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;

        .text{
            padding: 0 16px;
            width: 100%;
            max-width: 1400px;
            margin: 0 auto;

            h1{
                font-size: 5rem;
                font-weight: 200;
                color: #fff;
                margin-bottom: 8px;

                span{
                    font-weight: 600;
                }
            }
        }
     }
}
</style>