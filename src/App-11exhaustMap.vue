<template>
  <section>
	<button :disabled="disabled$" v-stream:click="click$">{{buttonText$}}</button>
	<p>{{name$}}</p>
	<img v-stream:error="imageError$" :src="image$">
  </section>
</template>

<script>
import { from, merge } from "rxjs";
import { pluck,switchMap,map, mapTo,catchError,share,startWith,exhaustMap } from "rxjs/operators";

export default {
	domStreams:["click$", "imageError$"],
  subscriptions() {
  	const createLoader = starwarsUrl => from(this.$http.get(starwarsUrl)).pipe(pluck("data"))

  	const showPerson$ = this.click$.pipe(mapTo("https://starwars.egghead.training/people/2"),
  		exhaustMap(createLoader),
  		catchError(err =>
  			// of({name: "something went worng"})
        	createLoader("https://starwars.egghead.training/people/2")
      	),
      	share()
      	);

  	const disabled$ = merge(
  		this.click$.pipe(mapTo(true)),
  		showPerson$.pipe(mapTo(false)),
  		).pipe(startWith(false))

  	const buttonText$ = disabled$.pipe(
      map(bool => (bool ? "Loading..." : "Load"))
    );

  	const name$ = showPerson$.pipe(pluck("name"))
  	const loadImage$ = showPerson$.pipe(pluck("image")).pipe(map(image => `https://starwars.egghead.training/${image}`))
  	const failImage$ = this.imageError$.pipe(mapTo(`https://images.pexels.com/photos/736230/pexels-photo-736230.jpeg?auto=compress&cs=tinysrgb&dpr=1&w=500`))
  	const image$ = merge(loadImage$, failImage$)

    return {
      name$,
      image$,
      buttonText$,
      disabled$
    }
  }
};
</script>


<!--  The switchMap operator is often referenced as the default operator when you need to switch to another stream (which happens when clicking a button switches to loading data). But in the scenario where you want the first stream (the button click) to "pause" until the second stream (the data loads) completes, exhaustMap is the proper operator to use. -->