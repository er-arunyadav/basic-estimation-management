<template>
    <div class="addEstimation">
        <div class="jumbotron text-center">
            <h1>Edit Estimation</h1>
        </div>
        <div class="container">
            <div class="row">
                <div class="alert" role="alert" style="background:green;color:#fff" v-if="!!state.message">
                    {{ state.message }}
                </div>
                <form v-on:submit.prevent="submitForm">
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="sprint_name">Sprint Name</label>
                            <input type="number" name="sprint_name" class="form-control" placeholder="Enter Sprint Name"
                                v-model="form.sprint">
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="task_name">Task Name *</label>
                            <input type="text" name="task_name" class="form-control" placeholder="Enter Task Name"
                                v-model="form.task_name">
                        </div>
                    </div>

                    <div class="col-md-12">
                        <div class="form-group">
                            <label for="description">Description</label>
                            <textarea name="description" id="description" cols="30" rows="10" class="form-control"
                                placeholder="Enter Description" v-model="form.description"></textarea>
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="hours">Hours</label>
                            <input type="number" name="hours" class="form-control" placeholder="Enter Hours"
                                v-model="form.hours">
                        </div>
                    </div>
                    <div class="col-md-6">
                        <div class="form-group">
                            <label for="developer_name">Developer Name</label>
                            <input type="text" name="developer_name" class="form-control"
                                placeholder="Enter Developer Name" v-model="form.developer_name">
                        </div>
                    </div>
                    <div class="col-md-2">
                        <button class="btn btn-primary">Submit</button>
                        <RouterLink to="/" class="btn btn-primary">Back</RouterLink>
                    </div>
                </form>
            </div>
        </div>
    </div>
</template>

<script setup>
import axios from 'axios';
import { onMounted, reactive } from 'vue';
import { useRoute } from 'vue-router';

const initialState = {
    'sprint': '',
    'task_name': '',
    'description': '',
    'hours': '',
    'developer_name': '',
    'id': ''
}
const form = reactive({ ...initialState });

let estimation_id = ''
const getEstimation = onMounted(() => {
    const route = useRoute();
    estimation_id = route.params.id;
    getList(estimation_id)
});

const getList = (id) => {
    axios.get(`http://estimation.test/get/estimation/${id}`)
        .then((response) => {
            if (response.data.status == 200) {
                Object.assign(form, response.data.data)
            }
        })
}

const state = reactive({
    message: ''
});
const submitForm = () => {
    axios.post(`http://estimation.test/estimation/update/${estimation_id}`, form)
        .then((response) => {
            if (response.data.status == 200) {
                state.message = response.data.message
            }
        })
        .catch((error) => {
            console.log(error);
        }).finally(() => {
            //Perform action in always
        });
}
</script>