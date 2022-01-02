<template>
  <v-container>
    <section id="hero">
      <v-card elevation="3" rounded>
        <v-card-title>
          <h1>Calculate your ideal strength</h1>
        </v-card-title>

        <v-card-text>
          <v-form v-model="valid" ref="form" lazy-validation>
            <div class="card-row"><span>I can</span></div>
            <div class="card-row">
              <v-input>
                <v-select
                  v-model="form.exerciseId"
                  :items="exercises"
                  :rules="[rules.required]"
                  item-text="name"
                  item-value="id"
                  label="exercise"
                  outlined
                  dense
                  single-line
                ></v-select>
              </v-input>
            </div>
            <div class="card-row" id="weight-selector">
              <div>
                <v-input>
                  <v-text-field
                    v-model="form.weight"
                    :rules="[rules.required, rules.boundWeight]"
                    label="weight"
                    type="number"
                    outlined
                    dense
                  ></v-text-field>
                </v-input>
              </div>
              <div>
                <v-input>
                  <v-select
                    v-model="form.units"
                    :items="units"
                    :rules="[rules.required]"
                    label="units"
                    :value="units[0]"
                    outlined
                    dense
                  ></v-select>
                </v-input>
              </div>
            </div>
            <div class="card-row" id="reps-selector">
              <span>for</span>
              <v-input>
                <v-text-field
                  v-model="form.reps"
                  :rules="[rules.required, rules.boundReps]"
                  label="reps"
                  type="number"
                  outlined
                  dense
                ></v-text-field>
              </v-input>
              <span>rep{{ form.reps !== 1 ? 's' : '' }}.</span>
            </div>
          </v-form>
        </v-card-text>

        <v-card-actions>
          <div id="action-buttons">
            <v-btn color="primary" @click="reset" large>Reset</v-btn>
            <v-btn color="primary" @click="calculate" large>Calculate</v-btn>
          </div>
        </v-card-actions>
      </v-card>
    </section>

    <section v-if="showResults" id="results">
      <v-card elevation="3" rounded>
        <v-card-title>
          <h1>Your ideal one-rep maxes</h1>
        </v-card-title>

        <v-card-text>
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Exercise</th>
                  <th class="text-left">One-rep max</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in results" :key="item.id">
                  <td>{{ item.name }}</td>
                  <td>{{ item.oneRepMax }} {{ form.units }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
      </v-card>

      <v-card elevation="3" rounded>
        <v-card-title>
          <h1>Your ideal weight ranges</h1>
        </v-card-title>

        <v-card-text>
          <v-form>
            <v-input>
              <v-text-field
                v-model="form.outputReps"
                :rules="[rules.boundReps]"
                label="reps"
                type="number"
                outlined
                dense
              ></v-text-field>
            </v-input>
          </v-form>

          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Exercise</th>
                  <th class="text-left">Range</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in results" :key="item.id">
                  <td>{{ item.name }}</td>
                  <td>{{ item.lowRange }} - {{ item.highRange }} {{ form.units }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
      </v-card>

      <v-card elevation="3" rounded>
        <v-card-title>
          <h1>Your ideal set examples</h1>
        </v-card-title>

        <v-card-text>
          <v-form>
            <v-input>
              <v-select
                v-model="form.outputExerciseId"
                :items="exercises"
                :rules="[rules.required]"
                item-text="name"
                item-value="id"
                label="exercise"
                outlined
                dense
                single-line
              ></v-select>
            </v-input>
          </v-form>

          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">Goal</th>
                  <th class="text-left">Sets</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="item in setGroupings" :key="item.name">
                  <td>{{ item.name }}</td>
                  <td>
                    {{ item.sets }}x{{ item.reps }} -
                    {{
                      parseInt(
                        item.multiplier *
                          exercises.find((ex) => form.outputExerciseId === ex.id).multiplier *
                          squatOneRepMax
                      )
                    }}
                    {{ form.units }}
                  </td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
      </v-card>
    </section>
  </v-container>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      deviation: 0.03,
      valid: false,
      showResults: false,
      form: {
        exerciseId: null,
        weight: null,
        units: 'lbs',
        reps: 1,
        outputReps: 1,
        outputExerciseId: 0,
      },
      rules: {
        required: (v) => v !== null || 'This field is required',
        boundReps: (v) => (v <= 12 && v >= 1) || 'Choose a number between 1 - 12',
        boundWeight: (v) => (v > 0 && v <= 1200) || `Choose a number between 1 - 1200 ${this.form.units}`,
      },
      exercises: [
        { id: 0, name: 'squat', multiplier: 1 },
        { id: 1, name: 'deadlift', multiplier: 1.2 },
        { id: 2, name: 'power clean', multiplier: 0.68 },
        { id: 3, name: 'bench press', multiplier: 0.75 },
        { id: 4, name: 'barbell row', multiplier: 0.6 },
        { id: 5, name: 'overhead press', multiplier: 0.45 },
      ],
      setGroupings: [
        { name: 'strength', sets: 5, reps: 5, multiplier: 0.81 },
        { name: 'hypertrophy', sets: 3, reps: 10, multiplier: 0.7 },
        { name: 'endurance', sets: 4, reps: 15, multiplier: 0.55 },
      ],
      repRatios: [0, 1, 0.97, 0.94, 0.92, 0.89, 0.86, 0.83, 0.81, 0.78, 0.76, 0.73, 0.7],
      units: ['lbs', 'kgs'],
    };
  },
  computed: {
    squatOneRepMax() {
      return (
        parseFloat(this.form.weight) /
        this.exercises.find((exercise) => exercise.id === this.form.exerciseId)?.multiplier /
        this.repRatios[parseInt(this.form.reps)]
      );
    },
    results() {
      let base = this.squatOneRepMax;
      let repBase = base * this.repRatios[Math.min(Math.max(this.form.outputReps, 1), 12)];

      return this.exercises.map((exercise) => {
        exercise.oneRepMax = parseInt(base * exercise.multiplier);
        exercise.lowRange =
          parseInt(repBase * exercise.multiplier) - parseInt(repBase * exercise.multiplier * this.deviation);
        exercise.highRange =
          parseInt(repBase * exercise.multiplier) + parseInt(repBase * exercise.multiplier * this.deviation);
        return exercise;
      });
    },
  },
  methods: {
    reset() {
      this.form.exerciseId = null;
      this.form.weight = null;
      this.form.reps = 1;
      this.$refs.form.resetValidation();
      this.valid = false;
      this.showResults = false;
    },
    calculate() {
      this.$refs.form.validate();

      if (this.form.exerciseId === null || !this.form.weight) {
        this.showResults = false;
      } else {
        this.showResults = true;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
* {
  word-break: normal;
}

section {
  display: grid;
  align-items: center;
  justify-content: stretch;

  &#hero {
    min-height: 100vh;
  }
}

.card-row {
  font-size: 2em;
  display: flex;

  span {
    padding: 0.25em 0 0.5em 0;
  }

  &#weight-selector {
    & > div:nth-of-type(1) {
      width: 95%;

      .v-text-field {
        border-top-right-radius: 0;
        border-bottom-right-radius: 0;
      }
    }

    & > div:nth-of-type(2) {
      .v-text-field {
        border-top-left-radius: 0;
        border-bottom-left-radius: 0;
      }
    }
  }

  &#reps-selector {
    .v-input {
      padding: 0 0.5em;
    }
  }
}

#results {
  padding-bottom: 10em;

  .v-card {
    margin-bottom: 5em;
  }
}
</style>
