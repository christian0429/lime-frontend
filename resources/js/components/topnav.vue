<template>
    <nav class="navbar shadow fixed navbar-expand-lg navbar-light bg-light">
        <div class="container-fluid">
            <router-link class="navbar-brand mt-2 mt-lg-0" to="/">
                <img src="images/large.png" width="50" height="50" alt="MDB Logo" loading="lazy" />
            </router-link>
            <search v-if="show == 'true'" :search="search" @fetch="fetch" />
            <div class="d-flex align-items-center">
                <b-dropdown
                    id="dropdown-right"
                    right
                    text="Right align"
                    variant="primary"
                    class="m-2"
                >
                    <template #button-content>
                        <i class="fas fa-user"></i>
                    </template>
                    <b-dropdown-item v-if="name">{{ name }}</b-dropdown-item>
                    <b-dropdown-item href="logout">Logout</b-dropdown-item>
                </b-dropdown>
            </div>
        </div>
    </nav>
</template>

<script>
import search from './search.vue';
export default {
    props: {
        show: {
            type: String,
            default: false,
        },
        name: {
            type: String,
            default: ''
        },
        search : {
            type: String,
            default: ''
        }
    },
    components: {
        search
    },
    data() {
        return {
        };
    },
    methods: {
        fetch(search) {
            this.$emit('fetch', search);
        }
    }
}
</script>

<style>
</style>
