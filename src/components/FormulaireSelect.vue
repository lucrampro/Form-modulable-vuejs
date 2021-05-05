<template>
    <div class="formulaire-select">
        <p>{{ $props.text }}</p>
        <select v-model="user_value" @change="userChange" name="" id="">
            <option v-for="(value, i) in $props.select_value" :key="i" :value="value"> {{value}} </option>
        </select>
    </div>
</template>
<script>
    export default {
        name: 'FormulaireSelect',
        data() {
            return {
                user_value: ''
            }
        },
        props: {
            text: {
                default: 'default text'
            },
            select_value: {
                default: ['default option', 'default option', 'default option', 'default option']
            },
            crm: String
        },
        methods: {
            userChange() {
                this.$emit('userWhrite', {
                    value: this.user_value,
                    crm: this.$props.crm
                });
            }
        }
    }
</script>
<style lang="css">

</style>