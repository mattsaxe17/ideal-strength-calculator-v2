<template>
  <v-container id="hero">
    <v-card elevation="3" rounded>
      <v-card-title>
        <h1>Calculate your ideal strength</h1>
      </v-card-title>

      <v-card-text>
        <div class="card-row"><span>I can</span></div>
        <div class="card-row">
          <v-input>
            <v-select :items="exercises" item-text="name" label="exercise" outlined dense single-line></v-select>
          </v-input>
        </div>
        <div class="card-row" id="weight-selector">
          <div>
            <v-input>
              <v-text-field label="weight" type="number" outlined dense></v-text-field>
            </v-input>
          </div>
          <div>
            <v-input>
              <v-select :items="units" label="units" :value="units[0]" outlined dense></v-select>
            </v-input>
          </div>
        </div>
        <div class="card-row" id="reps-selector">
          <span>for</span>
          <v-input>
            <v-text-field label="reps" type="number" outlined dense></v-text-field>
          </v-input>
          <span>reps.</span>
        </div>
      </v-card-text>

      <v-card-actions>
        <div id="action-buttons">
          <v-btn color="primary" large>Reset</v-btn>
          <v-btn color="primary" large>Calculate</v-btn>
        </div>
      </v-card-actions>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      deviation: 0.03,
      exercises: [
        { name: 'squat', multiplier: 1 },
        { name: 'deadlift', multiplier: 1.2 },
        { name: 'power clean', multiplier: 0.68 },
        { name: 'bench press', multiplier: 0.75 },
        { name: 'barbell row', multiplier: 0.6 },
        { name: 'overhead press', multiplier: 0.45 },
      ],
      setGroupings: [
        { name: 'strength', sets: 5, reps: 5, multiplier: 0.81 },
        { name: 'hypertrophy', sets: 3, reps: 10, multiplier: 0.7 },
        { name: 'endurance', sets: 4, reps: 15, multiplier: 0.55 },
      ],
      repRatios: [0, 1, 0.97, 0.94, 0.92, 0.89, 0.86, 0.83, 0.81, 0.78, 0.76],
      units: ['lbs', 'kgs'],
    }
  },
}
</script>

<style lang="scss" scoped>
* {
  word-break: normal;
}

#hero {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
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
</style>
