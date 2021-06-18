<template>
  <v-app>
    <v-container>
      <v-row justify="center">
        <v-col col="12" sm="6">
          <v-select :items="categoryItems" label="Category" v-model="category"></v-select>
        </v-col>
      </v-row>
      <v-row justify="center">
        <p class="text-h5">Name & Describe Your Event</p>
      </v-row>
      <v-row justify="center">
        <v-col col="12" sm="6"> <v-text-field label="Title" v-model="title"> </v-text-field></v-col>
      </v-row>
      <v-row justify="center">
        <v-col col="12" sm="6">
          <v-textarea
            label="Description of your event"
            rows="2"
            prepend-icon="mdi-text-subject"
            v-model="description"
          ></v-textarea
        ></v-col>
      </v-row>
      <v-row justify="center">
        <p class="text-h5">Where is Your Event?</p>
      </v-row>
      <v-row justify="center">
        <v-col col="12" sm="6">
          <v-text-field label="Location" prepend-icon="mdi-map-marker" v-model="location">
          </v-text-field
        ></v-col>
      </v-row>

      <v-row justify="center">
        <p class="text-h5">When is Your Event?</p>
      </v-row>
      <v-row justify="center">
        <v-col col="12" sm="6">
          <v-menu
            v-model="dateMenu"
            :close-on-content-click="false"
            transition="scale-transition"
            offset-y
            max-width="290px"
            min-width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="dateFormatted"
                label="Date"
                hint="DD/MM/YYYY format"
                readonly
                prepend-icon="mdi-calendar-text"
                persistent-hint
                v-bind="attrs"
                @blur="date = parseDate(dateFormatted)"
                v-on="on"
              ></v-text-field>
            </template>
            <v-date-picker v-model="date" no-title @input="dateMenu = false"></v-date-picker>
          </v-menu>
        </v-col>
      </v-row>

      <v-row justify="center">
        <v-col col="12" sm="6">
          <v-menu
            ref="menu"
            v-model="timeMenu"
            :close-on-content-click="false"
            :nudge-right="40"
            :return-value.sync="time"
            transition="scale-transition"
            offset-y
            max-width="290px"
            min-width="290px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-model="time"
                label="Time"
                hint="24 hour format"
                persistent-hint
                prepend-icon="mdi-clock-outline"
                readonly
                v-bind="attrs"
                v-on="on"
              ></v-text-field>
            </template>
            <v-time-picker
              v-if="timeMenu"
              v-model="time"
              format="24hr"
              no-title
              @click:minute="$refs.menu.save(time)"
            ></v-time-picker>
          </v-menu>
        </v-col>
      </v-row>
      <v-row justify="center">
        <v-btn @click="submit" class="mr-4 primary">submit</v-btn>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>
export default {
  name: 'App',

  data: vm => ({
    categoryItems: ['Fun', 'Work', 'Social', 'Sport'],
    date: null,
    dateFormatted: vm.formatDate(vm.date),
    dateMenu: false,
    timeMenu: false,
    time: null,
    title: null,
    location: null,
    description: null,
    category: null,
  }),
  watch: {
    date() {
      this.dateFormatted = this.formatDate(this.date);
    },
  },
  computed: {
    computedDateFormatted() {
      return this.formatDate(this.date);
    },
  },
  methods: {
    formatDate(date) {
      if (!date) return null;

      const [year, month, day] = date.split('-');
      return `${day}/${month}/${year}`;
    },
    parseDate(date) {
      if (!date) return null;

      const [day, month, year] = date.split('/');
      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`;
    },
    submit() {
      console.log('Data:');
      console.log(this.category, this.title);
      console.log(this.description);
      console.log(this.location, this.date, this.time);
    },
  },
};
</script>
