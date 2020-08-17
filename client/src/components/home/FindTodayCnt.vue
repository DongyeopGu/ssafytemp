<template>
  <div>
    <h3 style="text-align : left;" >오늘 체온측정 현황</h3>
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
        </v-form>
        <v-text-field
          disabled=""
          label="오늘 날짜"
          v-model="today"
      ></v-text-field>

      <v-btn
        :disabled="!valid"
        color="success"
        @click.stop="dialog = true"
        @click="todaytempcnt(selected)"
        :dialog="dialog"
      >
        submit
      </v-btn>
      <v-dialog
      v-model="dialog"
      max-width="290"
      >
        <v-card>
          <div class="pop-head" >
          <strong class="tit">오늘 체온측정 현황</strong>
          </div>
          <v-card-text v-if="todayTempCnt" class="pt-4 pb-0">
            <h5>{{todayTempCnt.구분.지역}} {{todayTempCnt.구분.기수}}기 {{todayTempCnt.구분.반}}반</h5>
            <p>오전 미측정: {{todayTempCnt.오전미측정}} 명 | 오전 측정: {{todayTempCnt.오전측정}} 명</p>
            <p>오후 미측정: {{todayTempCnt.오후미측정}} 명 | 오후 측정: {{todayTempCnt.오후측정}} 명</p>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="green darken-1"
              text
              @click="dialog = false"
            >
              확인
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
  </div>
</template>

<script>
const homeStore = 'homeStore'
import { mapGetters, mapActions} from 'vuex'
export default {
  name: 'FindTodayCnt',
  data () {
    return {
      valid: false,
      dialog: false,
      todayTempCnt: null,
      commonRules: [
        v => !!v || 'Required.'
      ],
      selected: {
        area: '',
        gisu: '',
        class: '',
      },
      options: {
        optionsArea: ['전체','서울', '대전', '구미', '광주'],
        optionsGisu: ['전체',3, 4],
        optionsClass: ['전체', ...[...Array(20).keys()].map(i => i+1)],
      }       
    }
  },
  methods: {
    ...mapActions(homeStore, [
      'todaytempcnt'
    ])
  },
  computed:{
    ...mapGetters(homeStore, [
        'sampleTodayCnt',
    ]),
    today() {
      return this.$moment().format('YYYY.MM.DD (dd)')
    }
  },
  watch:{
    sampleTodayCnt: {
      deep: true,
      handler() {
        this.todayTempCnt = this.sampleTodayCnt
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