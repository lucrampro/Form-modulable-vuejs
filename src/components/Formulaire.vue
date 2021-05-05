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
        data() {
            return {
                actualy_step: 0,
                crm_information: {
                    field_QN5rroAG: '',
                    field_ukyxr48g: '',
                    field_G4bPJ0RO: '',
                    field_qEzkxK0g: '',
                    field_SVRbIDv4: '',
                    field_djqQB8v4: 'budget',
                    field_yfp8JXUj: '',
                    name: '',
                    company: '',
                    phone: '',
                    email: '',
                    message: '',
                },
                steps: [{
                    name: 'Projet',
                    content: [{
                            type: 'button',
                            value: 'CMS & Web',
                            crm: 'field_QN5rroAG',
                        },
                        {
                            type: 'button',
                            value: 'E-commerce',
                            crm: 'field_QN5rroAG',
                        },
                        {
                            type: 'button',
                            value: 'Marketplace',
                            crm: 'field_QN5rroAG',
                        },
                    ]
                }, {
                    name: 'Details',
                    content: [{
                            type: 'textarea',
                            value: 'Decrivez le ou les projet',
                            crm: 'field_ukyxr48g',
                        },
                        {
                            type: 'textarea',
                            value: 'Décrivez quelles sont les objectifs à atteindre',
                            crm: 'field_G4bPJ0RO',
                        },
                        {
                            type: 'textarea',
                            value: 'Indiquez les principaux éléments qui vous différencient de la concurrence',
                            crm: 'field_qEzkxK0g'
                        },
                        {
                            type: 'textarea',
                            value: 'Décrivez le profil de votre cible',
                            crm: 'field_SVRbIDv4'
                        }
                    ],
                }, {
                    name: 'Budget',
                    content: [{
                            type: 'budget',
                            value: 'Avez vous un budget défini à allouer à la mise en place de votre projet ?',
                            crm: 'field_djqQB8v4'
                        },
                        {
                            type: 'select',
                            text: 'Quel est le délai de mise en production de votre projet ?',
                            value: ['1-3mois', '3-6mois', '6-9mois', '12mois +'],
                            crm: 'field_yfp8JXUj'
                        },
                        {
                            type: 'textarea',
                            value: 'Si vous avez-vous des informations complémentaires, n’hésitez pas à les ajouter ici.',
                            crm: 'message'
                        },
                    ]
                }, {
                    name: 'Info',
                    content: [{
                            type: 'input',
                            value: 'Quel est le nom de votre entreprise ?',
                            crm: 'company'
                        },
                        {
                            type: 'input',
                            value: 'Comment vous appelez-vous ?',
                            crm: 'name'
                        },
                        {
                            type: 'input',
                            value: 'Sur quel email peut-on vous écrire ?',
                            crm: 'email'
                        },
                        {
                            type: 'input',
                            value: 'Sur quel téléphone peut-on vous appeler ?',
                            crm: 'phone'
                        },
                    ]
                }],

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
                    this.apiRequest(
                        'https://nux-crm.mojitoprod.fr/api/campaign_forms/4/leads',
                        'POST', {
                            'Content-Type': 'application/json',
                            Authorization: 'Bearer PASdnFymF5rwyh79bGwnUBf2CTB5qpXa4PhkJdXpBXm9eYBfEycnBoc3BpvaEVmlfOy8JYhon8LeO4kz',
                        },
                        this.crm_information
                    ).then(response => {
                        // this.step += 1;
                        // this.form_is_ok = response;
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