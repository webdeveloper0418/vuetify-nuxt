<template>
  <v-container class="pending">
    <v-list class="pa-0">
      <v-list-tile avatar>
        <v-list-tile-avatar>
          <img src="https://randomuser.me/api/portraits/men/85.jpg">
        </v-list-tile-avatar>
        <v-list-tile-content>
          <v-list-tile-title>John Leider</v-list-tile-title>
        </v-list-tile-content>
      </v-list-tile>
      <v-divider class="overview-header"/>

    </v-list>


    <v-list
      v-if="pending==false"
      two-line
      class="pl-5 pr-5 review-list"
    >
      <v-list-tile>

        <v-list-tile-content class="flex-row review-list-item mr-4">
          <v-list-tile-title class="pl-4 margin-auto">Oui Oui</v-list-tile-title>
          <v-list-tile-title class="pr-5 margin-auto"><span class="float-right">06/12/2018</span></v-list-tile-title>
        </v-list-tile-content>
        <v-list-tile-action>
          <v-btn
            color="warning"
            @click="newpost">
            Review
          </v-btn>
        </v-list-tile-action>
      </v-list-tile>

    </v-list>
    <v-card v-if="pending==true">
      <v-stepper
        v-model="e6">
        <v-stepper-step
          :complete="e6 > 1"
          step="1">
          Review Post
        </v-stepper-step>

        <v-stepper-content step="1">

          <label class="custom-lc">Photo</label>
          <dropzone
            id="foo"
            ref="el"
            v-model="dropzoneurl"
            :options="dropOptions"/>
          <label class="custom-lc">Date</label>
          <v-menu
            ref="menu1"
            :close-on-content-click="false"
            v-model="menu1"
            :nudge-right="40"
            lazy
            transition="scale-transition"
            offset-y
            full-width
            max-width="290px"
            min-width="290px"
          >
            <v-text-field
              slot="activator"
              v-model="dateFormatted"
              hint="MM/DD/YYYY format"
              persistent-hint
              prepend-icon="event"
              outline
              @blur="date = parseDate(dateFormatted)" />
            <v-date-picker
              v-model="date"
              no-title
              @input="menu1 = false" />
          </v-menu>
          <label class="custom-lc">Title</label>
          <v-text-field
            v-model="title"
            outline
            required />
          <label class="custom-lc">Description</label>
          <v-textarea
            v-model="description"
            outline />
          <label class="custom-lc">Select Category</label>
          <v-select
            :items="items"
            v-model="category"
            outline />
          <div style="float: right">
            <v-btn
              outline
              color="info"
              @click="pending=false"
            >Cancel</v-btn>
            <v-btn
              color="info"
              @click="pending=false">Post</v-btn>
          </div>

        </v-stepper-content>
      </v-stepper>
    </v-card>
  </v-container>
</template>
<style>
.pending input {
  margin: auto;
}
.v-select__selections {
  padding: 0 !important;
  margin: auto;
}
.margin-auto {
  margin: auto;
}
.review-list-item {
  height: 50px;
}
.review-list-item {
  border: 2px dotted;
  border-radius: 7px;
}
.review-list button {
  border-radius: 6px;
}
</style>
<script>
import Dropzone from 'nuxt-dropzone'
import 'nuxt-dropzone/dropzone.css'
export default {
  components: {
    Dropzone
  },
  data: vm => ({
    category: 'web',
    tab: null,
    items: ['web', 'shopping', 'videos', 'images', 'news'],
    pending: false,
    title: 'Debating what to purchase with my money',
    description:
      'I had 40 dollars and 15minutes to figure out what I wanted to buy from disnelyand.' +
      'Too many options',
    dateFormatted: vm.formatDate(new Date().toISOString().substr(0, 10)),
    date: new Date().toISOString().substr(0, 10),
    menu1: false,
    computedDateFormattedMomentjs: '',
    e6: 1,
    options: {
      url: 'http://guru.org/'
    },
    dropzoneurl: 'https://randomuser.me/api/portraits/men/85.jpg',
    dropOptions: {
      url: 'https://randomuser.me/api/portraits/men/85.jpg',
      maxFilesize: 2, // MB
      maxFiles: 4,
      chunking: true,
      chunkSize: 500, // Bytes
      thumbnailWidth: 150, // px
      thumbnailHeight: 150,
      addRemoveLinks: true,
      dictDefaultMessage:
        '<p class="dropzone-span">Drag and drop your files</p>' +
        'anywhere or browse'
    }
  }),
  computed: {
    computedDateFormatted() {
      return this.formatDate(this.date)
    }
  },
  watch: {
    date(val) {
      this.dateFormatted = this.formatDate(this.date)
    }
  },
  methods: {
    newpost() {
      this.pending = true
    },
    formatDate(date) {
      if (!date) return null

      const [year, month, day] = date.split('-')
      return `${month}/${day}/${year}`
    },
    parseDate(date) {
      if (!date) return null
      const [month, day, year] = date.split('/')
      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
    }
  }
}
</script>
