<template>
  <div class="container mt-4">
    <div class="row justify-content-center">
      <div class="col-lg-8">
        <p>
          Você está vendo:<br />
          <span v-if="sheet" class="small">
            {{ sheet.title }} | {{ sheet.place }}</span
          >
        </p>
      </div>
    </div>
    <div class="row justify-content-center">
      <div class="col-lg-8">
        <div class="alert alert-info">
          <p class="font-weight-bold">Instruções:</p>
          {{ sheet.instructions }}
        </div>
      </div>
    </div>
    <div class="row justify-content-center">
      <div class="col-lg-8">
        <!-- Spínner -->
        <div class="text-center">
          <div v-if="$fetchState.pending">
            <b-spinner
              label="Carregando treinos..."
              variant="danger"
              type="grow"
            ></b-spinner>
          </div>
        </div>

        <div v-for="serie in series">
          <div class="card mt-4">
            <div class="card-body">
              <div class="card-title">
                <h5 class="text-dark">
                  {{ serie.title }}
                </h5>
                <h6 class="card-subtitle mb-2 text-muted">
                  {{ serie.instructions }}<br />
                  Repetições: {{ serie.repetitions }}
                </h6>
              </div>
              <ul
                v-for="exercise in serie.exercises"
                :key="exercise.id"
                class="text-dark"
              >
                <li>
                  <p class="mb-0 text-uppercase font-weight-bold">
                    {{ exercise.title }}
                  </p>
                  <p v-if="exercise.instructions" class="m-0">
                    <strong>Obs:</strong> {{ exercise.instructions }}
                  </p>
                  <p v-if="exercise.repetitions" class="m-0">
                    <strong>Repetições:</strong> {{ exercise.repetitions }}
                  </p>
                </li>
              </ul>
            </div>
          </div>
        </div>
        <NuxtLink
          class="btn btn-secondary w-100 my-4 py-3"
          :to="'/level/' + sheet.level.name"
        >
          👈 Voltar
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "SeriesPage",
  transition: "fade",
  data() {
    return {
      response: [],
      sheet: {
        id: null,
        title: null,
        place: null,
        week: null,
        instructions: null,
        level: {
          name: null,
        },
      },
      series: [],
      hasSeries: false,
      hasSheet: false,
    };
  },
  async fetch() {
    let sheetUri = `/sheets/${this.$route.params.sheet}`;
    let seriesUri = `/sheets/${this.$route.params.sheet}/series`;

    let sheet = await this.$axios.$get(sheetUri);
    this.sheet = sheet.sheet;

    let series = await this.$axios.$get(seriesUri);
    this.series = series.series;
  },
};
</script>

<style></style>
