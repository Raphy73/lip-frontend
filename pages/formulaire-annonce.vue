<template>
  <div>
      <b-container class="my-5">
            <img class="d-block mx-auto logo-lip mb-3" src="@/assets/images/logo-lip.svg">
            <b-form @submit="handleSubmit" @reset="onReset" v-if="show">
                <b-form-group
                    id="input-group-1"
                    label="Matière"
                    label-for="input-1"
                >
                    <b-form-input
                    id="input-1"
                    v-model="modifiedData.data.class"
                    type="text"
                    placeholder="Design"
                    required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    id="input-group-2"
                    label="Diplôme préparé"
                    label-for="input-2"
                >
                    <b-form-input
                    id="input-2"
                    v-model="modifiedData.data.prepared_exam"
                    type="text"
                    placeholder="MBA Expert UX/UI Design"
                    required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    id="input-group-3"
                    label="Année d'étude"
                    label-for="input-3"
                >
                    <b-form-input
                    id="input-3"
                    v-model="modifiedData.data.school_year"
                    type="number"
                    placeholder="4"
                    required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    id="input-group-4"
                    label="Nombre d'heures de cours"
                    label-for="input-4"
                >
                    <b-form-input
                    id="input-4"
                    v-model="modifiedData.data.number_of_course_hours"
                    type="number"
                    placeholder="26"
                    required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    id="input-group-5"
                    label="Compétences"
                    label-for="input-5"
                >
                    <b-form-input
                    id="input-5"
                    v-model="modifiedData.data.skills"
                    type="text"
                    placeholder="Maquettage Figma / Adobe XD"
                    required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    id="input-group-6"
                    label="Exemples d'applications possibles et attendues"
                    label-for="input-6"
                >
                    <b-form-input
                    id="input-6"
                    v-model="modifiedData.data.possible_application_example"
                    type="text"
                    placeholder="Intranet, réseau social..."
                    required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    id="input-group-7"
                    label="Nombre d'entreprises souhaitées"
                    label-for="input-7"
                >
                    <b-form-input
                    id="input-7"
                    v-model="modifiedData.data.number_of_companies"
                    type="number"
                    placeholder="3"
                    required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    id="input-group-8"
                    label="Décrire votre projet d'intégration d'entreprise"
                    label-for="input-8"
                >
                    <b-form-textarea
                    id="input-8"
                    v-model="modifiedData.data.description"
                    placeholder="Je vais avoir une classe de 20 élèves avec 4 groupes de 5 pour travailler sur votre projet. Le meilleur d'entre eux vous sera proposé."
                    rows="3"
                    max-rows="6"
                    required
                    ></b-form-textarea>
                </b-form-group>

                <b-button type="submit" variant="primary">Envoyer</b-button>
                <b-button type="reset" variant="danger">Recommencer</b-button>
            </b-form>
      </b-container>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        modifiedData: {
            data: {
                class: '',
                prepared_exam: '',
                school_year: 0, 
                number_of_course_hours: 0,
                skills: '',
                possible_application_example: '',
                number_of_companies: 0,
                description: ''
            }
        },
        show: true,
        error: null,
        headers: {'Content-Type': 'application/json'}
      }
    },
    methods: {
        parseJSON: function (resp) {
            return (resp.json ? resp.json() : resp);
        },
        checkStatus: function (resp) {
            if (resp.status >= 200 && resp.status < 300) {
                return resp;
            }
            return this.parseJSON(resp).then((resp) => {
                throw resp;
            });
        },
        handleSubmit: async function(e) {
            e.preventDefault();

            try {
                const response = await fetch('https://learn-in-project.herokuapp.com/api/announcements', {
                    method: 'POST',
                    headers: this.headers,
                    body: JSON.stringify(this.modifiedData)
                }).then(this.checkStatus)
                    .then(this.parseJSON);
                    console.log(response);
            } catch (error) {
                this.error = error
            }
        },
        onReset(event) {
            event.preventDefault()
            // Reset our form values
            this.form.email = ''
            // Trick to reset/clear native browser form validation state
            this.show = false
            this.$nextTick(() => {
            this.show = true
            })
        }
    }
  }
</script>

<style scoped>
    .logo-lip {
        width: 175px;
    }  
</style>