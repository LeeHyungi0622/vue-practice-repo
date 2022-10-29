<template>
  <v-app>
    <v-container>
      <v-row class="d-flex justify-center align-center">
        <h1>[Vuetify component test page]</h1>       
      </v-row>
      <v-row>
        <v-row class="pl-4 pt-8">
          <h3>(1) v-combobox에 출력되는 값을 Customize하고, 서버로 query string을 보낼때 어떤 식으로 처리할지에 대한 테스트</h3>
        </v-row>
        <v-combobox
          v-model="selectedComboboxValue"
          :items="comboBoxItems"
        >
        </v-combobox>
      </v-row>
      <v-row no-gutters class="d-flex flex-column pt-4 pb-4">
        <v-column cols="6">
            Selected value is {{ selectedComboboxValue }}
        </v-column>
        <v-column cols="6" class="mt-4 mb-4">
          <v-btn @click="filterSelectedComboboxValue">filter</v-btn>
        </v-column>
      </v-row>
      <v-row class="pt-4 pb-4">
        Filtered selected value is {{ filteredComboboxValue }}
      </v-row>
      <v-row>
        <h3>(2) 입력한 값이 서버에 있는 값인지 아닌지 확인해서 에러 메시지 출력</h3>
      </v-row>
      <v-row>
        서버 내 사용자 정보 리스트 출력 
        <v-container>
          <v-data-table
            :headers="headers"
            :items="tableListItems"
            :items-per-page="5"
            class="elevation-1"
          >
          </v-data-table>
        </v-container>
      </v-row>
    </v-container>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  components: {
  },
  mounted() {
    this.fetchInitialEmployeeDataSet();
  },
  data: () => ({
    headers: [     
      {text: 'ID', value: 'id'}, 
      {text: 'NAME', value: 'name'}, 
      {text: 'GENDER', value: 'gender'}, 
      {text: 'ADDRESS', value: 'address'}, 
      {text: 'DATE OF BIRTH', value: 'dateOfBirth'}
    ],
    selectedComboboxValue: '(public) 공용 마트',
    filteredComboboxValue: '',
    comboBoxItems: ['(public1) 공용 마트1', '(public2) 공용 마트2', '(public3) 공용 마트3'],
    tableListItems: []
  }),
  methods: {
    // 우선 기존에 객체 형태로 combobox에 출력하던 부분을 위와 같이 문자열 리스트 형태로
    // Custom한 문자열을 구성하도록 한다.
    // 단, 선택한 item의 값은 API query string의 값으로 넘겨줄때, 아래와 같이 값을 filter해서 보내도록 보내도록 한다.
    filterSelectedComboboxValue: function() {
      console.log('filterSelectedComboboxValue button is clicked!');
      const filteredSelectedComboboxVal = this.selectedComboboxValue.split(' ')[0].replace('(',"").replace(")","");
      this.filteredComboboxValue = filteredSelectedComboboxVal;
      console.log('Length of filtered item is ', this.filteredComboboxValue.length);
    },
    fetchInitialEmployeeDataSet: function() {
      this.$axios.get('http://localhost:8080/employee')
        .then(response => {
          console.log('response::', response);
          let items = []

          response.data.forEach(item => {
            const sItem = {
              id: item.id,
              name: item.name,
              gender: item.gender,
              address: item.address,
              dateOfBirth: item.dateOfBirth
            }
            items.push(sItem);
          })

          this.tableListItems = items;
        })
        .catch(error => {
          console.log('error:', error);
        })
    }
  }
};
</script>
