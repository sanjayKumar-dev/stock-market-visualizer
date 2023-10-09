<template>
    <div>
        <v-snackbar :timeout="2000" v-model="snackbar.show" :color="snackBarData.type" bottom>
            {{ snackBarData.mgs }}
            <!-- <v-btn  @click="snackbar.show = false">
                Close
            </v-btn> -->
        </v-snackbar>
    </div>
</template>
<script>
export default {
    props: {
        // message: String,
        snackBarData: Object
    },
    data() {
        return {
            snackbar: {
                show: false,
                text: 'Testing'
            }
        }
    },
    watch: {
        snackBarData(newVal, oldVal) {
            // Handle the change in the 'message' prop
            this.snackbar.show = newVal.showSnackBar
            console.log(`Message changed from ${oldVal} to ${newVal}`);
        }
    }
}

</script>
  