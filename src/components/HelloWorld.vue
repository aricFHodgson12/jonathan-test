<template>
  <v-stepper v-model="e1">
    <v-stepper-header class="step-header">
      <v-stepper-step
        :complete="e1 > 1"
        step="1"
      >
        Configuration
      </v-stepper-step>

      <v-divider></v-divider>

      <v-stepper-step
        :complete="e1 > 2"
        step="2"
      >
        Summary
      </v-stepper-step>

    </v-stepper-header>

    <v-stepper-items>
      <v-stepper-content step="1" class="step-content">
        <v-simple-table>
          <template v-slot:default>
            <thead>
              <tr>
                <th class="text-left">
                  Add plan name
                </th>
                <th class="text-left">
                  & Amount
                </th>
                <th class="text-left">
                  Job Owner
                </th>
                <th class="text-left">
                </th>
              </tr>
            </thead>
            <tbody>
              <tr
                v-for="(item, index) in rows"
                :key="index"
              >
                <td class="pt-4 pb-4">
                  <v-row>
                    <v-col>
                      <v-text-field
                        v-model="item.name"
                        hide-details
                        label="Product name"
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                  </v-row>
                </td>
                <td>
                  <v-text-field
                    v-model="item.amount"
                    hide-details
                    label="Dollar amount"
                    outlined
                    dense
                  ></v-text-field>
                </td>
                <td>
                  <v-select
                    v-model="item.jobOwner"
                    hide-details
                    :items="items"
                    :item-text="'name'"
                    :item-value="'name'"
                    outlined
                    label="Select job owner"
                    dense
                  ></v-select>
                </td>
                <td>
                  <v-icon
                  @click="removeRow(item)"
                  color="darken-2"
                  >
                    mdi-delete
                  </v-icon>
              </td>
              </tr>
            </tbody>
          </template>
        </v-simple-table>

        <v-divider></v-divider>
        <div class="d-flex justify-space-between pt-md-8" >
          <v-btn text color="primary" @click="addNewRow()">
            + ADD ROW
          </v-btn>

          <v-btn
            color="primary"
            @click="handleMoveNextStep()"
            rounded
          >
            NEXT
          </v-btn>

        </div>

      </v-stepper-content>

      <v-stepper-content step="2">
        <v-row>
          <v-col cols="6">
            <v-simple-table>
              <template v-slot:default>
                <thead>
                  <tr>
                    <th class="text-left">
                      & Sum
                    </th>
                    <th class="text-left">
                      Job Owner
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td class="pt-4 pb-4">
                      {{ sum }}
                    </td>
                    <td>
                      {{ jobOwner }}
                    </td>
                  </tr>
                </tbody>
              </template>
            </v-simple-table>
            <v-divider></v-divider>
            <div class="d-flex justify-space-between pt-md-8" >
              <v-btn
                text
                color="primary"
                @click="e1 = 1"
              >
              <v-icon
                color="darken-2"
                >
                mdi-chevron-left
                </v-icon>
                Back
              </v-btn>
            </div>
          </v-col>
        </v-row>
      </v-stepper-content>

    </v-stepper-items>
  </v-stepper>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      e1: 1,
      sum: 'dd',
      jobOwner: '',
      rows: [
        {
          name: '',
          amount: 0,
          jobOwner: '',
        },
      ],
      items: [],
    };
  },
  mounted() {
    const that = this;
    axios
      .get('https://demo-api.bettercommissions.com/interview-data/users')
      .then((response) => {
        that.items = response.data.users;
      })
      .catch((error) => {
        console.log(error);
      });
  },
  methods: {
    handleMoveNextStep() {
      this.e1 = 2;
      const sum = this.rows.reduce(
        (accumulator, object) => accumulator + parseFloat(object.amount),
        0,
      );
      this.sum = sum;

      const jobOwners = this.rows.reduce(
        (accumulator, object) => `${accumulator}, ${object.jobOwner}`,
        '',
      );
      this.jobOwner = jobOwners.slice(1);
    },
    removeRow(row) {
      this.rows.splice(this.rows.indexOf(row), 1);
    },
    addNewRow() {
      this.rows.push(
        {
          name: '',
          amount: 0,
          jobOwner: '',
        },
      );
    },
  },
};
</script>

<style scoped>
  .v-sheet {
    box-shadow: none !important;
    padding: 10px;
  }
  .step-header {
    box-shadow: 0px 3px 1px -2px rgba(0, 0, 0, 0.2),
                0px 2px 2px rgba(0, 0, 0, 0.14), 0px 1px 5px rgba(0, 0, 0, 0.12);
  }

  .step-content {
    background: transparent;
    box-shadow: none;
  }

  .v-btn--is-elevated {
    box-shadow: none !important;
  }

  .v-stepper__content {
    padding: 24px 0px;
  }

  .v-text-field{
    width: 234px;
  }

</style>
