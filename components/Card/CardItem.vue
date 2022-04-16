<template>
  <div
    :class="[
      'item-task d-flex align-items-start border-bottom pt-3 pb-4',
      isGrid ? 'col-12 col-md-6 col-lg-4' : 'col-12',
    ]"
  >
    <button
      class="btn btn-sm btn-secondary py-1 px-3"
      @click="isHide = !isHide"
    >
      {{ isHide ? "SHOW" : "HIDE" }}
    </button>
    <div v-show="!isHide">
      <div class="title-task mb-1">
        {{ task.title }}
        <span>{{ task.category }}</span>
      </div>
      <div class="description-task small text-muted">
        {{ task.description }}
      </div>
      <input class="form-control form-control-sm" type="date" />
    </div>
    <span v-show="isHide">Task Selesai</span>
  </div>
</template>

<script>
export default {
  props: {
    task: {
      type: Object,
      default: "Untitled",
    },
    isHide: {
      type: Boolean,
      required: true,
      default: false,
    },
    isGrid: {
      type: Boolean,
      required: true,
      default: false,
    },
  },
};
</script>
