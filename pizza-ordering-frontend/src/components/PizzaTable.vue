<template>
  <table>
    <thead>
      <tr>
        <th v-if="checkable"></th>
        <th>#</th>
        <th>Title</th>
        <th>Category</th>
        <th>Description</th>
        <th>Created</th>
        <th>Updated</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="pizza in pizzas" :key="pizza.id">
        <td data-label="id">{{ pizza.id }}</td>
        <td data-label="Name">{{ pizza.title }}</td>
        <td data-label="Company">{{ pizza.pizza_category }}</td>
        <!-- <td data-label="Company">{{ pizza.ingredients }}</td> -->
        <td data-label="Company">{{ pizza.description }}</td>
        <td data-label="Created">
          <small class="text-gray-500" :title="pizza.created_at">
            {{ pizza.created_at }}</small
          >
        </td>
        <td data-label="Created">
          <small class="text-gray-500" :title="pizza.updated_at">
            {{ pizza.updated_at }}</small
          >
        </td>
        <td class="actions-cell">
          <jb-buttons type="justify-start" no-wrap>
             <jb-button
              color="warning"
              :icon="mdiEye"
              small
              @click="viewPizza(pizza.id)"
            />
            <jb-button
              class="mr-3"
              color="success"
              :icon="mdiBookEdit"
              small
              @click="editPizza(pizza.id)"
            />
            <jb-button
              color="danger"
              :icon="mdiTrashCan"
              small
              @click="deletePizza(pizza.id)"
            />

          </jb-buttons>
        </td>
      </tr>
    </tbody>
  </table>
  <!-- <div class="table-pagination">
    <level>
      <jb-buttons>
        <jb-button
          v-for="page in pagesList"
          @click="currentPage = page"
          :active="page === currentPage"
          :label="page + 1"
          :key="page"
          small
        />
      </jb-buttons>
      <small>Page {{ currentPageHuman }} of {{ numPages }}</small>
    </level>
  </div> -->
</template>

<script>
// eslint-disable-next-line no-unused-vars
import { mdiBookEdit, mdiTrashCan, mdiEye } from '@mdi/js'
import JbButtons from '@/components/JbButtons'
import JbButton from '@/components/JbButton'

export default {
  name: 'PizzadTable',
  components: {
    JbButtons,
    JbButton
  },
  props: {
    checkable: Boolean
  },
  data () {
    return {
      pizzas: []
    }
  },
  setup () {
    return {
      mdiBookEdit,
      mdiTrashCan,
      mdiEye
    }
  },
  created () {
    this.axios
      .get(
        'http://localhost/pizza-ordering-app-Vue3-Laravel-8-/pizza-ordering-backend/public/api/Pizza'
      )
      .then((response) => {
        console.log(response.data.pizzas_list)
        this.pizzas = response.data.pizzas_list
      })
      .catch((error) => console.log(error))
      .finally(() => (this.loading = false))
  },
  methods: {
    deletePizza (id) {
      this.axios
        .delete(
          `http://localhost/pizza-ordering-app-Vue3-Laravel-8-/pizza-ordering-backend/public/api/Pizza/${id}`
        )
        .then((response) => {
          // eslint-disable-next-line eqeqeq
          if (response.data.status == 409) {
            alert(response.data.message)
          } else {
            alert(response.data.message)
            this.axios
              .get(
                'http://localhost/pizza-ordering-app-Vue3-Laravel-8-/pizza-ordering-backend/public/api/Pizza'
              )
              .then((response) => {
                this.pizzas = response.data.pizzas_list
              })
              .catch((error) => console.log(error))
              .finally(() => (this.loading = false))
          }
          //
        })
        .catch((error) => {
          console.log(error)
        })
        .finally(() => (this.loading = false))
    },
    editPizza (id) {
      this.$router.push({
        path: `/edit-pizza/${id}`
      })
    },
    viewPizza (id) {
      this.$router.push({
        path: `/pizza-detail/${id}`
      })
    }
  }
}
</script>
