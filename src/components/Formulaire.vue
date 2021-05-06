<template>
    <div class="formulaire">
        <div class="header--formulaire">
            <p v-for="(step, i) in steps" :key="i"> {{step.name}} </p>
        </div>
        <div class="content--formulaire">
            <div class="step" v-for="(step, i) in steps" :key="i" v-show="actualy_step == i">
                <p>{{ i + 1 }} / {{ steps.length }}</p>
                <div v-for="(content, i2) in step.content" :key="i2">
                    <FormulaireInput @userWhrite="onUserWhrite" v-show="content.type == 'input'" :crm="content.crm"
                        :text="content.value" />
                    <FormulaireButton v-show="content.type == 'button'" :name="content.value"
                        @click.native="clickOnFormInput(content.crm, content.value)" :text="content.value" />
                    <FormulaireTextArea v-show="content.type === 'textarea'" :crm="content.crm" :text="content.value"
                        @userWhrite="onUserWhrite" />
                    <FormulaireSelect v-show="content.type === 'select'" :text="content.text"
                        :select_value="content.value" :crm="content.crm" @userWhrite="onUserWhrite" />
                    <FormulaireBudget v-show="content.type === 'budget'" :text="content.value" />
                </div>
            </div>
        </div>
        <div class="footer--formulaire">
            <p @click="previousStep">revenir</p>
            <p @click="nextStep">continuer</p>
        </div>
    </div>
</template>
<script>
    import FormulaireButton from './FormulaireButton';
    import FormulaireInput from './FormulaireInput.vue';
    import FormulaireTextArea from './FormulaireTextArea';
    import FormulaireSelect from './FormulaireSelect';
    import FormulaireBudget from './FormulaireBudget';

    export default {
        name: 'formulaire',
        beforeMount() {
            this.crm_information = this.$props.crm_key;
            this.steps = this.$props.data_api;
        },
        data() {
            return {
                actualy_step: 0,
                crm_information: {},
                steps: [],
            }
        },
        components: {
            FormulaireButton,
            FormulaireTextArea,
            FormulaireInput,
            FormulaireSelect,
            FormulaireBudget
        },
        methods: {
            nextStep() {
                let step_1 = true;

                for (const step of this.steps[this.actualy_step].content) {
                    if (this.crm_information[step.crm] === '') {
                        step_1 = false;
                        console.log(step.crm);
                    }
                }

                if (step_1 && this.actualy_step < this.steps.length -1) {
                    console.log(this.steps.length);
                    console.log(this.actualy_step);
                    this.actualy_step += 1
                } else if (step_1 && this.actualy_step == this.steps.length -1) {
                    // this.apiRequest(
                    //     'https://nux-crm.mojitoprod.fr/api/campaign_forms/4/leads',
                    //     'POST', {
                    //         'Content-Type': 'application/json',
                    //         Authorization: 'Bearer PASdnFymF5rwyh79bGwnUBf2CTB5qpXa4PhkJdXpBXm9eYBfEycnBoc3BpvaEVmlfOy8JYhon8LeO4kz',
                    //     },
                    //     this.crm_information
                    // ).then(response => {
                    //     // this.step += 1;
                    //     // this.form_is_ok = response;
                    //     console.log(response);
                    // })
                    console.log(JSON.stringify(this.crm_information));
                    fetch('https://nux-crm.mojitoprod.fr/api/campaign_forms/4/leads', {
                        method: 'post',
                        headers: {
                            'Authorization': 'Bearer PASdnFymF5rwyh79bGwnUBf2CTB5qpXa4PhkJdXpBXm9eYBfEycnBoc3BpvaEVmlfOy8JYhon8LeO4kz',
                            // 'Access-Control-Allow-Origin': 'http://localhost:8080/'
                        },
                        body: JSON.stringify(this.crm_information),
                    })
                    .then( promise => {
                        return promise.json()
                    })
                    .then(response => {
                        console.log(response);
                    })
                } else if (step_1 == false) {
                    alert('Remplissez toutes vos information')
                }
            },
            previousStep() {
                this.actualy_step > 0 ? this.actualy_step -= 1 : ''
            },
            clickOnFormInput(crm, value) {

                let regex = new RegExp(value)


                if (regex.test(this.crm_information[`${crm}`])) {
                    let new_info = this.crm_information[`${crm}`];

                    new_info = new_info.replace(` ${value}`, '');

                    this.crm_information[`${crm}`] = new_info;

                } else {
                    this.crm_information[`${crm}`] += ` ${value}`
                }
            },
            onUserWhrite(info) {
                this.crm_information[info.crm] = info.value;
            },
            async apiRequest(link, method, header, body) {
                let result = await fetch(link, {
                    method,
                    headers: header,
                    body: JSON.stringify(body),
                }).then((response) => {
                    return !!response.ok;
                });

                return result
            },
        },
        props: {
            crm_key: Object,
            data_api: Array
        }
    }
</script>
<style lang="css">
    .formulaire {
        padding: 20px;
        width: 100%;
        min-height: 400px;
        box-shadow: 0px 0px 10px rgb(0 0 0 / 15%);
        display: flex;
        flex-direction: column;
        justify-content: space-between;
    }

    .formulaire .header--formulaire {
        min-height: 70px;
        border: solid 1px red;
        display: flex;
        align-items: center;
        justify-content: space-around;
    }

    .formulaire .footer--formulaire {
        display: flex;
        align-items: center;
        justify-content: flex-end;
    }

    .formulaire .footer--formulaire p {
        margin: 0px 20px;
    }
</style>