<template>
<div>
    <div class = "row" v-if = "status === 'LOADING'">
        <div class = "col-12">
            <div class="alert alert-primary alert-dismissible fade show" role="alert">
                <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    <span class="sr-only">Close</span>
                </button>
                <strong>Loading Workshop Details...</strong> 
            </div>
        </div>
    </div>

    <div class = "row">
        <div class = "col-12" v-if = "status === 'ERROR_LOADING'">
            <div class="alert alert-danger alert-dismissible fade show" role="alert">
                <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    <span class="sr-only">Close</span>
                </button>
                <strong>{{this.error.message}}</strong>
            </div>
        </div>
    </div>

    <div class="row" v-if = "status === 'LOADED'">
        <div class="col-12 clearfix">
            <div class="row">
                <h2>
                    {{workshop.name}}
                </h2>
                <hr />
                <div class="row my-4">
                    <div class="col-4">
                        <img class="img-fluid" :src="this.workshop.imageUrl" :alt="this.workshop.name" />
                    </div>
                    <div class="col-8">
                        <div class="row">
                            <div class="col-3">
                                <p>
                                    <small>
                                        {{this.workshop.startDate}} - {{this.workshop.endDate}}
                                    </small>
                                </p>
                                <p>
                                    <small class="text-muted">
                                        {{this.workshop.time}}
                                    </small>
                                </p>
                            </div>
                            <div class="col-3">
                                <p> 
                                    <i :class="this.workshop.modes.online ? 'fa-check' : 'fa-times'" />
                                    <!-- 'fa-check' or 'fa-times' -->
                                    <small>Online</small>
                                </p>
                                <p>
                                    <i :class="this.workshop.modes.inperson ? 'fa-check' : 'fa-times'" />
                                    <!-- 'fa-check' or 'fa-times' -->
                                    <small>In person</small>
                                </p>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-12" v-html = "this.workshop.description"></div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

</template>

<script>
import { fetchWorkshopById } from '@/services/workshops';

const LOADING = 'LOADING', LOADED = 'LOADED', ERROR_LOADING = 'ERROR_LOADING';

export default {
    name: 'WorkshopDetails',
    data () {
        return {
            status: LOADING
        }
    },
    mounted () {
        fetchWorkshopById( this.$route.params.id )      // AJAX call
            .then( workshop => {
                this.status = LOADED;
                this.workshop = workshop;
            })  // .then( addWorkshops ); same thing 
            .catch (error => {
                this.status = ERROR_LOADING;
                this.error = error;
            });
            // console.log (this.error );
    }
}
</script>

<style scoped>
.card {
    cursor: pointer;
}

.card:hover {
    box-shadow: 0px 0px 4px 0px goldenrod;
    background-color: #fcfcfc;
}

.card-img-container {
    min-height: 150px;
    margin: 10px 0;
}
</style>