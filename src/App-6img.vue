<template>
  <section>
	<button v-stream:click="click$">Click</button>
	<p>{{name$}}</p>
	<!-- <p>{{image$}}</p> -->
	<img :src="image$">
  </section>
</template>

<script>
import { from } from "rxjs";
import { pluck,switchMap,map } from "rxjs/operators";

export default {
	domStreams:["click$"],
  subscriptions() {
  	const person$ = from(this.$http.get("https://starwars.egghead.training/people/2")).pipe(pluck("data"))
  	const showPerson$ = this.click$.pipe(switchMap(() => {
  		return person$
  	}))
  	const name$ = showPerson$.pipe(pluck("name"))
  	const image$ = showPerson$.pipe(pluck("image")).pipe(map(image => `https://starwars.egghead.training/${image}`))

    return {
      name$,
      image$
    }
  }
};
</script>
