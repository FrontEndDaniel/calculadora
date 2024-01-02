<template>
    <div id="app">
        <h1>Calculadora</h1>
        <CalculatorView />
    </div>
</template>
<script>
import CalculatorView from "./main/CalculatorView.vue"
export default {
    components: { CalculatorView }

}
</script>
<style>
body {
    margin: 0px;
}

#app {
    display: flex;
    flex-direction: column;
    height: 100vh;
    width: 100vw;
    justify-content: center;
    align-items: center;
    text-align: center;
    color: #fff;
    background: linear-gradient(to left, rgb(59, 79, 90), rgb(29, 37, 83))
}
</style>