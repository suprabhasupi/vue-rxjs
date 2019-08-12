<template>
  <section>
	<button v-stream:click="click$">Click</button>
	<p>{{random$}}</p>
  </section>
</template>

<script>
// import { Observable } from 'rxjs'
import { interval } from 'rxjs'
import { map } from "rxjs/operators";

export default {
	domStreams:["click$"],
  subscriptions() {
  	const random$ = this.click$.pipe(map(() => Math.random()))
    return {
      random$
    }
  }
};
</script>
