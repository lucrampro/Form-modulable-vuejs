<template>
    <div :style="background" class="formulaire-button" @click="onClickFormulaireButton">
        <p>{{ $props.name }}</p>
        <div class="formulaire-button-circle"></div>
    </div>
</template>
<script>
    export default {
        name: 'FormulaireButton',
        data() {
            return {
                isSelectButton: false,
            }
        },
        methods: {
            onClickFormulaireButton() {
                this.isSelectButton = !this.isSelectButton;
            }
        },
        computed: {
            background() {
                if (this.isSelectButton === false) {
                    return {
                        background: '#b3b3b3'
                    }
                } else {
                    return {
                        background: '#030303'
                    }
                }
            }
        },
        props: {
            name: {
                default: 'Name of button'
            },
        },
    }
</script>
<style lang="css">
    .formulaire-button {
        min-height: 70px;
        padding: 0px 20px;
        margin: 5px 0px;
        /* background-color: #b3b3b3; */
        max-width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        color: #fff;
        transition: 0.3s;
    }

    .formulaire-button .formulaire-button-circle {
        height: 30px;
        width: 30px;
        border: solid 1px white;
        border-radius: 50%;
    }
</style>