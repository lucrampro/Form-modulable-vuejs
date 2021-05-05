<template>
    <div class="formulaire--textarea">
        <p>{{ $props.text }}</p> <textarea v-model="user_value" @change="onUserWhrite"></textarea>
    </div>
</template>

<script>
    export default {
        name: 'FormulaireTextArea',
        data() {
            return {
                user_value: ''
            }
        },
        methods: {
            onUserWhrite() {
                this.$emit('userWhrite', {
                    value: this.user_value,
                    crm: this.$props.crm
                });
            }
        },
        props: {
            text: {
                default: 'default text'
            },
            crm: String
        },

    }
</script>

<style lang="css">
    .formulaire--textarea {
        align-items: center;
        justify-content: space-between;
        margin: 25px 0px;
    }

    .formulaire--textarea p {
        margin-right: 25px;
        margin-bottom: 5px;
    }

    .formulaire--textarea textarea {
        width: 100%;
        height: 80px;
        border: #c0c0c0 solid 1px;
        max-width: 100%;
        min-width: 100%;
    }
</style>