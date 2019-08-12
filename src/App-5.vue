<template>
  <section>
	<button v-stream:click="click$">Click</button>
	<p>{{showName$}}</p>
  </section>
</template>

<script>
import { from } from "rxjs";
import { pluck,switchMap } from "rxjs/operators";

export default {
	domStreams:["click$"],
  subscriptions() {
  	const people$ = from(this.$http.get("https://starwars.egghead.training/people/2")).pipe(pluck("data", "name"))
  	const showName$ = this.click$.pipe(switchMap(() => {
  		return people$
  	}))

    return {
      people$,
      showName$
    }
  }
};
</script>
