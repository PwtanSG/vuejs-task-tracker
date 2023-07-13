<template>
  <!-- <div
    v-on:click="$emit('toggle-reminder', task.id)"
    :class="[task.reminder ? 'reminder' : '', 'task']"
  > -->
  <div class='task' :class="[task.reminder ? 'reminder' : '', 'task']">
    <h3>
      {{ task.text }}
      <div>
        <i @click="$emit('toggle-reminder', task.id)" :class="task.reminder ? 'fas fa-check-square blue' : 'fas fa-check-square gray'"></i>
        <i @click="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'fas fa-clock blue' : 'fas fa-clock gray'] "></i>
        <i @click="$emit('delete-task', task.id)" class="fas fa-times red"></i>
      </div>
    </h3>
    <p>{{ task.day }}</p>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object,
  },
  methods: {
    // test(id){
    //   console.log(id)
    // }
    // longer method
    // onDelete(id){
    //   // console.log(id)
    //   this.$emit('delete-task', id)
    // }
    // onReminder(id) {
    // console.log(id)
    // this.$emit("toggle-reminder", id);
    // },
  },
};
</script>