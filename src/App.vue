<template>
  <div id="app">
    <div class="container--formlaire">
      <Formulaire :crm_key="crm_key" :data_api="data_api" />
    </div>
  </div>
</template>

<script>
  import Formulaire from './components/Formulaire.vue'

  export default {
    name: 'App',
    beforeMount() {
      fetch('https://jsonplaceholder.typicode.com/todos/1')
        .then(response => response.json())
        .then(json => console.log(json))
    },
    data() {
      return {
        crm_key: {
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
        data_api: [{
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
        }]

      }
    },
    components: {
      Formulaire
    }
  }
</script>

<style>
  #app {
    height: 100vh;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .container--formlaire {
    width: 500px;
  }
</style>