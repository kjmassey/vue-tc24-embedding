<template>
    <v-card>
        <v-card-item>
            <v-card-title>
                Select A State
            </v-card-title>
            <v-card-subtitle class="mt-0">Drag You Selection</v-card-subtitle>
        </v-card-item>

        <v-card-text>
            <div class="d-flex">
                <div style="max-width: 50%;">
                    <!-- <v-chip-group column multiple v-model="selectedStates">
                        <v-chip class="state-pill" v-for="state in stateChips" :key="state" variant="elevated"
                            density="comfortable" label :value="state">{{ state
                            }}</v-chip>
                    </v-chip-group> -->
                    <v-chip-group column>
                        <draggable class="list-group d-flex flex-wrap"
                            :list="selectedStates.length < 1 ? origChips : stateChips"
                            :group="{ name: 'states', put: false, }" :itemKey="item">
                            <template #item="{ element }">
                                <v-chip class="state-pill" variant="elevated" density="comfortable">
                                    <v-icon icon="mdi-earth" size="xs" start></v-icon> {{ element }}</v-chip>

                            </template>
                        </draggable>
                    </v-chip-group>
                </div>
                <div style="background-color: lightgray; border-radius: 10px;" class="flex-grow-1 px-2">
                    <div style="display: float-right; position: relative; top: 4px; float: right; cursor: pointer;"
                        v-if="selectedStates.length > 0" @click="clearFilters()">
                        <v-icon icon="mdi-close"></v-icon>
                    </div>
                    <v-chip-group column>
                        <draggable class="list-group w-100" :list="selectedStates" group="states" :itemKey="item"
                            style="min-height: 125px;">
                            <template #item="{ element }">
                                <v-chip class="state-pill" variant="elevated" density="comfortable">
                                    <v-icon icon="mdi-earth" size="xs" start></v-icon>
                                    {{
                                element
                            }}</v-chip>

                            </template>
                        </draggable>
                    </v-chip-group>
                </div>
            </div>
        </v-card-text>
    </v-card>
    {{ origChips }}
</template>
<script>
import { reactive } from 'vue'
import draggable from 'vuedraggable'
import _ from 'lodash'

export default {
    props: { stateNames: { type: Array, default: () => [] } },
    components: {
        draggable
    },
    data: () => ({
        selectedStates: [],
        allChips: []
    }),
    computed: {
        stateChips() {
            let newArr = []
            let propArr = reactive(this.$props.stateNames)

            propArr.forEach(e => newArr.push(e))

            return newArr

        },
        origChips() {
            return _.cloneDeep(this.stateChips)
        }
    },
    methods: {
        clearFilters() {
            this.selectedStates = []
            this.$emit('clearVizFilters')


        }
    },
    watch: {
        selectedStates: {
            handler(newValue) {
                if (this.selectedStates.length > 0) {
                    console.log('GOT CHANGE: ', newValue)
                    this.$emit('stateSelectionChange', newValue)
                }
            },
            immediate: true,
            deep: true
        }
    }
}
</script>
<style>
.state-pill {
    background-color: #00B180 !important;
}
</style>