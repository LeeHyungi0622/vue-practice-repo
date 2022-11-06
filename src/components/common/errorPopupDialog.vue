<template>
    <v-dialog
        class="d-flex justify-center align-center"
        v-model="dialog"
        width="25%"
        height="70%"
    >   
        <v-card
            class="d-flex align-around justify-center"
            width="100%"
            height=250
        >
            <v-container class="d-flex flex-column justify-center align-center">
                <v-row class="d-flex align-center">
                    <h2>Error dialog</h2> 
                </v-row>
                <v-row class="d-flex align-center">
                    {{ errorMessage }}
                </v-row>
                <v-row class="d-flex justify-center align-center">
                    <v-btn color="primary" width="40%" @click="close">닫기</v-btn>
                </v-row>
            </v-container>
        </v-card>
    </v-dialog>
</template>

<script>
    export default {
        name: 'ErrorPopupDialog',
        props: {
            errorMessage: {
                type: String
            }
        },
        data() {
            return {
                dialog: true
            }
        },
        methods: {
            close: function() {
                console.log("close popup dialog");
                this.$emit("closeErrorDialog");
            }
        }
    }
</script>

<style>

</style>