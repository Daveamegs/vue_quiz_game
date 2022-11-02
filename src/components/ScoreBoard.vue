<template>
    <div>
        <h3>Player 1 <h1>{{this.winCount}}</h1></h3>
        <h3>Computer<h1>{{this.loseCount}}</h1></h3>

    </div>
</template>

<script>
    export default {
        props: ["winCount", "loseCount"]

    }
</script>

<style lang="scss">

</style>