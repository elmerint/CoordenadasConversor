<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated class="bg-deep-purple-10">
      <q-toolbar>
        <q-btn flat @click="drawer = !drawer" round dense icon="menu" />

        <q-toolbar-title>Obras Hidraúlicas 2022</q-toolbar-title>
      </q-toolbar>
    </q-header>
    <q-drawer v-model="drawer" show-if-above :width="200" :breakpoint="500">
      <q-scroll-area class="fit">
        <q-list padding class="menu-list">
          <h6 class="text-center">Integrantes</h6>
          <q-item clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="person" />
            </q-item-section>

            <q-item-section>
              Fátima Abigail Reina Guerrero, RG18003
            </q-item-section>
          </q-item>

          <q-item clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="person" />
            </q-item-section>

            <q-item-section>
              Josue Daniel Santos Palacios, SP15016
            </q-item-section>
          </q-item>

          <q-item clickable v-ripple>
            <q-item-section avatar>
              <q-icon name="person" />
            </q-item-section>

            <q-item-section>
              Ronald Oswaldo Hernández Arriaza, HA17001
            </q-item-section>
          </q-item>
        </q-list>
      </q-scroll-area>
    </q-drawer>
    <h3 class="text-center text-weight-bold text-deep-purple-10">
      Conversor de coordenadas
    </h3>

    <div class="q-pa-md row">
      <h6
        class="text-center text-weight-bold text-negative"
        style="margin: 2rem 5.2rem"
      >
        Geodésicas-UTM
      </h6>
      <div class="col-md-4 col-sm-1" style="margin: 0 2rem">
        <div class="row">
          <q-input
            rounded
            outlined
            v-model="text"
            label="Grados Latitud"
            class="col-4"
            type="number"
          />

          <q-input
            rounded
            outlined
            v-model="text2"
            label="Minutos Latitud"
            class="col-4"
            type="number"
          />
          <q-input
            rounded
            outlined
            v-model="text3"
            label="Segundos Latitud"
            class="col-4"
            type="number"
          />
        </div>

        <div class="row">
          <q-input
            rounded
            outlined
            v-model="text4"
            label="Grados Longitud"
            class="col-4"
            type="number"
          />
          <q-input
            rounded
            outlined
            v-model="text5"
            label="Minutos Longitud"
            class="col-4"
            type="number"
          />
          <q-input
            rounded
            outlined
            v-model="text6"
            label="Segundos Longitud"
            class="col-4"
            type="number"
          />
        </div>
        <div class="row">
          <q-select
            rounded
            outlined
            v-model="emisferio"
            :options="options"
            label="Hemisferio"
            class="col-12"
          />
        </div>
        <div class="col-md-4 col-sm-1" style="margin: 0 2rem">
          <q-btn
            label="Calcular"
            color="deep-purple-10"
            @click="calcularLambert"
            style="margin: 1rem"
          />
          <q-btn
            label="Limpiar"
            color="negative"
            @click="limpiar2"
            style="margin: 1rem"
          />
          <q-dialog v-model="alert">
            <q-card>
              <q-card-section>
                <div class="text-h6">Respuesta</div>
              </q-card-section>

              <q-card-section class="q-pt-none" v-model="calcular1">
                <p>{{ salida }}</p>
              </q-card-section>

              <q-card-actions align="right">
                <q-btn flat label="OK" color="primary" v-close-popup />
              </q-card-actions>
            </q-card>
          </q-dialog>
        </div>
      </div>
    </div>
    <div class="q-pa-md row">
      <h6
        class="text-center text-weight-bold text-negative"
        style="margin: 2rem 5.2rem"
      >
        UTM-Geodésicas
      </h6>
      <div class="col-md-4 col-sm-1" style="margin: 0 2rem">
        <div class="row">
          <q-input
            rounded
            outlined
            v-model="x"
            label="Longitud"
            class="col-6"
            type="number"
          />

          <q-input
            rounded
            outlined
            v-model="y"
            label="Latitud"
            class="col-6"
            type="number"
          />
        </div>
        <div class="row">
          <q-select
            rounded
            outlined
            v-model="hem"
            :options="options1"
            label="Hemisferio"
            class="col-6"
          />
          <q-input
            rounded
            outlined
            v-model="huso"
            label="Huso"
            class="col-6"
            type="number"
          />
        </div>

        <div class="col-md-4 col-sm-1" style="margin: 0 2rem">
          <q-btn
            label="Calcular"
            color="deep-purple-10"
            @click="calcularGMS"
            style="margin: 1rem"
          />
          <q-btn
            label="Limpiar"
            color="negative"
            @click="limpiar1"
            style="margin: 1rem"
          />
          <q-dialog v-model="alert1">
            <q-card>
              <q-card-section>
                <div class="text-h6">Respuesta</div>
              </q-card-section>

              <q-card-section class="q-pt-none" v-model="calcular1">
                <p>{{ salida2 }}</p>
              </q-card-section>

              <q-card-actions align="right">
                <q-btn flat label="OK" color="primary" v-close-popup />
              </q-card-actions>
            </q-card>
          </q-dialog>
        </div>
      </div>
    </div>
  </q-layout>
</template>

<script>
export default {
  name: "MainLayout",
  data() {
    return {
      text: "",
      text2: "",
      text3: "",
      text4: "",
      text5: "",
      text6: "",
      alert: false,
      alert1: false,
      nombres: false,
      result1: "",
      result: "",
      longitudDev: 0,
      latitudDev: 0,
      emisferio: null,
      salida: "",
      x: "", //Longitud
      y: "", //Latitud
      hem: null,
      huso: "",
      longGr: 0,
      longMin: 0,
      longSeg: 0,
      latGr: 0,
      latMin: 0,
      latSeg: 0,
      salida2: "",
      drawer: false,

      options: ["Norte", "Sur"],
      options1: ["Norte", "Sur"],
    };
  },
  methods: {
    onClick() {
      this.false = true;
    },
    limpiar2() {
      // console.log(Math.round(7.65, 2));
      this.text4 = "";
      this.text5 = "";
      this.text6 = "";
      this.text = "";
      this.text2 = "";
      this.text3 = "";
      this.emisferio = null;
    },
    limpiar1() {
      this.x = "";
      this.y = "";
      this.hem = null;
      this.huso = "";
    },
    calcularLambert() {
      //Latitud
      this.alert = true;
      let grade = this.text;
      let minutes = this.text2;
      let seconds = this.text3;
      let grade1 = this.text4;
      let minutes1 = this.text5;
      let seconds1 = this.text6;
      let gradosDecimalesLatitud = 0;
      let gradosDecimalesLongitud = 0;
      let latRad;
      let longRad;
      let huso;
      let meridianoHuso;
      let deltaLambda;
      let A;
      let xi;
      let eta;
      let ni;
      let zeta;
      let A1;
      let A2;
      let J2;
      let J4;
      let J6;
      let alpha;
      let beta;
      let gamma;
      let bfi;
      let em = this.emisferio;

      // const semiejeMayor = 6378137;
      // const semiejeMenor = 6356752.314;
      const excentricidad = 0.006739497;
      const radioPolar = 6399593.626;
      if (em === "Sur") {
        grade = grade * -1;
        minutes = minutes * -1;
        seconds = seconds * -1;
        grade1 = grade1 * -1;
        minutes1 = minutes1 * -1;
        seconds1 = seconds1 * -1;
      }
      if (
        grade == "" ||
        minutes == "" ||
        seconds == "" ||
        grade1 == "" ||
        minutes1 == "" ||
        seconds1 == "" ||
        em === null
      ) {
        this.salida = "Todos los campos son requeridos";
        console.log("Ahuevo no pude entrar");
      } else {
        //Aqui van las formulas
        //Latitud en grados decimales
        parseFloat(grade);
        parseFloat(grade1);
        parseFloat(minutes);
        parseFloat(minutes1);
        parseFloat(seconds);
        parseFloat(seconds1);
        // console.log("Ahuevo entre");
        gradosDecimalesLatitud = parseFloat(
          parseFloat(grade) +
            parseFloat(minutes) / 60 +
            parseFloat(seconds) / 3600
        );
        // console.log(gradosDecimalesLatitud);
        gradosDecimalesLongitud = parseFloat(
          parseFloat(grade1) +
            parseFloat(minutes1) / 60 +
            parseFloat(seconds1) / 3600
        );
        // console.log(gradosDecimalesLongitud);
        latRad = parseFloat(
          (parseFloat(gradosDecimalesLatitud) * Math.PI) / 180
        );
        // console.log(latRad);
        longRad = parseFloat(
          (parseFloat(gradosDecimalesLongitud) * Math.PI) / 180
        );
        // console.log(longRad);
        huso = parseInt(gradosDecimalesLongitud / 6 + 31);
        // console.log(huso);
        meridianoHuso = parseFloat(6 * huso - 183);
        // console.log(meridianoHuso);
        deltaLambda = parseFloat(
          parseFloat(longRad) - (parseFloat(meridianoHuso) * Math.PI) / 180
        );
        // console.log(deltaLambda);
        A = parseFloat(
          Math.cos(parseFloat(latRad)) * Math.sin(parseFloat(deltaLambda))
        );
        // console.log(A);
        xi = parseFloat(
          0.5 * Math.log((1 + parseFloat(A)) / (1 - parseFloat(A)))
        );
        // console.log(xi);
        eta = parseFloat(
          Math.atan(
            Math.tan(parseFloat(latRad)) / Math.cos(parseFloat(deltaLambda))
          ) - parseFloat(latRad)
        );
        // console.log(eta);
        //Esperame que mi mamá anda ganas de hablar hoy xd
        ni = parseFloat(
          (parseFloat(radioPolar) /
            Math.sqrt(
              1 +
                parseFloat(excentricidad) *
                  (Math.cos(parseFloat(latRad)) * Math.cos(parseFloat(latRad)))
            )) *
            0.9996
        );
        // console.log(ni);
        zeta = parseFloat(
          (parseFloat(excentricidad) / 2) *
            (parseFloat(xi) * parseFloat(xi)) *
            (Math.cos(parseFloat(latRad)) * Math.cos(parseFloat(latRad)))
        );
        // console.log(zeta);
        A1 = parseFloat(Math.sin(2 * parseFloat(latRad)));
        // console.log(A1);
        A2 = parseFloat(
          parseFloat(A1) *
            (Math.cos(parseFloat(latRad)) * Math.cos(parseFloat(latRad)))
        );
        // console.log(A2);
        J2 = parseFloat(parseFloat(latRad) + parseFloat(A1) / 2);
        // console.log(J2);
        J4 = parseFloat((3 * parseFloat(J2) + parseFloat(A2)) / 4);
        // console.log(J4);
        J6 = parseFloat(
          (5 * parseFloat(J4) +
            parseFloat(A2) *
              (Math.cos(parseFloat(latRad)) * Math.cos(parseFloat(latRad)))) /
            3
        );
        // console.log(J6);
        alpha = parseFloat((3 / 4) * parseFloat(excentricidad));
        // console.log(alpha);
        beta = parseFloat((5 / 3) * parseFloat(alpha * alpha));
        // console.log(beta);
        gamma = parseFloat((35 / 27) * parseFloat(alpha * alpha * alpha));
        // console.log(gamma);
        bfi = parseFloat(
          0.9996 *
            parseFloat(radioPolar) *
            (parseFloat(latRad) -
              parseFloat(alpha) * parseFloat(J2) +
              parseFloat(beta) * parseFloat(J4) -
              parseFloat(gamma) * parseFloat(J6))
        );
        // console.log(bfi);
        this.longitudDev = parseFloat(
          parseFloat(xi) * parseFloat(ni) * (1 + parseFloat(zeta) / 3) + 500000
        );

        if (em === "Norte") {
          this.latitudDev = parseFloat(
            parseFloat(eta) * parseFloat(ni) * (1 + parseFloat(zeta)) +
              parseFloat(bfi)
          );
        } else {
          this.latitudDev = parseFloat(
            parseFloat(eta) * parseFloat(ni) * (1 + parseFloat(zeta)) +
              parseFloat(bfi) +
              10000000
          );
        }
        this.salida =
          "Longitud: " + this.longitudDev + " \nLatitud: " + this.latitudDev;
        this.text = "";
        this.text1 = "";
        this.text2 = "";
        this.text3 = "";
        this.text4 = "";
        this.text5 = "";
        this.text6 = "";
      }
    },
    calcularGMS() {
      this.alert1 = true;
      // console.log(Math.round(0.56, 2));
      const excentricidad = 0.006739497;
      const radioPolar = 6399593.626;
      let xl = this.x;
      let yla = this.y;
      let hemis = this.hem;
      let hus = this.huso;
      let yalSurdelEcuador;
      let fi;
      let ni;
      let a;
      let b;
      let A1;
      let A2;
      let J2;
      let J4;
      let J6;
      let alpha;
      let beta;
      let gamma;
      let bfi;
      let zeta;
      let xi;
      let eta;
      let senHxi;
      let deltaLambda;
      let tau;
      let fiRad;
      let meridianoCentral;
      let latitud;
      let longitud;
      let grados;
      let minutos;
      let segundos;

      // console.log(this.salida2);

      if (xl === "" || yla === "" || hemis === null || hus === "") {
        this.salida2 = "Todos los campos deben ser llenados";
      } else {
        parseFloat(xl);
        parseFloat(yla);
        parseFloat(hus);
        if (hemis === "Norte") {
          yalSurdelEcuador = parseFloat(yla);
        } else {
          yalSurdelEcuador = parseFloat(yla) - 10000000;
        }
        // console.log(yalSurdelEcuador);
        fi = parseFloat(parseFloat(yalSurdelEcuador) / (6366197.724 * 0.9996));
        // console.log(fi);
        ni = parseFloat(
          (parseFloat(radioPolar) /
            Math.sqrt(
              1 +
                parseFloat(excentricidad) *
                  (Math.cos(parseFloat(fi)) * Math.cos(parseFloat(fi)))
            )) *
            0.9996
        );
        // console.log(ni);
        a = parseFloat((parseFloat(xl) - 500000) / parseFloat(ni));
        // console.log(a);
        A1 = parseFloat(Math.sin(2 * parseFloat(fi)));
        // console.log(A1);
        A2 = parseFloat(
          parseFloat(A1) * (Math.cos(parseFloat(fi)) * Math.cos(parseFloat(fi)))
        );
        // console.log(A2);
        J2 = parseFloat(parseFloat(fi) + parseFloat(A1) / 2);
        // console.log(J2);

        J4 = parseFloat((3 * parseFloat(J2) + parseFloat(A2)) / 4);
        // console.log(J4);
        J6 = parseFloat(
          (5 * parseFloat(J4) +
            parseFloat(A2) *
              (Math.cos(parseFloat(fi)) * Math.cos(parseFloat(fi)))) /
            3
        );
        // console.log(J6);
        alpha = parseFloat((3 / 4) * parseFloat(excentricidad));
        // console.log(alpha);
        beta = parseFloat((5 / 3) * parseFloat(alpha * alpha));
        // console.log(beta);
        gamma = parseFloat((35 / 27) * parseFloat(alpha * alpha * alpha));
        // console.log(gamma);
        bfi = parseFloat(
          0.9996 *
            parseFloat(radioPolar) *
            (parseFloat(fi) -
              parseFloat(alpha) * parseFloat(J2) +
              parseFloat(beta) * parseFloat(J4) -
              parseFloat(gamma) * parseFloat(J6))
        );
        // console.log(bfi);
        b = parseFloat(
          (parseFloat(yalSurdelEcuador) - parseFloat(bfi)) / parseFloat(ni)
        );
        // console.log(b);
        zeta = parseFloat(
          ((parseFloat(excentricidad) * parseFloat(a * a)) / 2) *
            Math.cos(parseFloat(fi)) *
            Math.cos(parseFloat(fi))
        );
        // console.log(zeta);
        xi = parseFloat(parseFloat(a) * (1 - parseFloat(zeta) / 3));
        // console.log(xi);
        eta = parseFloat(
          parseFloat(b) * (1 - parseFloat(zeta)) + parseFloat(fi)
        );
        // console.log(eta);
        senHxi = parseFloat(
          (Math.pow(2.718282, parseFloat(xi)) -
            Math.pow(2.718282, parseFloat(-xi))) /
            2
        );
        // console.log(senHxi);
        deltaLambda = parseFloat(
          Math.atan(parseFloat(parseFloat(senHxi) / Math.cos(parseFloat(eta))))
        );
        // console.log(deltaLambda);
        tau = parseFloat(
          Math.atan(
            Math.cos(parseFloat(deltaLambda)) * Math.tan(parseFloat(eta))
          )
        );
        // console.log(tau);
        fiRad = parseFloat(
          parseFloat(fi) +
            (1 +
              parseFloat(excentricidad) *
                (Math.cos(parseFloat(fi)) * Math.cos(parseFloat(fi))) -
              (3 / 2) *
                (parseFloat(excentricidad) * Math.sin(parseFloat(fi))) *
                Math.cos(parseFloat(fi)) *
                (parseFloat(tau) - parseFloat(fi))) *
              (parseFloat(tau) - parseFloat(fi))
        );
        // console.log(fiRad);
        meridianoCentral = parseFloat(6 * parseFloat(hus) - 183);
        // console.log(meridianoCentral);
        latitud = parseFloat((parseFloat(fiRad) / Math.PI) * 180);
        // console.log(latitud);
        // console.log("La latitud :) :" + latitud);
        longitud = parseFloat(
          (parseFloat(deltaLambda) / Math.PI) * 180 +
            parseFloat(meridianoCentral)
        );
        // console.log(meridianoCentral);
        console.log(longitud);
        // console.log(typeof longitud);
        this.latGr = Math.round(parseInt(latitud));
        this.longGr = Math.round(parseInt(longitud));
        // console.log(typeof this.longGr);
        this.latMin = Math.round(
          parseInt((parseFloat(latitud) - parseFloat(this.latGr)) * 60)
        );
        this.longMin = Math.round(
          parseInt((parseFloat(longitud) - parseFloat(this.longGr)) * 60)
        );
        this.latSeg = parseFloat(
          ((parseFloat(latitud) - parseFloat(this.latGr)) * 60 - this.latMin) *
            60
        );
        this.latSeg = this.latSeg;
        this.longSeg = parseFloat(
          ((parseFloat(longitud) - parseFloat(this.longGr)) * 60 -
            parseFloat(this.longMin)) *
            60
        );
        this.longSeg = this.longSeg;
        // console.log(typeof this.longSeg, typeof this.longMin, typeof this.longGr);
        if (latitud < 0) {
          //Se van a contatenar en positivo S hay que cambiarles signo
          this.latGr = parseInt(this.latGr * -1);
          this.latMin = parseInt(this.latMin * -1);
          this.latSeg = parseFloat(this.latSeg * -1);
          if (longitud < 0) {
            this.longGr = parseInt(this.longGr * -1);
            this.longMin = parseInt(this.longMin * -1);
            this.longSeg = parseFloat(this.longSeg * -1);
            this.salida2 =
              "Latitud: " +
              this.latGr +
              "°" +
              this.latMin +
              "'" +
              this.latSeg.toFixed(2) +
              '" S' +
              "Longitud: " +
              this.longGr +
              "°" +
              this.longMin +
              "'" +
              this.longSeg.toFixed(2) +
              '" W';
          } else {
            this.salida2 =
              "Latitud: " +
              this.latGr +
              "°" +
              this.latMin +
              "'" +
              this.latSeg.toFixed(2) +
              '" S' +
              "Longitud: " +
              this.longGr +
              "°" +
              this.longMin +
              "'" +
              this.longSeg.toFixed(2) +
              '" E';
          }
        } else {
          if (longitud < 0) {
            this.longGr = parseInt(this.longGr * -1);
            this.longMin = parseInt(this.longMin * -1);
            this.longSeg = parseFloat(this.longSeg * -1);
            this.salida2 =
              "Latitud: " +
              this.latGr +
              "°" +
              this.latMin +
              "'" +
              this.latSeg.toFixed(2) +
              '" N' +
              "Longitud: " +
              this.longGr +
              "°" +
              this.longMin +
              "'" +
              this.longSeg.toFixed(2) +
              '" W';
          } else {
            this.salida2 =
              "Latitud: " +
              this.latGr +
              "°" +
              this.latMin +
              "'" +
              this.latSeg.toFixed(2) +
              '" N ' +
              "Longitud: " +
              this.longGr +
              "°" +
              this.longMin +
              "'" +
              this.longSeg.toFixed(2) +
              '" E ';
          }
        }
      }
      // this.x = "";
      // this.y = "";
      // this.hem = null;
      // this.huso = "";
    },
  },
};
</script>
