<template>
  <section>
	<button v-stream:click="click$">Click</button>
	<p>{{people$}}</p>
  </section>
</template>

<script>
import { from } from "rxjs";
import { pluck } from "rxjs/operators";

export default {
	domStreams:["click$"],
  subscriptions() {
  	const people$ = from(this.$http.get("https://starwars.egghead.training/people/2")).pipe(pluck("data", "name"))

    return {
      people$
    }
  }
};
</script>
