<template>
  <div>
    <div>
      <h1 class="mb-5 mt-3">Dashboard Covid</h1>

      <div style="margin-left:13px" class="row mb-5">
        <div class="col">
          <div class="card shadow-card" style="width: 17rem;margin-top: 38px;">
            <a href="#confirmados" class="min-card min-card-color-confirm">
              <i class="material-icons icon">&#xe85d;</i>
            </a>
            <div class="card-body msg-card row">
              <div class="col-12">Confirmados</div>
              <div class="col-12">
                <h6 class="card-title" style="font-size: 22px;">
                  {{ totalConfirmado }}
                </h6>
                <span
                  style="display:flex;font-size: 13px;color: #7d7b7b;border-top: 1px solid #ddd9d9;"
                  ><strong style="margin-top:10px;margin-left: 15px;">
                    <i class="material-icons icon-calendar">&#xe8df;</i
                    >{{ infoData }}</strong
                  ></span
                >
              </div>
            </div>
          </div>
        </div>

        <div class="col">
          <div class="card shadow-card" style="width: 17rem;margin-top: 38px;">
            <a href="#ativos" class="min-card min-card-color-ativo">
              <i class="material-icons icon">&#xe85e;</i>
            </a>
            <div class="card-body msg-card row">
              <div class="col-12">Ativos</div>
              <div class="col-12">
                <h6 class="card-title" style="font-size: 22px;">
                  {{ totalAtivo }}
                </h6>
                <span
                  style="display:flex;font-size: 13px;color: #7d7b7b;border-top: 1px solid #ddd9d9;"
                  ><strong style="margin-top:10px;margin-left: 15px;">
                    <i class="material-icons icon-calendar">&#xe8df;</i
                    >{{ infoData }}</strong
                  ></span
                >
              </div>
            </div>
          </div>
        </div>

        <div class="col">
          <div class="card shadow-card" style="width: 17rem;margin-top: 38px;">
            <a href="#recuperados" class="min-card min-card-color-recuperado">
              <i class="material-icons icon">&#xe862;</i>
            </a>
            <div class="card-body msg-card row">
              <div class="col-12">Recuperados</div>
              <div class="col-12">
                <h6 class="card-title" style="font-size: 22px;">
                  {{ totalRecuperado }}
                </h6>
                <span
                  style="display:flex;font-size: 13px;color: #7d7b7b;border-top: 1px solid #ddd9d9;"
                  ><strong style="margin-top:10px;margin-left: 15px;">
                    <i class="material-icons icon-calendar">&#xe8df;</i
                    >{{ infoData }}</strong
                  ></span
                >
              </div>
            </div>
          </div>
        </div>

        <div class="col">
          <div class="card shadow-card" style="width: 17rem;margin-top: 38px;">
            <a href="#mortos" class="min-card min-card-color-mortos">
              <i class="material-icons icon">&#xe85f;</i>
            </a>
            <div class="card-body msg-card row">
              <div class="col-12">Mortos</div>
              <div class="col-12">
                <h6 class="card-title" style="font-size: 22px;">
                  {{ totalMortos }}
                </h6>
                <span
                  style="display:flex;font-size: 13px;color: #7d7b7b;border-top: 1px solid #ddd9d9;"
                  ><strong style="margin-top:10px;margin-left: 15px;">
                    <i class="material-icons icon-calendar">&#xe8df;</i
                    >{{ infoData }}</strong
                  ></span
                >
              </div>
            </div>
          </div>
        </div>
      </div>

      <div class="m-5">
        <section class="row">
          <div
            id="confirmados"
            class="col-md-6"
            v-if="arrConfirmados.length > 0"
          >
            <h2 class="card-color min-card-color-confirm">Confirmados</h2>
            <LineChart
              style="background-color: white;border: 1px solid #bdbbbb;margin-top:10px;"
              :chartDate="arrConfirmados"
              :options="chartOptions"
              :chartColors="confirmadosChartColors"
              label="Confirmados"
            ></LineChart>
          </div>
          <div id="ativos" class="col-md-6" v-if="arrAtivos.length > 0">
            <h2 class="card-color min-card-color-ativo">Ativos</h2>
            <LineChart
              style="background-color: white;border: 1px solid #bdbbbb;margin-top:10px;"
              :chartDate="arrAtivos"
              :options="chartOptions"
              :chartColors="ativosChartColors"
              label="Ativos"
            ></LineChart>
          </div>
        </section>

        <section class="row mt-5">
          <div
            id="recuperados"
            class="col-md-6"
            v-if="arrRecuperados.length > 0"
          >
            <h2 class="card-color min-card-color-recuperado">Recuperados</h2>
            <LineChart
              style="background-color: white;border: 1px solid #bdbbbb;margin-top:10px;"
              :chartDate="arrRecuperados"
              :options="chartOptions"
              :chartColors="recuperadosChartColors"
              label="Recuperados"
            ></LineChart>
          </div>

          <div id="mortos" class="col-md-6" v-if="arrMortos.length > 0">
            <h2 class="card-color min-card-color-mortos">Mortos</h2>
            <LineChart
              style="background-color: white;border: 1px solid #bdbbbb;margin-top:10px;"
              :chartDate="arrMortos"
              :options="chartOptions"
              :chartColors="mortosChartColors"
              label="Mortos"
            ></LineChart>
          </div>
        </section>
      </div>
      <div v-if="loading" class="d-flex justify-content-center">
        <div class="spinner-border" role="status">
          <span class="sr-only">Loading...</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import LineChart from "../components/LineChart";
export default {
  name: "Home",
  components: {
    LineChart
  },
  data: () => ({
    loading: false,
    totalConfirmado: 0,
    totalAtivo: 0,
    totalRecuperado: 0,
    totalMortos: 0,
    infoData: null,
    confirmadosChartColors: {
      borderColor: "#a8a8a8",
      backgroundColor: "#cecece"
    },
    ativosChartColors: {
      borderColor: "#a8a8a8",
      backgroundColor: "#cecece"
    },
    recuperadosChartColors: {
      borderColor: "#a8a8a8",
      backgroundColor: "#cecece"
    },
    mortosChartColors: {
      borderColor: "#a8a8a8",
      backgroundColor: "#cecece"
    },
    arrConfirmados: [],
    arrAtivos: [],
    arrRecuperados: [],
    arrMortos: [],
    chartOptions: {
      responsive: true,
      maintainAspectRatio: false,
      legend: {
        labels: {
          // This more specific font property overrides the global property
          fontColor: "#858585"
        }
      }
    }
  }),
  async created() {},
  async mounted() {
    this.loading = true;
    await fetch("https://api.covid19api.com/total/dayone/country/brazil")
      .then(response => response.json())
      .then(valor => {
        valor
          ? ((this.loading = false), (this.lista = valor))
          : (this.loading = false);
      })
      .catch(function(error) {
        console.log(error);
        this.loading = false;
      });

    this.lista.forEach(element => {
      let data = element.Date;
      let convertData = data
        .replace("T00:00:00Z", "")
        .split("-")
        .reverse()
        .join("/");

      this.arrConfirmados.push({
        date: convertData,
        total: element.Confirmed
      });

      this.arrAtivos.push({
        date: convertData,
        total: element.Active
      });

      this.arrRecuperados.push({
        date: convertData,
        total: element.Recovered
      });

      this.arrMortos.push({
        date: convertData,
        total: element.Deaths
      });
    });

    const confirmados = this.arrConfirmados.pop();
    this.totalConfirmado = confirmados.total;
    //Ultima data retornada esta em um unico elemento no array
    this.infoData = confirmados.date;

    const ativos = this.arrAtivos.pop();
    this.totalAtivo = ativos.total;

    const recuperados = this.arrRecuperados.pop();
    this.totalRecuperado = recuperados.total;

    const mortos = this.arrMortos.pop();
    this.totalMortos = mortos.total;
  }
};
</script>

<style>
* {
  font-family: Calibri;
}
.min-card {
  display: flex;
  justify-content: center;
  width: 70px;
  height: 70px;
  position: absolute;
  margin-top: -32px;
  box-shadow: 1px 4px 2px #efe4c7;
  margin-left: 8px;
  z-index: 1;
  border-radius: 4px;
}

a:link
{
text-decoration:none;
}

html {
  scroll-behavior: smooth;
}

.icon-calendar {
  position: absolute;
  margin-left: -20px;
  margin-top: 2px;
  font-size: 19px;
}

.min-card-color-ativo {
  background-color: #ffcf48;
}

.min-card-color-confirm {
  background-color: #63c9e7;
}

.min-card-color-recuperado {
  background-color: #61e78a;
}

.shadow-card {
  width: 17rem;
  margin-top: 38px;
  box-shadow: 1px 1px 1px #959595;
}
.min-card-color-mortos {
  background-color: tomato;
}

.card-color {
  border-radius: 4px;
  box-shadow: 1px 1px 1px #959595;
  width: 245px;
  color: white;
  display: initial;
  padding-right: 15px;
  padding-left: 15px;
}

body {
  background-color: #eee;
}
h1,
h2 {
  color: #858585;
}
.grid {
  display: grid;
}

.grid-template-columns-2 {
  grid-template-columns: 1fr 1fr;
}

.msg-card {
  display: flex;
  justify-content: flex-end;
}
.icon {
  margin-top: 33%;
  color: white;
}
</style>