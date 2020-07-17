<template>
  <div>
    <v-row>
      <v-col cols="8" offset="2">
        <div v-if="task">       

        <v-form
          ref="form"
          v-model="valid"
          validation
          :disabled="onEdit"
        >
          <h1>{{task.title}}</h1>

          <!-- Tags -->
          <v-combobox
            multiple
            label="Tags"
            prepend-icon="fa-tags"
            chips
            deletable-chips
            class="tag-input"
            v-model="task.tags"
          >
          </v-combobox>

          <!-- Description -->
          <v-textarea
            counter="2048"
            label="Description"
            :rules="descRules"
            prepend-icon="fa-scroll"
            v-model="description"
          ></v-textarea>

          <!-- Date picker -->
          <v-dialog
            ref="dialog"
            v-model="modal"
            :return-value.sync="date"
            persistent
            width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="date"
                label="Deadline"
                prepend-icon="fa-clock"
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="date" scrollable>
              <v-spacer></v-spacer>
              <v-btn text color="primary" @click="modal = false">Cancel</v-btn>
              <v-btn text color="primary" @click="$refs.dialog.save(date)"
                >OK</v-btn
              >
            </v-date-picker>
          </v-dialog>
         <div v-if="task.status !== 'completed'">
              <v-btn class="mr-5" v-if="onEdit" @click="onEdit=false">Edit</v-btn>  
              <v-btn v-else @click.prevent="onSubmit" color="primary" :disabled="!valid"
                >SAVE
              </v-btn>
              <v-btn color="primary" @click="completeTask">COMPLETE TASK</v-btn>
         </div>
        </v-form>
        </div>
        <p v-else>Not Found!!!</p>
      </v-col>
    </v-row>
  </div>
</template>

<script>
export default {
  computed: {
    task() {
      return this.$store.getters.taskById(+this.$route.params.id);
    },
  },
  data() {
    return {
      valid: false,
      descRules: [(v) => v.length <= 2048 || "Max 2048 characters"],
      date: null,
      modal: false,
      description: "",
      tags: [],
      onEdit: true
    };
  },
  methods: {
    onSubmit() {
      this.$store.dispatch("updateTask", {
        id: this.task.id,
        description: this.description,
        deadline: this.date
      });
      this.$router.push("/list");
    this.onEdit = true;
    },
    completeTask() {
        this.$store.dispatch('completeTask', this.task.id)
        this.$router.push("/list");
    }
  },
  mounted() {
      this.description = this.task.description,
      this.date = this.task.deadline
  }
};
</script>

<style></style>
