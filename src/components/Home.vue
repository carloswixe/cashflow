<template>
  <Layout>
    <template #header>
      <Header></Header>
    </template>
    <template #resume>
      <Resume
        :total-label="'Ahorro total'"
        :label="label"
        :total-amount="totalAmount"
        :amount="amount"
      >
        <template #graphic>
          <Graphic :amounts="amounts" @select="select"></Graphic>
        </template>
        <template #action> <Action @create="create"></Action> </template>
      </Resume>
    </template>
    <template #movements>
      <Movements @remove="remove" :movements="movements" />
    </template>
  </Layout>
</template>

<script>
import Layout from "./Layout.vue";
import Header from "./Header.vue";
import Resume from "./Resume/index.vue";
import Movements from "./Movements/index.vue";
import Action from "./Action.vue";
import Graphic from "./Resume/Graphic.vue";

export default {
  components: {
    Layout,
    Header,
    Resume,
    Movements,
    Action,
    Graphic,
  },
  data() {
    return {
      label: null,
      amount: null,
      movements: [
        {
          id: 0,
          title: "Movimiento 1",
          description: "Lorem ipsum dolor sit amet",
          amount: 100,
          time: new Date("10-10-2024"),
        },
        {
          id: 1,
          title: "Movimiento 2",
          description: "Lorem ipsum dolor sit amet",
          amount: 200,
          time: new Date("10-10-2024"),
        },
        {
          id: 2,
          title: "Movimiento 3",
          description: "Lorem ipsum dolor sit amet",
          amount: 500,
          time: new Date("10-10-2024"),
        },
        {
          id: 3,
          title: "Movimiento 4",
          description: "Lorem ipsum dolor sit amet",
          amount: 200,
          time: new Date("10-10-2024"),
        },
        {
          id: 4,
          title: "Movimiento 5",
          description: "Lorem ipsum dolor sit amet",
          amount: -400,
          time: new Date("10-10-2024"),
        },
        {
          id: 5,
          title: "Movimiento 6",
          description: "Lorem ipsum dolor sit amet",
          amount: -600,
          time: new Date("10-10-2024"),
        },
        {
          id: 6,
          title: "Movimiento 7",
          description: "Lorem ipsum dolor sit amet",
          amount: -300,
          time: new Date("10-10-2024"),
        },
        {
          id: 7,
          title: "Movimiento 8",
          description: "Lorem ipsum dolor sit amet",
          amount: 100,
          time: new Date("10-10-2024"),
        },
        {
          id: 8,
          title: "Movimiento 9",
          description: "Lorem ipsum dolor sit amet",
          amount: 300,
          time: new Date("10-10-2024"),
        },
        {
          id: 9,
          title: "Movimiento 10",
          description: "Lorem ipsum dolor sit amet",
          amount: 500,
          time: new Date("10-10-2024"),
        },
      ],
    };
  },
  computed: {
    amounts() {
      debugger;
      const lastDays = this.movements
        .filter((m) => {
          const today = new Date();
          const oldDate = today.setDate(today.getDate() - 100);
          m.time;
          return m.time > oldDate;
        })
        .map((m) => m.amount);

      return lastDays.map((m, i) => {
        const lastMovements = lastDays.slice(0, i + 1);

        return lastMovements.reduce((suma, movement) => {
          return suma + movement;
        }, 0);
      });
    },
    totalAmount() {
      return this.movements.reduce((sum, m) => {
        return sum + m.amount;
      }, 0);
    },
  },
  methods: {
    create(movement) {
      movement.id = this.movements.length;
      this.movements.push(movement);
      this.save();
    },
    remove(id) {
      const index = this.movements.findIndex((m) => m.id === id);
      this.movements.splice(index, 1);
      this.save();
    },
    save() {
      localStorage.setItem("movements", JSON.stringify(this.movements));
    },
    select(el) {
      console.log(el);
      this.amount = el;
    },
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem("movements"));
    if (Array.isArray(movements)) {
      this.movements = movements?.map((m) => {
        return { ...m, time: new Date(m.time) };
      });
    }
  },
};
</script>
