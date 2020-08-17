<template>
  <div>
    <h3 style="text-align : left;" >나의 온도 확인하기</h3>
      <hr style="border: 1px solid black;">
      <v-form
      ref="form"
      v-model="valid"
      >
        <v-select
          :items="options.optionsArea"
          v-model="selected.area"
          :rules="commonRules"
          filled
          required
          label="지역"
        ></v-select>

        <v-select
          :items="options.optionsGisu"
          v-model="selected.gisu"
          :rules="commonRules"
          filled
          required
          label="기수"
        ></v-select>

        <v-select
          :items="options.optionsClass"
          v-model="selected.class"
          :rules="commonRules"
          filled
          required
          label="반"
        ></v-select>

        <v-text-field
        v-model="selected.name"
        :rules="commonRules"
        label="이름"
        required
        @keydown.enter="searchstudent(selected)"
      ></v-text-field>

        </v-form>

      <v-btn
        :disabled="!valid"
        color="success"
        @click="searchstudent(selected)"
      >
        submit
      </v-btn>
      <v-dialog
        v-model="dialog"
        max-width="290"
      >
      <v-card>
        <div class="pop-head" >
          <strong class="tit">나의온도</strong>
        </div>
        <v-card-text class="pt-4 pb-0">
          <div v-if="fetchStudent">
            <div v-if="isStudent">
              <h5>{{ sampleStudent.student[0].area}} {{ sampleStudent.student[0].gisu }}기 {{ sampleStudent.student[0].stu_class }}반 {{ sampleStudent.student[0].name }}</h5>
              <p>오늘 측정 온도: {{ sampleStudent.temperature[0].temp }}</p>
            </div>
            <div v-else>{{sampleStudent.message}}</div>
          </div>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="green darken-1"
            text
            @click="dialog = false"
          >
            okay
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>    
  </div>
</template>

<script>
import {mapActions, mapGetters} from 'vuex'
const studentsStore = 'studentsStore'
export default {
  name: 'FindStudent',
  data() {
    return {
      search: '',
      valid: false,
      isStudent: false,
      dialog: false,
      fetchStudent: false,
      commonRules: [
        v => !!v || 'Required.'
      ],
      selected: {
        area: '',
        gisu: '',
        class: '',
        name: '',
      },
      options: {
        optionsArea: ['서울', '대전', '구미', '광주'],
        optionsGisu: [3, 4],
        optionsClass: [...Array(20).keys()].map(i => i+1),
      }
    }
  },
  methods: {
    ...mapActions(studentsStore, [
      'searchstudent'
    ]),
  },
  computed: {
    ...mapGetters(studentsStore, [
      'sampleStudent'
    ])
  },
  watch: {
    sampleStudent: {
      deep: true,
      handler() {
        this.dialog = true
        this.fetchStudent = true
        Object.keys(this.sampleStudent).includes('student')
        ? this.isStudent = true
        : this.isStudent = false      
      }
    },
    dialog() {
      this.$refs.form.reset();
    }
  }
}
</script>

<style>
.pop-head {
    position: relative;
    background: #ffeb77;
    padding: 10px 40px 10px 40px;
}
.pop-head .tit {
    font-size: 20px;
    color: black;
    font-weight: 500;
}
</style>