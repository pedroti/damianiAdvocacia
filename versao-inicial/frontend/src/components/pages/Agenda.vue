<template>
  <div class="agenda">
    <PageTitle icon="fa fa-calendar" main="Agenda" />
    <b-form>
      <input id="reminder-id" type="hidden" v-model="reminder.id" />
      <b-row>
        <b-col md="7" sm="12">
          <b-form-group label="Cliente:" label-for="customer">
            <b-form-select id="customer" :options="customers" v-model="reminder.customer" />
          </b-form-group>
        </b-col>
      </b-row>
      <b-row>
        <b-col md="7" sm="12">
          <b-form-group label="Descrição:" label-for="description">
            <b-form-input
              id="description"
              type="text"
              v-model="reminder.description"
              placeholder="Descrição"
            />
          </b-form-group>
        </b-col>
      </b-row>
      <b-row>
        <b-col md="7" sm="12">
          <b-form-group label="Data:" label-for="date">
            <b-form-datepicker
              id="date"
              v-model="reminder.reminderDate"
              class="mb-2"
              locale="pt-BR"
            ></b-form-datepicker>
          </b-form-group>
        </b-col>
      </b-row>
      <b-row>
        <b-col md="7" sm="12">
          <b-form-group label="Hora:" label-for="hora">
            <b-form-timepicker
              id="hora"
              v-model="reminder.reminderHour"
              class="mb-2"
              locale="pt-BR"
              label-close-button="X"
            ></b-form-timepicker>
          </b-form-group>
        </b-col>
      </b-row>

      <b-button variant="primary" @click="save">Salvar</b-button>
      <b-button class="ml-2" @click="reset">Limpar</b-button>
    </b-form>
  </div>
</template>

<script>
import { baseApiUrl, showError } from "@/global";
import axios from "axios";
import PageTitle from "../template/PageTitle";
import { mapState } from "vuex";

export default {
  name: "Agenda",
  components: { PageTitle },
  computed: mapState(["user"]),
  data: function() {
    return {
      mode: "save",
      customers: [],
      reminder: {}
    };
  },
  methods: {
    reset() {
      this.reminder = {};
    },
    save() {
      const method = "post";
      axios[method](`${baseApiUrl}/reminders`, this.reminder)
        .then(() => {
          this.$toasted.global.defaultSuccess();
          this.reset();
        })
        .catch(showError);
    },
    loadCustomers() {
      const url = `${baseApiUrl}/customers`;
      axios.get(url).then(res => {
        this.customers = res.data.map(customer => {
          return {
            value: customer.id,
            text: `${customer.cpfCnpj} - ${customer.name}`
          };
        });
      });
    },
    onContext(ctx) {
      this.reminder.reminderDate = ctx.selectedYMD;
    }
  },
  mounted() {
    this.loadCustomers();
  }
};
</script>

<style>
</style>