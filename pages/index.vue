<template>
  <div class="py-4">
    <div class="container">
      <div class="title border-bottom">
        <div
          class="title border-bottom d-flex align-items-center justify-content-between py-2"
        >
          <h5>Task</h5>
          <div class="d-flex align-items-center ms-auto">
            <select id="category" name="category" v-model="searchCategory">
              <option value="Pakaian">Pakaian</option>
              <option value="Aksesoris">Aksesoris</option>
              <option value="Perhiasan">Perhiasan</option>
            </select>
          </div>
          <div class="d-flex align-items-center ms-auto">
            <button
              class="btn btn-outline-secondary py-1 px-3"
              @click="hide = !hide"
            >
              {{ hide ? "Show" : "Hide" }}
            </button>
          </div>

          <div class="d-flex align-items-center ms-auto">
            <!-- /* Form input pencarian */ -->
            <input
              type="text"
              class="form-control"
              placeholder="Search"
              v-model="searchQuery"
            />
            <div class="d-flex align-items-center justify-content-end w-100">
              <span class="me-2">View As</span>
              <button
                class="btn btn-outline-secondary py-1 px-3"
                @click="isGrid = !isGrid"
              >
                {{ isGrid ? "Grid" : "List" }}
              </button>
            </div>
          </div>
        </div>
      </div>

      <div class="list-task row">
        <CardItem
          v-for="(task, i) in resultQuery"
          :key="i"
          :task="task"
          :isGrid="isGrid"
          :hide="hide"
        />
      </div>

      <div class="action py-2">
        <!--* Jika isCreating == false maka tombol Add Task tidak akan tampil */ /*
        isCreating = !isCreating berfungsi sebagai switcher toggle */-->
        <a
          href="#"
          class="add-button"
          v-if="!isCreating"
          @click="isCreating = !isCreating"
          >Add Task</a
        >
        <div class="add-card" v-else>
          <div class="card mb-2">
            <div class="card-body d-flex flex-column p-0">
              <input
                class="form-control border-0 mb-2"
                placeholder="Title"
                type="text"
              />
              <textarea
                class="form-control border-0 small"
                placeholder="Description"
                rows="3"
              ></textarea>
            </div>
          </div>
          <div class="button-wrapper d-flex">
            <button class="btn btn-primary me-2">Save</button>
            <button
              class="btn btn-outline-secondary"
              @click="isCreating = !isCreating"
            >
              Cancel
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import CardItem from "@/components/Card/CardItem.vue";

export default {
  components: {
    CardItem,
  },

  data() {
    return {
      tasks: [
        {
          title: "Baju",
          description: "ini Kategori Pakaian",
          isDone: false,
          category: "Pakaian",
        },
        {
          title: "Celana",
          description: "ini Kategori Pakaian",
          isDone: false,
          category: "Pakaian",
        },
        {
          title: "Jam Tangan",
          description: " ini Kategori Aksesoris",
          isDone: false,
          category: "Aksesoris",
        },
        {
          title: "Gelang",
          description: "ini Kategori Aksesoris",
          isDone: false,
          category: "Aksesoris",
        },
        {
          title: "Cincin",
          description: "ini Kategori Perhiasan",
          isDone: false,
          category: "Perhiasan",
        },
        {
          title: "kalung",
          description: " ini Kategori Perhiasan",
          isDone: false,
          category: "Perhiasan",
        },
      ],

      isCreating: false,
      isGrid: false,
      hide: false,
      // Variabel penampung teks pencarian
      searchQuery: "",

      searchCategory: "",
      // Status saat menambahkan task
      isCreating: false,
      // Tipe layout daftar task
    };
  },
  computed: {
    resultQuery() {
      if (this.searchCategory) {
        return this.tasks.filter((item) => {
          return this.searchCategory
            .toLowerCase()
            .split(" ")
            .every((v) => item.category.toLowerCase().includes(v));
        });
      } else {
        console.log(this.tasks);
        return this.tasks;
      }
    },
  },
};
</script>

<style>
.div h5 {
  font-family: sans;
  color: red;
}
</style>
