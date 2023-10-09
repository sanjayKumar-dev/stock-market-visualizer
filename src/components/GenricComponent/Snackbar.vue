<template>
    <div>
        <v-snackbar :timeout="2000" v-model="snackbar.show" :color="snackBarData.type" bottom>
            {{ snackBarData.mgs }}
        </v-snackbar>
    </div>
</template>
<script>
export default {
    props: {
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
        snackBarData(newVal) {
            this.snackbar.show = newVal.showSnackBar
        }
    }
}

</script>
  