<template>
  <!-- <div class="mb-10 mx-10">
    <v-container>
      <v-row>
        <v-spacer></v-spacer>
        <v-col class="d-flex" cols="12" sm="4">
          <v-text-field
            v-model="search"
            append-icon="mdi-magnify"
            label="Search"
            single-line
            hide-details
          ></v-text-field>
        </v-col>
      </v-row>
    </v-container>-->
    <v-data-table

      :headers="headers"
      :items="displayStudents"
      :single-select="singleSelect"
      item-key="id"
          
      :search="search"
      class="elevation-1"
    >
      <template v-slot:item.actions="{ item }">
        <v-icon
          small
          class="mr-2"
          @click="editItem(item)"
        >
          mdi-pencil
        </v-icon>
        <v-icon
          small
          @click="deleteItem(item)"
        >
          mdi-delete
        </v-icon>
      </template>
    </v-data-table>
   <!-- </div> -->
</template>

<script>
export default {
  name: 'StudentsListView',
  props: {displayStudents: {type: Array}, search: {type: String}},
  data() {
    return {
      selected: [],
      //서치삭제
      singleSelect: false,
      headers: [
        { text: '이름', value: 'name' },
        { text: '지역', value: 'area' },
        { text: '기수', value: 'gisu'},
        { text: '반', value: 'stu_class' },
        { text: '시리얼번호', value: 'serial'},
        { text: '수정/삭제', value: 'actions', sortable: false },
      ],
    }
  },
  methods: {
    editItem(stu) {
      const no = stu.no
      this.$router.push(`/students/${no}`)      
    },
    deleteItem(stu) {      
      this.$emit('delete', stu)
    }
  },
}
</script>

<style>

</style>