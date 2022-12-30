<template>
    <div>
        <h5>
            PC <span class="score">{{ this.computer }}</span> | 
            <span class="score">{{ this.player }}</span> YOU
        </h5>
        
    </div>
</template>

<script>
    export default{
        props:['isWin','player','computer']
    }
</script>

<style lang="scss">
    .score{
        padding: 10px;
        background-color: rgb(0, 4, 255);
        color: rgb(255, 255, 255);
    }
</style>