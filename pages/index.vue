<template>
  <div class="py-4">
    <div class="container">
      <div class="title border-bottom d-flex align-items-center justify-content-between py-2">
        <h5>Task</h5>
        <div class="d-flex align-items-center ms-auto">
          <div class="dropdown-center">
            <button class="btn btn-secondary dropdown-toggle" type="button" data-bs-toggle="dropdown">
              Category
            </button>
            <ul class="dropdown-menu">
              <li><a class="dropdown-item" @click="selectedCategory = 0">Semua</a></li>
              <li><a class="dropdown-item" href="#" v-for="(category, i) in categories" :key="i" @click="selectedCategory = category.id">{{ category.title }}</a></li>
            </ul>
          </div>
          <input type="text" class="form-control" placeholder="Search" v-model="searchQuery">
          <span class="me-2">view as</span>
          <button class="btn btn-outline-secondary py-1 px-3" @click="isGrid = !isGrid">{{ isGrid ? 'Grid' : 'List' }}</button>
        </div>
      </div>

      <div class="list-task row">
        <CardItem v-for="(task, i) in resultQuery" :key="i" :task="task" :isGrid="isGrid" />
      </div>
      <div class="action py-2">
        <a href="#" class="add-button" v-if="!isCreating" @click="isCreating = !isCreating">Add Task</a>
        <div class="add-card" v-else>
          <form @submit.prevent="handleSubmit">
            <div class="card mb-2">
              <div class="card-body d-flex flex-column p-0">
                  <input v-model="form.title" type="text" class="form-control border-0 mb-2" placeholder="Title">
                  <div class="d-flex flex-row p-1">
                    <textarea v-model="form.description" class="form-control border-0 small" placeholder="Descriptions" rows="3" style="width: 70%"></textarea>
                    <div class="d-flex flex-column">
                      <label><strong>Category</strong></label>
                      <label v-for="category in categories" :key="category.id">
                        <input type="radio" v-model="form.categoryId" :value="category.id">{{ category.title }}
                      </label>
                    </div>
                  </div>
              </div>
            </div>
            <div class="button-wrapper d-flex">
              <button class="btn btn-primary me-2">Save</button>
              <button class="btn btn-outline-secondary" @click="isCreating = !isCreating">Cancel</button>
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import CardItem from '~/components/card/CardItem.vue'
  export default {
    components : {
      CardItem
    },
    data() {
      return {
        form : {
          title : '',
          description: '',
          isDone: false,
          categoryId: 0,
        },
        selectedCategory: 0,
        categories: [
          {id: 1, title: 'Ringan'},
          {id: 2, title: 'Sedang'},
          {id: 3, title: 'Berat'}
        ],
        tasks: [],
        searchQuery: '',
        isCreating: false,
        isGrid: false,
      }
    },
    computed: {
      resultQuery() {
        if (this.searchQuery) {
          return this.tasks.filter((item) => {
            return this.searchQuery.toLowerCase().split(" ").every((v) => item.title.toLowerCase().includes(v));
          });
        } else if (this.selectedCategory != 0){
          return this.tasks.filter((item) => this.selectedCategory === item.categoryId);
        } else {
          return this.tasks;
        }
      }
    },
    methods : {
      handleSubmit() {
        const item = {...this.form}
        this.tasks.push(item);
        this.form.title = "";
        this.form.description ="";
        this.form.categoryId = null;
      }
    }
  }

</script>