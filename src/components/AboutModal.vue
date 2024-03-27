<template>
    <v-dialog v-model="this.$props.showInfoDialog" style="max-width: 675px; min-width: 575px" persistent>
        <v-card>
            <v-card-item>
                <v-card-title class="d-flex">
                    About This Library <v-spacer /><v-btn icon="mdi-close" class="cursor-hover" density="compact" flat
                        :ripple="false" @click="$emit('closeInfoDialog')">
                    </v-btn>
                </v-card-title>
                <v-card-subtitle><span class="text-caption">Developed by Kyle Massey for Tableau Conference 2024</span>
                </v-card-subtitle>
            </v-card-item>
            <v-card-text class="text-body-2">
                <div>
                    This is a mock up of a theoretical library portal using faux data sourced from ChatGPT and random
                    number generators. It's meant to showcase an abstract/silly use case for the <a
                        href="https://help.tableau.com/current/api/embedding_api/en-us/index.html"
                        target="_blank">Tableau Embedding API</a>. This highlights how you can truly integrate
                    data/analytics
                    powered by Tableau into any sort of web application you can imagine!
                </div>
                <div class="pt-4">
                    Please feel free to borrow, steal, adapt and modify any of the
                    source code, which is available
                    <a href="https://github.com/kjmassey/vue-tc24-embedding" target="_blank">here</a>.
                </div>

                <div class="pt-4">
                    If you have questions about this example, my TC24
                    presentation or just want to talk nerdy, please reach out:
                </div>

                <div class="pt-4">
                    <v-icon icon="mdi-twitter" color="#26a7de" class="mr-4"></v-icon>

                    <a href="https://twitter.com/UpInYourVizness" target="_blank">@UpInYourVizness</a>
                </div>
                <div>
                    <v-icon icon="mdi-github" class="mr-4"></v-icon>

                    <a href="https://github.com/kjmassey" target="_blank">kjmassey</a>
                </div>

                <div>
                    <v-icon icon="mdi-slack" color="purple" class="mr-4"></v-icon>

                    <a href="https://tableau-datadev.slack.com/team/U02TY283SEM" target="_blank">Kyle Massey</a>
                </div>

                <div>
                    <v-icon icon="mdi-web" color="green" class="mr-4"></v-icon>

                    <a href="https://kylejmassey.com" target="_blank">kylejmassey.com</a>
                </div>
            </v-card-text>
            <v-card-text class="text-subtitle-2">
                <em>Special thanks to Autumn Battani (<a href="http://x.com/aabattani" target="_blank">@aabattani</a>)
                    for her guidance and inspiration with many of the viz-related elements shown here!</em>
            </v-card-text>
            <v-card-actions>
                <div class="w-100 pt-8 pb-8">
                    <div class="text-caption pb-2 px-4 pb-6"><strong>Tools used:</strong></div>
                    <div class="d-flex justify-space-evenly align-center cursor-hover w-100 px-6 pb-4">
                        <v-tooltip text="Tableau/Tableau Embedding API" location="bottom">
                            <template v-slot:activator="{ props }">
                                <v-img src="../../public/tableau.png" :max-width="36" :max-height="36" cover
                                    v-bind="props" @click="openURL('http://tableau.com/developer')">
                                </v-img>
                            </template>
                        </v-tooltip>

                        <v-tooltip text="Vue.js" location="bottom">
                            <template v-slot:activator="{ props }">
                                <v-img src="../../public/vue.png" :max-width="36" :max-height="36" cover v-bind="props"
                                    @click="openURL('https://vuejs.org')">
                                </v-img>
                            </template>
                        </v-tooltip>

                        <v-tooltip text="Vuetify" location="bottom">
                            <template v-slot:activator="{ props }">
                                <v-img src="../../public/vuetify.png" :max-width="36" :max-height="36" cover
                                    v-bind="props" @click="openURL('https://vuetifyjs.com/en/')">
                                </v-img>
                            </template>
                        </v-tooltip>
                    </div>
                </div>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    props: { showInfoDialog: { type: Boolean, default: () => true } },
    data: () => ({
    })
}
</script>

<style>
a {
    text-decoration: none;
    color: blue
}
</style>