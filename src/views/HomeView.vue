<template>
  <div>

    <div class="jumbotron text-center">
      <h1>Estimation</h1>
    </div>

    <div class="container">
      <div class="d-flex justify-content-end">
        <router-link to="/add/estimation" class="btn btn-success">Add Estimation</router-link>
      </div>
      <div class="row ">

        <table class="table table-bordered">
          <thead>
            <tr>
              <th>Sr. No.</th>
              <th>Sprint</th>
              <th>Task name</th>
              <th>Descriptions</th>
              <th>Developer</th>
              <th>Hours</th>
              <th>Action</th>
            </tr>
          </thead>


          <tbody v-if="estimations.data.length > 0">
            <tr v-for="(estimation, index) in estimations.data" :key="estimation.id">
              <td>{{ ++index }}</td>
              <td>{{ estimation.sprint }}</td>
              <td>{{ estimation.task_name }}</td>
              <td>{{ estimation.description }}</td>
              <td>{{ estimation.developer_name }}</td>
              <td>{{ estimation.hours }}</td>
              <td>
                <router-link :to="'/edit/estimation/' + estimation.id" class="btn btn-info">Edit</router-link>
                <button class="btn btn-danger" @click="deleteEstimation(estimation.id)">Delete</button>
              </td>
            </tr>
          </tbody>

          <tbody v-else>
            <tr rowspan="5">
              <p class="text-center"><b>No Data Found!</b></p>
            </tr>
          </tbody>

        </table>
      </div>
    </div>

  </div>
</template>
<script setup>

import axios from 'axios'
import { onMounted, reactive } from 'vue';

const estimations = reactive({
  data: []
});


const getList = () => {
  axios.get('http://estimation.test/get/estimations')
    .then((response) => {
      if (response.data.status == 200) {
        estimations.data = response.data.data
      }
    })
}
const getEstimations = onMounted(() => {
  getList();
});
const deleteEstimation = (id) => {

  if (confirm('Are you sure?')) {
    axios.post(`http://estimation.test/estimation/delete/${id}`)
      .then((response) => {
        if (response.data.status == 200) {
          getList()
          state.message = response.data.message

        }
      })
      .catch((error) => {
        console.log(error);
      }).finally(() => {
        //Perform action in always
      });
  }

}

</script>