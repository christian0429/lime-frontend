<template>
    <div>
        <div class="card card-body my-5 py-5 text-center" v-if="fiches.length == 0">
            <h3>il y'a aucune fiche</h3>
        </div>
        <b-card v-for="fiche in fiches" :key="fiche.id">
            <b-card-body :class="$screen.width < 768 ? 'px-0' : ''">
                <b-container
                    :class="$screen.width < 768 ? 'px-0' : ''"
                    class="bv-example-row text-center"
                >
                    <b-row class="mb-2">
                        <b-row>
                            <b-col>employe : {{ employeById(fiche.employe_id) }}</b-col>
                        </b-row>
                        <b-row>
                            <b-col>date : {{ fiche.date }}</b-col>
                        </b-row>
                        <b-row>
                            <b-col>total : {{ fiche.total }}</b-col>
                        </b-row>

                        <oneRebrique
                            @fetchFiches="fetchFiches('/api/fiche')"
                            @attachRebrique="attachRebrique"
                            :rebrique="rebrique"
                            :alert="alert"
                            v-for="rebrique in fiche.rebriques"
                            :key="rebrique.id"
                        />
                    </b-row>

                    <div :class="$screen.width < 768 ? 'row' : ''">
                        <b-button
                            class="my-1"
                            variant="danger"
                            v-on:click="Delete(fiche.id)"
                        >Supprimer</b-button>
                        <b-button
                            class="my-1"
                            variant="warning"
                            v-on:click="Update(fiche)"
                            data-bs-toggle="modal"
                            data-bs-target="#ficheModal"
                        >Modifier</b-button>
                        <b-button
                            class="my-1"
                            variant="warning"
                            v-on:click="select(fiche)"
                            data-bs-toggle="modal"
                            data-bs-target="#rebriqueModal"
                        >Ajouter rubrique</b-button>

                        <b-button class="my-1" variant="success" v-on:click="getPdf(fiche)">Afficher</b-button>
                    </div>
                </b-container>
            </b-card-body>
        </b-card>
        <nav class="row">
            <ul class="pagination w-auto mx-auto">
                <li :class="[{ disabled: !pagination.prev_page_url }]" class="page-item">
                    <a
                        :class="[!pagination.prev_page_url ? 'disabled' : 'link-primary btun']"
                        @click="fetchFiches(pagination.prev_page_url)"
                        class="btn page-link"
                    >Precedent</a>
                </li>
                <li class="page-item">
                    <a
                        class="page-link text-dark"
                        href="#"
                    >{{ pagination.current_page + "/" + pagination.last_page }}</a>
                </li>
                <li :class="[{ disabled: !pagination.next_page_url }]" class="page-item">
                    <a
                        @click="fetchFiches(pagination.next_page_url)"
                        :class="[!pagination.next_page_url ? 'disabled' : 'link-primary btun']"
                        class="btn page-link"
                    >Suivant</a>
                </li>
            </ul>
        </nav>
    </div>
</template>

<script>
import oneRebrique from './oneRebrique.vue';
export default {
    components: {
        oneRebrique
    },
    props: {
        fiches: Array,
        pagination: Object,
        alert: Object,
        employes: Array
    },
    emits: ['deleteFiche', 'updateFiche', 'fetchFiches', 'attachRebrique', 'selectFiche']
    ,
    methods: {
        Delete(id) {
            this.$emit('deleteFiche', id);
        },
        Update(fiche) {
            this.$emit('updateFiche', fiche);
        },
        select(fiche) {
            this.$emit('selectFiche', fiche);
        },
        fetchFiches(url) {
            this.$emit('fetchFiches', url)
        },
        getPdf(fiche) {
            // console.log(fiche.employes.id);

            window.open(window.location.protocol + "//" + window.location.host + "/generatePdf/" + fiche.employes.id + "/fiche/" + fiche.id);
        },
        attachRebrique(RebriqueId) {
            this.$emit('attachRebrique', RebriqueId);
        },
        employeById(id) {
            let emp = '';
            let find = 0;
            for (let i = 0; i < this.employes.length && !find; i++) {
                if (this.employes[i].id == id) {
                    emp = this.employes[i].nom + ' ' + this.employes[i].prenom;
                }
            }
            return emp;
        }
    }
}
</script>

<style>
</style>
