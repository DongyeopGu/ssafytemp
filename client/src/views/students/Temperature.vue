<template>
  <div class="my-10 mx-10">
    <h2>체온 확인</h2>
    <!-- <button class="btn btn-success" style="color: white" @click="exportData">xlsx 저장</button> -->
    <v-container id="dropdown-example" style="max-width: 1000px;" >
      <v-row>
        <v-col cols="12" sm="3">
          <v-select
            class="my-2"
            :items="options.optionsArea"
            label="지역"
            target="#dropdown-example"  
            v-model="selected.selectedArea"
          ></v-select>
        </v-col>
        <v-col cols="12" sm="3">
          <v-select 
            class="my-2"
            :items="options.optionsGisu"
            label="기수" 
            target="#dropdown-example"
            v-model="selected.selectedGisu"
          ></v-select>
        </v-col>
        <v-col cols="12" sm="3">
          <v-select
            class="my-2"
            :items="options.optionsClass"
            label="반"
            target="#dropdown-example"
            v-model="selected.selectedClass"
            multiple
          ></v-select>
        </v-col >
        <v-col cols="12" sm="3" >
          <div class="xlsxbutton">
          <v-btn  color="success" @click="exportData">xlsx <i class="material-icons">save_alt</i></v-btn>
          </div>
        </v-col>
      </v-row>
    </v-container>
    <!-- 하단 -->
    <v-container fluid style="max-width: 1300px;">
      <v-card >
        <v-card-title>
          <v-row>
            <v-col cols="12" sm="3" md="3">
              <v-menu
                ref="menu"
                v-model="menu"
                :close-on-content-click="false"
                :return-value.sync="pickDate"
                transition="scale-transition"
                offset-y
                min-width="290px"
              >
                <template v-slot:activator="{ on, attrs }">
                  <v-text-field
                    v-model="formatPickDate"
                    label="날짜 선택"
                    prepend-icon=""
                    readonly
                    v-bind="attrs"
                    v-on="on"
                  ></v-text-field>
                </template>
                <v-date-picker v-model="pickDate" no-title scrollable> 
                  <v-spacer></v-spacer>
                  <v-btn text color="primary" @click="menu = false">Cancel</v-btn>
                  <v-btn text color="primary" @click="selectDate">OK</v-btn>
                </v-date-picker>
              </v-menu>
            </v-col>
 
            <v-col class="d-inline-block justify-content-around d-flex align-items-center px-10" cols="12" sm="6">
              
              <div class="d-inline-block mr-1">
                <small>미측정:</small>
              </div>
              
              <div class="form-check form-check-inline">
                <input type="checkbox" id="inlineCheckbox1" v-model="selected.amCheck">
                <label class="form-check-label pl-2" for="inlineCheckbox1"><small>오전</small></label>
              </div>
              <div class="form-check form-check-inline">
                <input type="checkbox" id="inlineCheckbox2" v-model="selected.pmCheck">
                <label class="form-check-label pl-2" for="inlineCheckbox2"><small>오후</small></label>
              </div>
            </v-col>
            <v-col class="d-flex" cols="12" sm="3">
              <v-text-field
                v-model="search"
                append-icon="mdi-magnify"
                label="Search"
                single-line
                hide-details
              ></v-text-field>
            </v-col>
          </v-row>
        </v-card-title>
        <TemperatureListView :displayList="displayList" :search="search" @mattermost="sendMatter" />
      </v-card>
    </v-container>
  </div>
</template>

<script>
import TemperatureListView from '@/components/students/TemperatureListView.vue'
import XLSX from 'xlsx'
import { mapActions, mapGetters } from 'vuex'
const sessionStore = 'sessionStore'
const studentsStore = 'studentsStore'
export default {
  name: 'Temperature',
  components: {
    TemperatureListView,
  },
  data() {
    return {
      pickDate: new Date().toISOString().substr(0, 10),
      menu: false,
      singleSelect: false,
      search: '',      
      displayList: [], 
    }
  },
  methods: {
    ...mapActions(sessionStore, [
      'fetchtemplist',
      'gettemplist',
    ]),
    ...mapActions(studentsStore, [
      'resetselected',
      'sendmatter',
    ]),
    selectDate() {
      this.$refs.menu.save(this.pickDate)
      this.gettemplist(this.pickDate)
      this.resetselected()
    },
    makeDisplayList() {
      this.displayList = this.studentsList
    },   
    getDisplayList() {
      let result = this.studentsList
      const area = this.selected.selectedArea
      const gisu = this.selected.selectedGisu
      const classes = this.selected.selectedClass  
      const amCheck = this.selected.amCheck 
      const pmCheck = this.selected.pmCheck
      if (area !== '전체') {          
        result = result.filter(data => data.student_area == area)
      }
      if (gisu !== '전체') {
        result = result.filter(data => data.student_gisu == gisu)
      } 
      if (classes.length > 0) {
        result = result.filter(data => classes.includes(data.student_class))
      }

      if (amCheck) {
        result = result.filter(data => !data.morning_temp )
      }
      if (pmCheck) {
        result = result.filter(data => !data.lunch_temp)
      }
      this.displayList = result
    },    
    exportData() {
      const sheetName = `${this.pickDate}_${this.selected.selectedGisu}기_${this.selected.selectedArea}_${this.selected.selectedClass}반 체온 측정 현황`
      const stuTempList = XLSX.utils.json_to_sheet(this.displayList)
      const wb = XLSX.utils.book_new()
      const bookName = this.selected.selectedArea + '_' + this.selected.selectedGisu + '기_' + this.selected.selectedClass + '반'
      XLSX.utils.book_append_sheet(wb, stuTempList, bookName)
      XLSX.writeFile(wb, sheetName + '.xlsx')
    },
    sendMatter(data) {
      this.sendmatter(data)
    }
  },
  computed: {
    ...mapGetters(sessionStore, [
      'tempList',
      'displayDate'
    ]),
    ...mapGetters(studentsStore, [
      'selected',
      'options'
    ]),
    studentsList() {
      return this.tempList
    },
    formatPickDate() {
      return this.$moment(this.pickDate).format('YYYY.MM.DD')
    }
  },
  watch: {
    selected: {
      deep: true,      
      handler() {
        this.getDisplayList()
      }
    },

    tempList: {
      deep: true,
      handler() {
        this.makeDisplayList()
      }
    }
  },
  created() {
    this.fetchtemplist()
    this.makeDisplayList()
  }
}
</script>

<style>
.xlsxbutton{
  padding-top: 16px;
}
.site-footer{
  margin-bottom: 40px;
}
</style>