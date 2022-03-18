<template>
    <div class="announcement-form d-flex flex-lg-row flex-column">
        <div class="announcement-form__left">
            <img class="d-block mx-auto logo-lip mb-3" src="@/assets/images/logo-lip-white.svg" alt="Logo LIP">
        </div>
        <div class="announcement-form__right">
            <h1>Création <br /> 
            Annonce Projet</h1>

            <p><img class="separator" src="@/assets/images/separator.png" alt="Séparateur"></p>

            <b-form class="d-flex flex-wrap" @submit="handleSubmit" @reset="onReset" v-if="show">
                <b-form-group
                    id="input-group-1"
                    label="Matière"
                    label-for="input-1"
                    class="w-50 pr-lg-3 mb-4"
                >
                    <b-form-input
                    id="input-1"
                    class="p-4"
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
                    class="w-50 pl-lg-3 mb-4"
                >
                    <b-form-input
                    id="input-2"
                    class="p-4"
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
                    class="w-50 pr-lg-3 mb-4"
                >
                    <b-form-input
                    id="input-3"
                    class="p-4"
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
                    class="w-50 pl-lg-3 mb-4"
                >
                    <b-form-input
                    id="input-4"
                    class="p-4"
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
                    class="w-50 pr-lg-3 mb-4"
                >
                    <b-form-input
                    id="input-5"
                    class="p-4"
                    v-model="modifiedData.data.skills"
                    type="text"
                    placeholder="Maquettage Figma / Adobe XD"
                    required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    id="input-group-6"
                    label="Exemples d'applications"
                    label-for="input-6"
                    class="w-50 pl-lg-3 mb-4"
                >
                    <b-form-input
                    id="input-6"
                    class="p-4"
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
                    class="w-50 pr-lg-3 mb-4"
                >
                    <b-form-input
                    id="input-7"
                    class="p-4"
                    v-model="modifiedData.data.number_of_companies"
                    type="number"
                    placeholder="3"
                    required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    id="input-group-8"
                    label="Date de fin"
                    label-for="input-8"
                    class="w-50 pl-lg-3 mb-4"
                >
                    <b-form-input
                    id="input-8"
                    class="p-4"
                    v-model="modifiedData.data.date_end"
                    type="date"
                    placeholder="3"
                    required
                    ></b-form-input>
                </b-form-group>

                <b-form-group
                    id="input-group-9"
                    label="Décrire votre projet d'intégration d'entreprise"
                    label-for="input-9"
                    class="w-100"
                >
                    <b-form-textarea
                    id="input-9"
                    class="p-4"
                    v-model="modifiedData.data.description"
                    placeholder="Je vais avoir une classe de 20 élèves avec 4 groupes de 5 pour travailler sur votre projet. Le meilleur d'entre eux vous sera proposé."
                    rows="5"
                    max-rows="7"
                    required
                    ></b-form-textarea>
                </b-form-group>

                <p><img class="separator" src="@/assets/images/separator.png" alt="Séparateur"></p>

                <div v-if="isNotSent == true" class="w-100 d-flex justify-content-end">
                    <b-button type="reset" variant="light">Recommencer</b-button>
                    <b-button class="ml-4" type="submit" variant="primary">Confirmer</b-button>
                </div>
                <div class="w-100" v-else>
                    <p class="message-confirmation">Merci ! Votre annonce est envoyée à notre équipe.</p>
                </div>
            </b-form>
        </div>
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
                date_end: "01-01-2000",
                description: ''
            }
        },
        show: true,
        error: null,
        headers: {'Content-Type': 'application/json'},
        isNotSent: true
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
                    console.log(response.data);
                    if(response.data) {
                        this.isNotSent = false;
                    }
            } catch (error) {
                this.error = error
            }
        },
        onReset(event) {
            event.preventDefault()
            // Reset our form values
            this.modifiedData.data.class = '',
            this.modifiedData.data.prepared_exam = '',
            this.modifiedData.data.school_year = 0, 
            this.modifiedData.data.number_of_course_hours = 0,
            this.modifiedData.data.skills = '',
            this.modifiedData.data.possible_application_example = '',
            this.modifiedData.data.number_of_companies = 0,
            this.modifiedData.data.date_end = "01-01-2000",
            this.modifiedData.data.description = ''
            // Trick to reset/clear native browser form validation state
            this.show = false
            this.$nextTick(() => {
                this.show = true
            })
        }
    }
  }
</script>

<style scoped lang="scss">
    .announcement-form {
        &__left {
            background-image: url('@/assets/images/picture-signin.jpg');
            background-size: cover;
            background-position: center;
            width: 35%;
            min-height: 100vh;
            position: relative;
            @media screen and (max-width: 992px) {
                width: 100%;
                min-height: unset;
                height: 275px;
            }
            .logo-lip {
                position: absolute;
                top: 50px;
                left: 0;
                right: 0;
                z-index: 2;
                @media screen and (max-width: 992px) {
                    top: 100px;
                }
            }
            &::after {
                z-index: 1;
                content: "";
                position: absolute;
                top: 0;
                left: 0;
                bottom: 0;
                right: 0;
                background-color: #444BFF;
                mix-blend-mode: multiply;
            }
        }
        &__right {
            padding: 75px 100px;
            width: 65%;
            @media screen and (max-width: 992px) {
                width: 100%;
                padding: 50px 15px;
            }
            .separator {
                width: 100%;
                margin: 20px 0;
            }
            .form-group {
                @media screen and (max-width: 992px) {
                    width: 100%!important;
                }
            }
            .message-confirmation {
                text-align: right;
            }
        }
    }
</style>