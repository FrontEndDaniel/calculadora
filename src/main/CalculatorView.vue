<template>
    <div class="calculator">
        <DisplayCalc :value="displayValue" />
        <ButtonCalc label="AC" triple @onButtonClick="clearMemory" />
        <ButtonCalc label="/" operation @onButtonClick="setOperation" />
        <ButtonCalc label="7" @onButtonClick="addDigit" />
        <ButtonCalc label="8" @onButtonClick="addDigit" />
        <ButtonCalc label="9" @onButtonClick="addDigit" />
        <ButtonCalc label="*" operation @onButtonClick="setOperation" />
        <ButtonCalc label="4" @onButtonClick="addDigit" />
        <ButtonCalc label="5" @onButtonClick="addDigit" />
        <ButtonCalc label="6" @onButtonClick="addDigit" />
        <ButtonCalc label="-" operation @onButtonClick="setOperation" />
        <ButtonCalc label="1" @onButtonClick="addDigit" />
        <ButtonCalc label="2" @onButtonClick="addDigit" />
        <ButtonCalc label="3" @onButtonClick="addDigit" />
        <ButtonCalc label="+" operation @onButtonClick="setOperation" />
        <ButtonCalc label="0" double @onButtonClick="addDigit" />
        <ButtonCalc label="." @onButtonClick="addDigit" />
        <ButtonCalc label="=" operation @onButtonClick="setOperation" />

    </div>
</template>
<script>
import DisplayCalc from '../components/DisplayCalc.vue';
import ButtonCalc from '../components/ButtonCalc.vue';
export default {
    components: { ButtonCalc, DisplayCalc },
    data() {
        return {
            displayValue: "0",
            clearDisplay: false,
            operation: null,
            values: [0, 0],
            current: 0,
        }
    },
    methods: {
        clearMemory() {
            Object.assign(this.$data, this.$options.data())
        },
        setOperation(operation) {
            if (this.current === 0) {
                this.operation = operation
                this.current = 1
                this.clearDisplay = true
            } else {
                const equals = operation === "="
                const currentOperation = this.operation
                try {
                    this.values[0] = eval(
                        `${this.values[0]} ${currentOperation} ${this.values[1]}`
                    )
                } catch (e) {
                    this.$$emit('onError', e)
                }
                this.values[1] = 0
                this.displayValue = this.values[0]
                this.operation = equals ? null : operation
                this.current = equals ? 0 : 1
                this.clearDisplay = !equals
            }

        },
        addDigit(n) {
            // se n for igual a "." e this.displayValue ja tiver "." retorne
            if (n === "." && this.displayValue.includes(".")) {
                return
            }
            const clearDisplay = this.displayValue === "0" || this.clearDisplay;
            const currentValue = clearDisplay ? "" : this.displayValue
            const displayValue = currentValue + n;

            this.displayValue = displayValue
            this.clearDisplay = false
            this.values[this.current] = displayValue
        }
    }
}
</script>
<style>
.calculator {
    height: 320px;
    width: 235px;
    border-radius: 5px;
    overflow: hidden;

    display: grid;
    grid-template-columns: repeat(4, 25%);
    grid-template-rows: 1fr 48px 48px 48px 48px 48px;

}
</style>