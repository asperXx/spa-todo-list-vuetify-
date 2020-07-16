<template>
  <v-container fluid>
    <v-row>
      <v-col cols="8" offset="2">
        <!-- Create tast form -->
        <v-form  ref="form" v-model="valid" validation @submit.prevent="onSubmit">
          <h1>Create task</h1>

          <!-- Task input field -->
          <v-text-field
            clearable
            v-model="title"
            prepend-icon="fa-file-medical"
            clear-icon="fa-times-circle"
            label="New task"
            :rules="taskRules"
            required
          ></v-text-field>

          <!-- Tags -->
          <v-combobox multiple
                    label="Tags" 
                    prepend-icon="fa-tags"
                    chips
                    deletable-chips
                    class="tag-input"
                    v-model="tags"
                    >
          </v-combobox>

          <!-- Description -->
          <v-textarea
          counter="2048"
          label="Description"
          :rules="descRules"
          prepend-icon="fa-scroll"
          :value='descValue'
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

          <v-btn
         type="submit"
          color="primary" 
          :disabled="!valid"
          >Create
          </v-btn>
        </v-form>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      valid: false,
      taskRules: [(v) => !!v || "Task is required"],
      descRules: [v => v.length <= 2048 || 'Max 2048 characters'],
      date: new Date().toISOString().substr(0, 10),
      modal: false,
      descValue: '',
      description: '',
      tags: [],
      title: ''
    };
  },
  methods: {
    onSubmit() {
      const task = {
        title: this.title,
        description: this.description,
        id: Date.now(),
        status: 'active',
        tags: this.tags ,
        deadline: this.date
      }
      this.$store.dispatch('createTask', task)
      this.$router.push('/list')
    },
   
  }
};
</script>
