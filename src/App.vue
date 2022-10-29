<template>
  <v-app>
    <v-container>
      <v-row class="d-flex justify-center align-center">
        <h1>[Vuetify component test page]</h1>       
      </v-row>
      <v-row>
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
    </v-container>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  components: {
  },

  data: () => ({
    selectedComboboxValue: '',
    filteredComboboxValue: '',
    comboBoxItems: ['(public1) 공용 마트1', '(public2) 공용 마트2', '(public3) 공용 마트3']
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
    }
  }
};
</script>
