<template>
  <section>
	<button v-stream:click="click$">Click</button>
	<p>{{name$}}</p>
	<img v-stream:error="imageError$" :src="image$">
  </section>
</template>

<script>
import { from, merge } from "rxjs";
import { pluck,switchMap,map, mapTo } from "rxjs/operators";

export default {
	domStreams:["click$", "imageError$"],
  subscriptions() {
  	const person$ = from(this.$http.get("https://starwars.egghead.training/people/2")).pipe(pluck("data"))
  	const showPerson$ = this.click$.pipe(switchMap(() => {
  		return person$
  	}))
  	const name$ = showPerson$.pipe(pluck("name"))
  	const loadImage$ = showPerson$.pipe(pluck("image")).pipe(map(image => `https://starwars.egghead.training/${image}`))
  	const failImage$ = this.imageError$.pipe(mapTo(`https://images.pexels.com/photos/736230/pexels-photo-736230.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500`))
  	const image$ = merge(loadImage$, failImage$)

    return {
      name$,
      image$
    }
  }
};
</script>
