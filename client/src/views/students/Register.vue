<template>
  <div class="my-10 mx-10">
    <h2>학생 등록</h2>
    <v-form v-model="valid">
    <v-container style="max-width: 1000px;">
      <v-row>
        <v-col
          cols="12"
          md="6"
        >
          <v-text-field
            v-model="stuInfo.serial"            
            :counter="20"
            :rules="serialRules"
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
          <v-overflow-btn
          class="my-2"
          :items="options.optionsArea"
          label="지역"
          target="#dropdown-example"  
          v-model="stuInfo.area"
        ></v-overflow-btn>
        </v-col>

        <v-col
          cols="12"
          md="3"
        >
          <v-overflow-btn 
          class="my-2"
          :items="options.optionsGisu"
          label="기수" 
          target="#dropdown-example"
          v-model="stuInfo.gisu"
        ></v-overflow-btn>
        </v-col>
        <v-col
          cols="12"
          md="3"
        >
          <v-overflow-btn
          class="my-2"
          :items="options.optionsClass"
          label="반"
          target="#dropdown-example"
          v-model="stuInfo.stu_class"
        ></v-overflow-btn>
        </v-col>
        <v-col
          cols="12"
          md="3"
        >
          <v-overflow-btn
          class="my-2"
          :items="options.optionsPart"
          label="구분"
          target="#dropdown-example"
          v-model="stuInfo.part"
        ></v-overflow-btn>
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
</template>

<script>
import GetSerial from '@/components/students/GetSerial.vue'
import { mapActions, mapGetters} from 'vuex'

const studentsStore = 'studentsStore'

export default {
  name: 'Register', 
  components: {GetSerial},
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
        optionsGisu: [     
          {text: '3기', value: 3},
          {text: '4기', value: 4}
        ],
        optionsPart: [
          'A', 'B', 'C'
        ],
        optionsClass: [1, 2, 3, 4, 5, 6],
      }
    }
  }, // data
  methods: {
    ...mapActions(studentsStore, [ 
      'registerstudent',
      'initializeserial'
    ]),

    submit() {     
      const registerInfo = this.stuInfo
      this.registerstudent(registerInfo)   
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
      'nameRules',
      'serialRules'
    ])
  }
}
</script>

<style>

</style>