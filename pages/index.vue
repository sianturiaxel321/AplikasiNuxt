<template>
  <div class="py-4">
    <div v-if="isLoading">
      <div class="lds-facebook">
        <div></div>
        <div></div>
        <div></div>
      </div>
    </div>
    <div class="container" v-else>
      <div class="title border-bottom">
        <div
          class="title border-bottom d-flex align-items-center justify-content-between py-2"
        >
          <h5>Task</h5>
          <div class="d-flex align-items-center ms-auto">
            <select id="category" name="category" v-model="searchCategory">
              <option value="">Filter By Kategori</option>
              <option value="Pakaian">Pakaian</option>
              <option value="Aksesoris">Aksesoris</option>
              <option value="Perhiasan">Perhiasan</option>
            </select>
          </div>

          <div class="d-flex align-items-center ms-auto">
            <select id="category" name="category" v-model="sortingBy">
              <option value="">Ascending || Descending</option>
              <option value="asc">Ascending</option>
              <option value="desc">Descending</option>
            </select>
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
          :isHide="isHide"
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
            <form v-on:submit.prevent="formAddTask">
              <div class="card-body d-flex flex-column p-0">
                <input
                  v-model="formAddTask.title"
                  class="form-control border-0 mb-2"
                  placeholder="Title"
                  type="text"
                />
                <input
                  v-model="formAddTask.category"
                  class="form-control border-0 mb-2"
                  placeholder="Kategori"
                  type="text"
                />
                <textarea
                  v-model="formAddTask.description"
                  class="form-control border-0 small"
                  placeholder="Description"
                  rows="3"
                ></textarea>
              </div>
            </form>
          </div>
          <div class="button-wrapper d-flex">
            <button class="btn btn-primary me-2" @click="submitForm">
              Saves
            </button>
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
      formAddTask: {
        title: "",
        description: "",
        isDone: false,
        category: "",
        isHide: false,
      },

      tasks: [
        {
          title: "Baju",
          description: "ini Kategori Pakaian",
          isDone: false,
          category: "Pakaian",
          isHide: false,
        },
        {
          title: "Celana",
          description: "ini Kategori Pakaian",
          isDone: false,
          category: "Pakaian",
          isHide: false,
        },
        {
          title: "Jam Tangan",
          description: " ini Kategori Aksesoris",
          isDone: false,
          category: "Aksesoris",
          isHide: false,
        },
        {
          title: "Gelang",
          description: "ini Kategori Aksesoris",
          isDone: false,
          category: "Aksesoris",
          isHide: false,
        },
        {
          title: "Cincin",
          description: "ini Kategori Perhiasan",
          isDone: false,
          category: "Perhiasan",
          isHidden: false,
        },
        {
          title: "kalung",
          description: " ini Kategori Perhiasan",
          isDone: false,
          category: "Perhiasan",
          isHidden: false,
        },
      ],
      isLoading: true,
      isHide: false,
      isCreating: false,
      isGrid: false,
      // Variabel penampung teks pencarian
      searchQuery: "",

      searchCategory: "",
      sortingBy: "",
      // Status saat menambahkan task
      isCreating: false,
      // Tipe layout daftar task
    };
  },
  beforeCreate() {
    console.log("before created");
  },
  created() {
    console.log(" created");
  },
  beforeMount() {
    console.log("beforeMount");
  },

  mounted() {
    setTimeout(() => {
      this.isLoading = false;
    }, 3000);
  },
  methods: {
    submitForm() {
      var myObj = {
        title: this.formAddTask.title,
        description: this.formAddTask.description,
        isDone: false,
        category: this.formAddTask.category,
        isHide: false,
      };
      // var myObj = {
      //   title: "Test Title 2",
      //   description: "Test Description 2",
      //   isDone: false,
      //   category: "Test Category 2",
      //   isHide: false,
      // };
      this.tasks.push(myObj);
      // console.log(this.tasks);
      return (
        (this.isCreating = false),
        (this.formAddTask.title = ""),
        (this.formAddTask.description = ""),
        (this.formAddTask.category = "")
      );
    },
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
      } else if (this.searchQuery) {
        return this.tasks.filter((item) => {
          return this.searchQuery
            .toLowerCase()
            .split(" ")
            .every((v) => item.title.toLowerCase().includes(v));
        });
      } else if (this.sortingBy) {
        // console.log("test");
        if (this.sortingBy == "asc") {
          return (this.tasks = _.orderBy(this.tasks, ["title"], ["asc"]));
          // console.log("asc");
        } else {
          return (this.tasks = _.orderBy(this.tasks, ["title"], ["desc"]));
          // console.log("desc");
        }
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

.lds-facebook {
  display: inline-block;
  position: relative;
  width: 180px;
  height: 180px;
  top: 50%;
  left: 50%;
}
.lds-facebook div {
  display: inline-block;
  position: absolute;
  left: 8px;
  width: 16px;
  background: rgb(10, 9, 9);
  animation: lds-facebook 1.2s cubic-bezier(0, 0.5, 0.5, 1) infinite;
}
.lds-facebook div:nth-child(1) {
  left: 8px;
  animation-delay: -0.24s;
}
.lds-facebook div:nth-child(2) {
  left: 32px;
  animation-delay: -0.12s;
}
.lds-facebook div:nth-child(3) {
  left: 56px;
  animation-delay: 0;
}
@keyframes lds-facebook {
  0% {
    top: 8px;
    height: 64px;
  }
  50%,
  100% {
    top: 24px;
    height: 32px;
  }
}
</style>
