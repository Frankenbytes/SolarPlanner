<template>
  <div class="card">
    <div class="card-content">
      <div class="content">
        <h2>Solar calculator</h2>
        <div v-if="!isCalculated" class="control">
          <button
              v-if="!disclaimer"
              class="button is-info"
              @click="showDisclaimer()"
          >
            How to use
          </button>
        </div>
        <button
            v-if="disclaimer"
            class="button is-danger"
            @click="showDisclaimer()"
        >
          X
        </button>
        <div v-if="disclaimer">
          <p>
            Type in the specs of your preferred solar-panels,
            charge-controllers, batteries and inverter.
          </p>
          <p>Once completed, press the "calculate" button.</p>
          <p>
            The calculator will tell you if and how many of your panels should
            be set in series or parallel, as well as how many panels should be
            attached one controller.
          </p>
          <p>
            The calculator also tells you how to put connect your batteries to
            your controllers, as well how to put them on your inverter.
          </p>
          <p class="disclaimer">
            This is a calculator, use results at your own risk!
          </p>
        </div>
      </div>
    </div>
  </div>
  <form v-if="!isCalculated" @submit.prevent="handleSubmit">
    <div class="card">
      <div class="card-content columns">
        <div class="content column">
          <h4 class="heading mb-6">Panels</h4>
          <div class="field mb-6">
            <label class="label mb-3"> Rated Watt </label>
            <div class="control">
              <input
                  v-model="panel_watt"
                  class="input"
                  type="number"
                  placeholder="Rated Watt of your Panels (ex. 550)"
                  required
              />
            </div>
            <label class="label mt-3 mb-3"> Rated Volt </label>
            <div class="control">
              <input
                  v-model="panel_volt"
                  class="input"
                  type="number"
                  placeholder="Rated volt of your panels (ex. 41)"
                  required
              />
            </div>
            <label class="label mt-3 mb-3"> Rated Ampere </label>
            <div class="control">
              <input
                  v-model="panel_ampere"
                  class="input"
                  type="number"
                  placeholder="Rated ampere of your panels (ex. 13)"
                  required
              />
            </div>
            <label class="label mt-3 mb-3"> Amount of panels </label>
            <div class="control">
              <input
                  v-model="panel_count"
                  class="input"
                  type="number"
                  placeholder="Amount of panels planned (ex. 10)"
                  required
              />
            </div>
          </div>
        </div>
        <div class="content column">
          <img
              src="@/assets/SolarPanel.png"
              class="pic"
              alt="grafic of a solar panel"
          />
        </div>
      </div>
      <div class="card-content columns">
        <div class="content column">
          <img
              src="@/assets/chargeController.png"
              class="pic"
              alt="grafic of a solar panel"
          />
        </div>
        <div class="content column">
          <h4 class="heading mb-6">Controller</h4>
          <div class="field mb-6">
            <label class="label mt-3 mb-3"> Max. Watt </label>
            <div class="control">
              <input
                  v-model="controller_watt"
                  class="input"
                  type="number"
                  placeholder="Maximum Watt of your Controller (ex. 3600)"
                  required
              />
            </div>
            <label class="label mt-3 mb-3"> Max. Volt </label>
            <div class="control">
              <input
                  v-model="controller_volt"
                  class="input"
                  type="number"
                  placeholder="Maximum volt of your controller (ex. 150)"
                  required
              />
            </div>
            <label class="label mt-3 mb-3"> Max Ampere </label>
            <div class="control">
              <input
                  v-model="controller_ampere"
                  class="input"
                  type="number"
                  placeholder="Maximum ampere of your controller (ex. 60)"
                  required
              />
            </div>
            <label class="label mt-3 mb-3"> Volt output </label>
            <div class="control">
              <input
                  v-model="controller_volt_output"
                  class="input"
                  type="number"
                  placeholder="Chosen volt output of controller (ex. 48)"
                  required
              />
            </div>
          </div>
        </div>
      </div>
      <div class="card-content columns">
        <div class="content column">
          <h4 class="heading mb-6">Batteries</h4>
          <div class="field mb-6">
            <label class="label mt-3 mb-3"> Rated Volt </label>
            <div class="control">
              <input
                  v-model="battery_volt"
                  class="input"
                  type="number"
                  placeholder="Rated volt of your battery (ex. 12)"
                  required
              />
            </div>
            <label class="label mt-3 mb-3"> Amount of batteries </label>
            <div class="control">
              <input
                  v-model="battery_count"
                  class="input"
                  type="number"
                  placeholder="Amount of batteries planned (ex. 4)"
                  required
              />
            </div>
          </div>
        </div>
        <div class="content column">
          <img
              src="@/assets/battery.png"
              class="pic"
              alt="grafic of a solar battery"
          />
        </div>
      </div>
      <div class="card-content columns">
        <div class="content column">
          <img
              src="@/assets/inverter.png"
              class="pic"
              alt="grafic of a solar panel"
          />
        </div>
        <div class="content column">
          <h4 class="heading mb-6">Inverter</h4>
          <div class="field mb-6">
            <label class="label mt-3 mb-3"> Rated volt input </label>
            <div class="control">
              <input
                  v-model="inverter_volt"
                  class="input"
                  type="number"
                  placeholder="Rated input volt of your inverter (ex. 48)"
                  required
              />
            </div>
          </div>
        </div>
      </div>
      <div class="card-content columns">
        <div class="content column">
          <div class="control">
            <button
                type="submit"
                v-if="!isCalculated"
                class="button is-link"
                @submit.prevent="calc()"
            >
              Calculate
            </button>
          </div>
        </div>
        <div class="content column">
          <img
              src="@/assets/circuit.png"
              class="pic"
              alt="grafic of a solar circuit"
          />
        </div>
      </div>
    </div>
  </form>
  <div v-if="isCalculated" class="card">
    <div class="card-content">
      <div class="content">
        <h4>Result</h4>
        <!-- table -->
        <table class="table">
          <thead>
          <tr>
            <th>Designation</th>
            <th>Value</th>
          </tr>
          </thead>
          <tfoot>
          <tr>
            <th>Error watt</th>
            <th>{{ error.watt }}</th>
          </tr>
          <tr>
            <th>Error ampere</th>
            <th>{{ error.ampere }}</th>
          </tr>
          <tr>
            <th>Error volt</th>
            <th>{{ error.volt }}</th>
          </tr>
          <tr>
            <th>Error inv</th>
            <th>{{ error.inv }}</th>
          </tr>
          <tr>
            <th>Watt: number of panels per Controller</th>
            <th>{{ watt_num_panels_per_controller }}</th>
          </tr>
          <tr>
            <th>volt: number of panels per Controller in series</th>
            <th>{{ volt_num_panels_per_controller_series }}</th>
          </tr>
          <tr>
            <th>
              ampere: number of
              {{ volt_num_panels_per_controller_series }} panels in series set
              parallel per Controller
            </th>
            <th>{{ ampere_num_panel_series_per_controller }}</th>
          </tr>
          <tr>
            <th>Volt: Controller and Battery are compatible:</th>
            <th>{{ controller_battery_compatibility }}</th>
          </tr>
          <tr>
            <th>Volt: Inverter and Battery are compatible:</th>
            <th>{{ inverter_battery_compatibility }}</th>
          </tr>
          <tr>
            <th>
              Batteries: Batteries set in series to match inverter input:
            </th>
            <th>{{ battery_set }}</th>
          </tr>
          </tfoot>
        </table>
        <div class="control">
          <button
              v-if="isCalculated"
              class="button is-link is-danger"
              @click="reset()"
          >
            Reset
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',
  components: {
  },
    data() {
      return {
        disclaimer: false,
        isCalculated: false,
        panel_watt: "",
        panel_volt: "",
        panel_ampere: "",
        panel_count: "",
        controller_watt: "",
        controller_volt: "",
        controller_ampere: "",
        controller_volt_output: "",
        battery_volt: "",
        battery_count: "",
        inverter_volt: "",
        max_panel_watt: "",
        error: {},
        watt_num_panels_per_controller: "",
        volt_num_panels_per_controller_series: "",
        ampere_num_panel_series_per_controller: "",
        controller_battery_compatibility: "",
        inverter_battery_compatibility: "",
        battery_set: "",
      };
    },
    methods: {
      showDisclaimer() {
        this.disclaimer = !this.disclaimer;
      },
      reset() {
        this.isCalculated = !this.isCalculated;
        window.location.reload();
      },
      handleSubmit() {
        this.isCalculated = true;
        if (this.controller_watt < this.panel_watt) {
          this.error.watt =
              "Your Controller got a too small watt rating for your panels!";
        }
        if (this.controller_ampere < this.panel_ampere) {
          this.error.ampere =
              "Your Controller got a too small ampere rating for your panels!";
        }
        if (this.controller_volt < this.panel_volt) {
          this.error.volt =
              "Your Controller got a too small volt rating for your panels!";
        }
        if (this.inverter_volt < this.battery_volt) {
          this.error.inv =
              "Your Inverter is not compatible with your battery (banks) because of too small inverter input voltage. Choose battery (banks) with lower voltage or inverter with higher input voltage!";
        }
        //checking if chosen volt output of controller can be matched by batteries
        if (this.controller_volt_output < this.battery_volt) {
          this.error.inv =
              "The chosen controller volt output can't be matched with your batteries. Choose higher controller voltage output, controller with higher voltage output possibilities or batteries with smaller rated voltage!";
        }
        //calculating how many panels can be attached to one controller without exceeding max watt rating of controller
        let controller_watt = this.controller_watt;
        while (controller_watt >= this.panel_watt) {
          controller_watt = controller_watt - this.panel_watt;
          this.watt_num_panels_per_controller++;
        }
        //calculating how many panels as series can be attached to one controller without exceeding max volt rating of controller
        let controller_volt = this.controller_volt;
        while (controller_volt >= this.panel_volt) {
          controller_volt = controller_volt - this.panel_volt;
          this.volt_num_panels_per_controller_series++;
        }
        //calculating how many panel series as parallel can be attached to one controller without exceeding max ampere rating of controller
        let controller_ampere = this.controller_ampere;
        while (controller_ampere >= this.panel_ampere) {
          controller_ampere = controller_ampere - this.panel_ampere;
          this.ampere_num_panel_series_per_controller++;
        }
        //checking if controller volt output is compatible with battery current voltage
        let controller_battery_remainder =
            this.controller_volt_output % this.battery_volt;
        if (controller_battery_remainder > 0) {
          this.controller_battery_compatibility = "false";
        } else {
          this.controller_battery_compatibility = "true";
        }
        //checking if inverter volt input is compatible with battery current voltage
        let inverter_battery_remainder = this.inverter_volt % this.battery_volt;
        if (inverter_battery_remainder > 0) {
          this.inverter_battery_compatibility = "false";
        } else {
          this.inverter_battery_compatibility = "true";
        }
        // if controller_battery_compatibility = true
        //show actual results
        //calculating how batteries need to be set in series
        if (this.controller_battery_compatibility) {
          this.battery_set = this.inverter_volt / this.battery_volt;
        }
        //else return error message
      },
    },
  };
</script>



<style scoped></style>

