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
      <v-row class="d-flex flex-column">
        <v-column>
          <v-text-field
            v-model="searchValue"
            :rules="[rules.required, checkInputValueValidation]"
          >
        </v-text-field>
      </v-column> 
      <v-column>
        <v-btn @click="clickValidationBtn">VALIDATION CHECK</v-btn>
      </v-column>
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
    <v-row class="d-flex justify-center align-center">
      <h1>[Child component value emitting test]</h1>       
    </v-row>
    <v-row>
      <CommonComponent @update-selected-value="updateValueFromChildFunc"/>
    </v-row>
    <v-row>
      {{ updatedValueFromChild }}
    </v-row>
    <v-row>
      <h1>(Example #1)[click event validation check] rules에서 validation check를 클릭시에 될 수 있도록 처리</h1>
      <p>:rules properties는 입력 element가 한 번 dirty가 된 상태에서 동적으로 적용된 rule이 적용되어, validation check를 해준다.</p>
    </v-row>
    <v-row>
      <v-textarea
        v-model="queryStatement"
        :rules="initialRules"
        ref="queryTextArea"
      ></v-textarea>
      <v-btn
        @click="checkValidation"
      >check query validation</v-btn>
    </v-row>
    <v-row>
      <h1>(Example #2) [click event validation check] error-messages에서 validation check를 클릭시에 될 수 있도록 처리(in error-messages properties)</h1>
      <p>:error-messages properties는 입력 element의 dirty 유/무와 관계없이, 동적으로 적용된 :error-messages의 속성 errors(validation check method)이 적용되어 검사를 해준다.</p>
    </v-row>
    <v-row>
      <v-textarea
        v-model="queryInputStatement"
        ref="queryInputTextArea"
        :error-messages="errors"
      ></v-textarea>
      <v-btn
        @click="checkInputValidation"
      >check query input validation</v-btn>
    </v-row>
    <v-row>
      <h1>POPUP DIALOG 처리 PRACTICE</h1>
    </v-row>
    <v-row>
      <v-btn @click="openErrorDialog">OPEN Popup Dialog</v-btn>
    </v-row>
  </v-container>
  <ErrorPopupDialog v-if="isErrorPopupShown" :errorMessage="errorMessage" @closeErrorDialog="closeErrorDialog" />
  
</v-app>
</template>

<script>
import CommonComponent from './components/common/commonComponent.vue';
import ErrorPopupDialog from './components/common/errorPopupDialog.vue';

export default {
  name: 'App',

  components: {
    CommonComponent,
    ErrorPopupDialog
  },
  mounted() {
    this.fetchInitialEmployeeDataSet();
    this.checkExistenceOfEmployee();
  },
  data: () => ({
    errors: [],
    errorMessage: '',
    isErrorPopupShown: false,
    queryInputStatement: '',
    queryStatement: '',
    searchValue: '', 
    headers: [     
      {text: 'ID', value: 'id'}, 
      {text: 'NAME', value: 'name'}, 
      {text: 'GENDER', value: 'gender'}, 
      {text: 'ADDRESS', value: 'address'}, 
      {text: 'DATE OF BIRTH', value: 'dateOfBirth'}
    ],
    rules: {
      required: value => !!value || '값을 입력해주세요.',
    },
    initialRules: [],
    selectedComboboxValue: '(public) 공용 마트',
    filteredComboboxValue: '',
    comboBoxItems: ['(public) 공용 마트' ,'(public1) 공용 마트1', '(public2) 공용 마트2', '(public3) 공용 마트3'],
    tableListItems: [],
    updatedValueFromChild: ''
  }),
  watch: {
    queryStatement: function() {
      this.initialRules = [];
    }
  },
  methods: {
    openErrorDialog: function() {
      this.isErrorPopupShown = true;
    },  
    closeErrorDialog() {
      this.isErrorPopupShown = false;
    },  
    validationFunc: function(val) {
      console.log('val:', val);
      if(val == 'hyungi') {
        return true
      }
      return "다시 입력을 해주세요.";
    },
    checkValidation() {
      console.log('checkValidation function');
      this.initialRules = [
        (val)=> !!val || '필수 입력 입니다.',
        (val)=>{
          if(val == 'hyungi') {
            return true;
          } else {
            return "다시 입력해주세요.";
          }
        }
      ]
      // this.$refs.queryTextArea.validate();
      // let self = this;
      // setTimeout(() => {
      //   if (self.$refs.queryTextArea.validate()) {
      //     alert('sumitted');
      //   }
      // })
    },  
    // (Example #2 click event validation check)
    asyncProcess: function() {
      this.$axios.get('http://localhost:8080/employee')
        .then(response => {
          console.log('CHECK RESPONSE MESSAGE : ', response);
          return true;
        })
        .catch(error => {
          console.log('CHECK ERROR MESSAGE : ', error.message);
          this.errorMessage = error.message;
          this.isErrorPopupShown = true;
        });
    },
    checkInputValidation: function() {
      // 우선 error-messages의 속성으로 들어갈 errors에 규칙을 동적으로 초기화 시켜주고,
      this.errors = [
        (val)=> !!val || '필수 입력 입니다.',
      ];
      // 입력된 값이 undefined이거나 ""(빈 공백)인 경우, validation 기본 입력에 대한 확인만 해주고 method처리를 끝내도록 한다.
      if(this.queryInputStatement == undefined || this.queryInputStatement == "") {
        this.$refs.queryInputTextArea.validate();
        return;
      }
      // 만약 무언가 입력값이 있다면, 비동기 처리는 나중에 실행되도록 해당 method를 실행시켜준다.
      this.asyncProcess();
    },
    updateValueFromChildFunc: function(val) {
      console.log('[called] updateValueFromChildFunc');
      this.updatedValueFromChild = val
    },
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
    },
    checkExistenceOfEmployee: function() {
      const employeeList = this.tableListItems;
      console.log('employeeList in checkExistenceOfEmployee method::', employeeList);
    },
    checkInputValueValidation: function(val) {
      // validation check를 할때에는 우선 해당 페이지, Pop-up dialog component
      // 초기 load시에 체크하고자 하는 항목의 모든 데이터 리스트를 받아서 data 내에 stateful variable로써 등록한다.
      // component가 기본적으로 초기 load되었을때, 값을 DB에서 가져왔기 때문에 해당 값을 이용해서 별도의 method에서 
      // 활용할 수 있다. 

      // 예시) 입력한 데이터 모델의 문자열 값이 현재 서버에 존재하는 데이터 모델명인지 입력하는 실시간으로 확인을 하고자 한다면,
      // 팝업 다이얼로그 로드시에 서버로부터 모든 데이터 모델 리스트를 가져와서 component 내의 date에 stateful variable로써 등록하도록 한다.
      // 등록된 변수를 활용해서 rules에 별도로 custom variable을 등록하도록 한다.
      
      // 입력할때마다 서버로 request를 보내게 되면 서버에 부하를 줄 수 있다.
      console.log('Validation button is clicked');
      const listItems = this.tableListItems;
      console.log('check list items in validation btn click event::', listItems);

      for(const item of listItems) {
        console.log('compare ',val,' with', item.name);
        if(val == item.name) {
          return true;
        }
      } 
      return "Database에 있는 사용자를 입력해주세요.";
    },
    clickValidationBtn: function() {
      console.log('validation check button is clicked');
    }
  }
};
</script>
