<template>
  <div
    class="text-center bg-light border border-dark rounded rounded-sm"
    style="width: 100%; max-width: 330px; padding: 15px; margin: 0 auto"
  >
    <div class="m-2 form-group">
      <label for="polinomio">Polinomio</label>
      <input
        type="text"
        class="form-control"
        id="f_expr"
        placeholder="Polinomio"
        v-model="f_expr"
      />
    </div>
    <div class="m-2 form-group">
      <label for="lower_limit">Limite Inferior</label>
      <input
        type="number"
        class="form-control"
        id="lower_limit"
        placeholder="Limite Inferior"
        v-model="x_lower"
      />
    </div>
    <div class="m-2 form-group">
      <label for="upper_limit">Limite Superior</label>
      <input
        type="number"
        class="form-control"
        id="upper_limit"
        placeholder="Limite Superior"
        v-model="x_upper"
      />
    </div>
    <div class="m-2 form-group">
      <label for="max_error">Error Maximo</label>
      <input
        type="number"
        class="form-control"
        id="max_error"
        placeholder="Error Maximo"
        v-model="max_error"
      />
    </div>
    <button class="mt-3 btn btn-success" @click="solve()">Resolver</button>
    <hr class="m-4" />
    <div class="form-group">
      <label for="root">Raiz</label>
      <input
        type="number"
        class="form-control"
        id="root"
        placeholder="Raiz"
        v-model="x"
      />
    </div>
  </div>
</template>

<script>
import { create, all } from "mathjs";

const math = create(all, {});

export default {
  name: "App",
  components: {},
  data() {
    return {
      x: 0,
    };
  },
  methods: {
    f(x) {
      return math.evaluate(this.f_expr, { x: x });
    },
    bisection() {
      return (this.x_lower + this.x_upper) / 2;
    },
    error(cur, prev) {
      return (Math.abs(cur - prev) / cur) * 100;
    },
    solve() {
      this.max_error = Math.max(this.max_error, 5);
      if (this.f(this.x_lower) * this.f(this.x_upper) < 0) {
        // while (this.f(this.x_lower) * this.f(this.x) !== 0) {
        // while (this.f() * this.f(this.x) !== 0) {
        let prev = 100;
        while (this.error(this.x, prev) > this.max_error) {
          prev = this.x;
          this.x = this.bisection();
          if (this.f(this.x_lower) * this.f(this.x) > 0) {
            this.x_upper = this.x;
          } else if (this.f(this.x_lower) * this.f(this.x) < 0) {
            this.x_lower = this.x;
          }
        }
      }
    },
  },
};

// console.log(math.evaluate("2x", { x: 3 }));
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

ul {
  list-style: none;
}
</style>
