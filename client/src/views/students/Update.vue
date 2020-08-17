<template>
  <div class="my-10 mx-10">
    <div  v-if="student">
    <h2>학생 수정</h2>
    <v-form v-model="valid">
    <v-container>
      <v-row>
        <v-col
          cols="12"
          md="6"
        >
          <v-text-field
            v-model="stuInfo.serial"
            :rules="serialRules"
            :counter="20"
            label="Serial Number"
            required
          >
          <template v-slot:append>
              <v-btn
                color="success"
                @click="dialogOn"
              >
                등록
              </v-btn>
            </template>
          </v-text-field>
        <v-dialog
          v-model="dialog"
          max-width="1080"
        >
          <v-card>
              <GetSerial @close="closeModal" @getSerialNum="getSerialNum"/>
            <v-card-actions>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
        </v-col>

        <v-col
          cols="12"
          md="6"
        >
          <v-text-field
            v-model="stuInfo.name"
            :rules="nameRules"
            :counter="10"
            label="이름"
            required
          ></v-text-field>
        </v-col>
       
        <v-col
          cols="12"
          md="3"
        >
          <v-select
          class="my-2"
          :items="options.optionsArea"
          label="지역"
          target="#dropdown-example"  
          v-model="stuInfo.area"
        ></v-select>
        </v-col>

        <v-col
          cols="12"
          md="3"
        >
          <v-select
          class="my-2"
          :items="options.optionsGisu"
          label="기수" 
          target="#dropdown-example"
          v-model="stuInfo.gisu"
        ></v-select>
        </v-col>

        <v-col
          cols="12"
          md="3"
        >
          <v-select
          class="my-2"
          :items="options.optionsClass"
          label="반"
          target="#dropdown-example"
          v-model="stuInfo.stu_class"
        ></v-select>
        </v-col>
        <v-col
          cols="12"
          md="3"
        >
          <v-select
          class="my-2"
          :items="options.optionsPart"
          label="구분"
          target="#dropdown-example"
          v-model="stuInfo.part"
        ></v-select>
        </v-col>


      </v-row>
    </v-container>
  </v-form>
  <v-btn
        :disabled="!valid"
        color="success"
        @click="submit"
      >
        submit
      </v-btn>
  </div>
  </div>
</template>

<script>
import GetSerial from '@/components/students/GetSerial.vue'
import {mapActions, mapGetters} from 'vuex'
const studentsStore = 'studentsStore'
export default {
  name: 'Update',
  components: {GetSerial},
  // props: {
  //   no: Number
  // },
  data() {
    return {
      dialog: false,
      valid: false,
      serial: '',
      username: '',
      stuInfo: {
        serial: '',
        name: '',
        area: '서울',
        gisu: 3,
        stu_class: 1,
        part: 'A'     
      },  
      options: {     
        optionsArea: [       
          '서울', '대전', '구미', '광주'
        ],
        optionsGisu: [3, 4],
        optionsPart: [
          'A', 'B', 'C'
        ],
        optionsClass: [...Array(20).keys()].map(i => i+1),
      }
    }
    
  },
  methods: {
    ...mapActions(studentsStore, [
      'getstudent',
      'updatestudent',
      'initializeserial'
    ]),
    submit() {
      this.updatestudent(this.stuInfo)
    },
    closeModal() {
      this.dialog = false
    },
    getSerialNum(data) {
      this.stuInfo.serial = data
      this.dialog = false
    },
    dialogOn() {
      this.initializeserial()
      this.dialog = true
    }
  },
  computed: {
    ...mapGetters(studentsStore, [
      'student',
      'nameRules',
      'serialRules'
    ])
  },
  watch: {
    student() {
      this.stuInfo = this.student
    }
  },
  created() {
    this.getstudent(this.$route.params.no)
  },
}
</script>

<style>

</style>