<template>
  <div>
    <v-app>
    <v-data-table
        :server-items-length="options.totalItems"
        :options.sync="options"
        :items="rows"
        :headers="columns">
        <template slot="headers" slot-scope="props">
          <tr :active="props.selected">
            <th v-for="column in props.headers">
              {{ column.value }}
            </th>
          </tr>
        </template>

      <template slot="items" slot-scope="props">
          <tr>
            <td v-for="cell in props.item.row">
              <v-edit-dialog lazy>
                {{ cell.value }}
                <v-text-field
                  :value="cell.value"
                  single-line
                  counter>
                </v-text-field>
              </v-edit-dialog>
            </td>
          </tr>
        </template>
        <template v-slot:top>
          <v-switch v-model="singleSelect" label="Single select" class="pa-3"></v-switch>
        </template>
    </v-data-table>
    </v-app>
  </div>
</template>

<script>
export default {
  data: () => ({
    options: {
      page: 1,
      rowsPerPage: 10,
      totalItems: 0
    },

    selected: []
  }),

  computed: {
    columns: {
      get () {
        return this.$store.state.columns
      }
    },

    rows: {
      get () {
        return this.$store.state.rows
      }
    }
  },

  methods: {
    async getRowsHandler () {
      try {
        const {total} = await this.$store.dispatch('getRows', {
          tableIdentifier: this.$route.params.tableIdentifier,
          page: this.options.page,
          size: this.options.rowsPerPage
        })

        this.options.totalItems = total
      } catch (error) {
        // Error
      }
    }
  }
}
</script>