<template>
  <v-container>
    <v-row>
      <div style="background-color: lightgray;">
        <v-chip-group multiple v-model="selection" color="#666">
          <v-chip value="Chip1" draggable v-if="showChip1">Chip 1</v-chip>
          <v-chip value="Chip2" draggable>Chip 2</v-chip>
          <v-chip value="Chip3" draggable>Chip 3</v-chip>
        </v-chip-group>
      </div>
    </v-row>
    <v-row>
      <div style="background-color: lightcyan; min-height: 125px; min-width: 80px; border: 1px solid red;">
        <v-chip-group multiple v-model="selection2">

        </v-chip-group>
      </div>
    </v-row>
    <v-row>
      <div class="text-h1">{{ selection }}</div>
    </v-row>
    <v-row>
      <draggable v-model="myArray" tag="transition-group" item-key="id">
        <template #item="{ element }">
          <v-chip> {{ element.name }} </v-chip>
        </template>
      </draggable>
    </v-row>
  </v-container>
</template>

<script>
import draggable from 'vuedraggable'

export default {
  data: () => ({
    testArray: [],
    selection: [],
    selection2: [],
    showChip1: true,
    drag: false,
    myArray: [
      {
        name: 'Kyle'
      },
      {
        name: 'Dan'
      }
    ]
  }),
  components: {
    draggable
  }
}
</script>
