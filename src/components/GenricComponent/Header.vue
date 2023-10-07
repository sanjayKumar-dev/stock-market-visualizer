<template>
        <v-app-bar color="warn" density="compact">
            <template v-slot:prepend>
                <v-app-bar-nav-icon></v-app-bar-nav-icon>
            </template>

            <v-app-bar-title>Stock Market Visualizer</v-app-bar-title>

            <template v-slot:append>
                <v-btn icon> <v-icon> mdi-heart </v-icon> </v-btn>
                <v-btn icon>
                    <v-icon>mdi-magnify</v-icon>
                </v-btn>
                <v-btn icon="mdi-dots-vertical"></v-btn>
            </template>
        </v-app-bar>
</template>

<script>
export default {
    name: 'Header'
}
</script>