<template>
  <div class="mx-10 my-10">
    <h2>학생 관리  
    </h2>
    <v-container id="dropdown-example" style="max-width: 1100px;" >
      <v-row>
        <v-col cols="12" sm="2">
          <v-select
            class="my-2"
            :items="options.optionsArea"
            label="지역"
            target="#dropdown-example"  
            v-model="selected.selectedArea"
          ></v-select>
        </v-col>
        <v-col cols="12" sm="2">
          <v-select
            class="my-2"
            :items="options.optionsGisu"
            label="기수" 
            target="#dropdown-example"
            v-model="selected.selectedGisu"
          ></v-select>
        </v-col>
        <v-col cols="12" sm="2" >
          <v-select
            class="my-2"
            :items="options.optionsClass"
            label="반"
            target="#dropdown-example"
            v-model="selected.selectedClass"
            multiple
          ></v-select>
        </v-col>
        <v-col cols="12" sm="4">
          <div class="search">
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
          </div>
        </v-col>
        <v-col cols="12" sm="2">
          <div class="routerbutton">
            <router-link class="btn btn-success" style="color: white" to="/register">학생 등록</router-link>
          </div>
        </v-col>
      </v-row>
    </v-container>
    <v-container style="max-width: 1300px;" >
      <StudentsListView :displayStudents="displayStudents" :search="search" @delete="deleteStudent" />
    </v-container>
  </div>
</template>

<script>
import StudentsListView from '@/components/students/StudentsListView.vue'
import { mapActions, mapGetters } from 'vuex'

const studentsStore = 'studentsStore'
const sessionStore = 'sessionStore'

export default {
  name: 'Students', 
  data() {
    return {
      displayStudents: [],
      search: '',
    }
  },
  created() {
    this.fetchstudents()    
  },
  components: { StudentsListView },
  computed: {
    ...mapGetters(studentsStore, [
      'studentsList',
      'selected',
      'options',
    ]),
    ...mapGetters(sessionStore, [
      'isLoggedIn'
    ])
  },
  watch: {
    studentsList() {     
      this.displayStudents = this.studentsList
    },
    selected: {
      deep: true,
      handler() {
        this.getDisplayList()
      }
    }
  },
  methods: {
    ...mapActions(studentsStore, [
      'fetchstudents',
      'deletestudent'
    ]),
    deleteStudent(stu) {
      this.deletestudent(stu)
    },   
    getDisplayList() {
      let result = this.studentsList
      const area = this.selected.selectedArea
      const gisu = this.selected.selectedGisu
      const part = this.selected.selectedPart
      const classes = this.selected.selectedClass   
      if (area !== '전체') {          
        result = result.filter(student => student.area == area)
      }
      if (gisu !== '전체') {
        result = result.filter(student => student.gisu == gisu)
      } 
      if (classes.length > 0) {
        result = result.filter(student => classes.includes(student.stu_class))
      }
      if (part !== '전체') {
        result = result.filter(student => student.part == part)
      }
      this.displayStudents = result
    },   
  },

}
</script>
<style>
.search{
  padding-top: 4px;
}
.routerbutton{
  padding-top: 16px;
}

</style>