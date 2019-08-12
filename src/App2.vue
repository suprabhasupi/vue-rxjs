<template>
  <section>
    <!-- <h1>{{interval$}}</h1> -->
    <h1>{{timeOne$}}</h1>
    <h1>{{timeTwo$}}</h1>
  </section>
</template>

<script>
// import { Observable } from 'rxjs'
import { interval } from 'rxjs'
import { map } from "rxjs/operators";

export default {
  subscriptions() {
  	const interval$ = interval(1000)
  	const timeOne$ = interval$.pipe(map(i => i * 2))
  	const timeTwo$ = interval$.pipe(map(i => i * 3))
    return {
      timeOne$,
      timeTwo$
    }
  }
};
</script>
