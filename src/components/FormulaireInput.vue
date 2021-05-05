<template>
    <div class="formulaire-input">
        <p>{{ $props.text }}</p><input type="text" @change="onUserWhrite" :placeholder="$props.placeholder">
    </div>
</template>
<script>
    export default {
        name: 'FormulaireInput',
        props: {
            text: {
                default: 'default text'
            },
            placeholder: {
                default: 'default placeolder'
            },
            crm: String
        },
        methods: {
            onUserWhrite() {
                this.$emit('userWhrite', {
                    value: this.user_value,
                    crm: this.$props.crm
                });
            }
        },
    }
</script>
<style lang="css">
    .formulaire-input {
        width: 100%;
        margin: 20px 0px;

    }

    .formulaire-input p {
        margin-right: 25px;
        margin-bottom: 5px;
    }

    .formulaire-input input {
        height: 60px;
        width: 95%;
        padding: 0px 10px;
        max-width: 100%;
    }
</style>
