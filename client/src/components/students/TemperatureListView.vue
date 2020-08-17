<template>
    <div>
      <v-row class="justify-content-center bg-light fixed-bottom">
        <v-col cols="11" sm="6">
          <v-text-field
            label="전송할 메시지 입력하세요."
            v-model="message"
            placeholder="입력 없을시 온도 측정하세요가 보내집니다."
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="2" class="px-1">
            <v-btn color="success" @click="confirmSelect"><img src="@/assets/mattermost.png" style="cursor:pointer; margin-right:10px; width: 25px; height: auto;">선택 보내기</v-btn>
        </v-col>
        <v-col cols="12" sm="2" class="px-1">
            <v-btn color="pink" style="color: white" @click="confirmAll"><img src="@/assets/mattermost.png" style="cursor:pointer; margin-right:10px; width: 25px; height: auto;">전체 보내기</v-btn>
        </v-col>
      </v-row>
    <br>
    <v-data-table
      :headers="headers"
      :items="displayList"
      item-key="student_number"
      :search="search"
      class="elevation-1 mt-4"
      show-select
      :single-select="singleSelect"
      v-model="selected"
    >
    <template v-slot:item.morning_temp={item}   >
      <span :class="highTempColor(item.morning_temp)">
            {{ 
              (item.morning_temp)
              ? item.morning_temp 
              : "-" 
            }}
      </span>
    </template>


    <template  v-slot:item.morning_time={item}>
            {{ 
              (item.morning_time)
              ? $moment(item.morning_time).format('HH:mm') 
              : "-" 
            }}
    </template>

    <template v-slot:item.lunch_temp={item}>
      <span :class="highTempColor(item.lunch_temp)">
            {{ 
              (item.lunch_temp)
              ? item.lunch_temp 
              : "-"
            }}
      </span>
    </template>

    <template v-slot:item.lunch_time={item}>
      
            {{ 
              (item.lunch_time)
              ? $moment(item.lunch_time).format('HH:mm') 
              : "-" 
            }}

    </template>

    </v-data-table>
  </div>
</template>

<script>

export default {
  name: 'TemperatureListView',
  props: { displayList: {type: Array} ,search: {type: String}},
  data() {
    return {
      singleSelect: false,
      selected: [],
      message:'',
      confirmMessage: false,
      //서치삭제
      dropdown_edit: ['','안녕하세요', '반갑습니다.'],
      disable_pagination: true,
      headers: [
        { text: '이름', value: 'student_name' },
        { text: '지역', value: 'student_area' },
        { text: '기수', value: 'student_gisu' },
        { text: '반', value: 'student_class' },
        { text: '오전 체온', value: 'morning_temp' },
        { text: '오전 시간', value: 'morning_time' },       
        { text: '오후 체온', value: 'lunch_temp' },
        { text: '오후 시간', value: 'lunch_time' },   
      ],
    }
  },
  methods: {
    highTempColor(temp) {
      let color = ''
      temp && Number(temp) >= 37
      ? color = 'red'
      : color = 'black'
      return `${color}--text`
    },
    sendMatter() {
      const data = {
        students: this.selected,
        message: this.message
      }
      this.$emit('mattermost', data)
    },
    sendMatterAll() {
      const data = {
        students: this.displayList,
        message: this.message
      }
      this.$emit('mattermost', data)
    },
    confirmAll() {
      let confirmMessage = confirm('정말 전체 메세지 보내시겠습니까?')
      if (confirmMessage) {
        this.sendMatterAll()
      }
    },
    confirmSelect() {
      let confirmSelectMessage = confirm('선택한 인원에게 메세지를 보내시겠습니까?')
      if (confirmSelectMessage) {
        this.sendMatter()
      }
    }
  },
}
</script>

<style>
</style>