<template>
    <v-container>
        <v-row>Child component</v-row>
        <v-row>
            <v-combobox
                v-model="selectedValue"
                :items="comboBoxItems"
                @change="onSelectionChange"
            >
            </v-combobox>
        </v-row>
    </v-container>
</template>

<script>
export default {
    name: 'CommonComponent',
    
    components: {

    },
    
    mounted() {
        this.emitMethod();
    },

    data: () => ({
        selectedValue: '(public) 공용 마트',
        comboBoxItems: ['(public) 공용 마트', '(public1) 공용 마트1', '(public2) 공용 마트2', '(public3) 공용 마트3']
    }),
    methods: {
        emitMethod: function() {
            const filteredVal = this.selectedValue.split(' ')[0].replace('(','').replace(')','');
            this.$emit('update-selected-value', filteredVal + ' [from child component]');
        },
        onSelectionChange: function() {
            this.emitMethod();
        }
    },
}
</script>

<style>

</style>