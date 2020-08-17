<template>
  <div class="my-10 mx-10">
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
            :counter="10"
            label="Serial Number"
            required
          ></v-text-field>
        </v-col>

        <v-col
          cols="12"
          md="6"
        >
          <v-text-field
            v-model="username"
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
          v-model="options.selected.selectedArea"
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
          v-model="options.selected.selectedGisu"
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
          v-model="options.selected.selectedClass"
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
          v-model="options.selected.selectedPart"
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

export default {
  name: 'Register',
  data() {
    return {
      valid: false,
      serial: '',
      serialRules: [
        v => !!v || 'Serial Number is required',
        v => v.length <= 10 || 'Serial Number must be less than 10 characters',
      ],
      username: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => v.length <= 10 || 'Name must be less than 10 characters',
      ],
      areaList: {
        1: '서울',
        2: '대전',
        3: '구미',
        4: '광주'
      },
      partList: {
        1: 'A',
        2: 'B',
        3: 'C'
      },
      options: {
        selected: {
          selectedArea: 1,
          selectedGisu: 3,
          selectedClass: 1,
          selectedPart: 1
        },
        optionsArea: [        
          {text: '서울', value: 1},
          {text: '대전', value: 2},
          {text: '구미', value: 3},
          {text: '광주', value: 4},
        ],
        optionsGisu: [     
          {text: '3기', value: 3},
          {text: '4기', value: 4}
        ],
        optionsPart: [
          {text: 'A', value: 1},
          {text: 'B', value: 2},
          {text: 'C', value: 3}
        ],
        optionsClass: [1, 2, 3, 4, 5, 6],
      }
    }
  }, // data
  created() {
            this.$socket.on('return', (data)=> {
                this.stuInfo.serial = data.message
            })
        },
  
  methods: {

    submit() {
      const registerInfo = {
        'name': this.username,
        'area': this.areaList[this.options.selected.selectedArea],
        'gisu': this.options.selected.selectedGisu,
        'part': this.partList[this.options.selected.selectedPart],
        'stu_class': this.options.selected.selectedClass,
        'serial': this.serial,
      }
      
      this.$emit('register', registerInfo)
      
    }
  }
}
</script>

<style>

</style>